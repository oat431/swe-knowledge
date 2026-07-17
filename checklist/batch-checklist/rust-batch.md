# Rust Batch Processing Checklist

> Rust-specific companion to the general [Batch Processing Checklist](batch.md).
> Tokio async runtime, zero-cost abstractions, fearless concurrency. No GC pauses, predictable memory.
> Rust 2024 edition, tokio, sqlx. Last updated: 2026-07-17

---

## 1. Project Setup

- [ ] **Rust toolchain** — `rustup default stable`. `cargo new job-name`. Edition 2024.
- [ ] **Dependencies** — `Cargo.toml`:

```toml
[dependencies]
tokio = { version = "1", features = ["full"] }
sqlx = { version = "0.8", features = ["runtime-tokio", "postgres", "chrono", "uuid"] }
serde = { version = "1", features = ["derive"] }
serde_json = "1"
tracing = "0.1"
tracing-subscriber = { version = "0.3", features = ["json", "env-filter"] }
config = "0.14"                    # config loading
chrono = { version = "0.4", features = ["serde"] }
uuid = { version = "1", features = ["v7", "serde"] }
tokio-graceful-shutdown = "0.15"   # signal handling
thiserror = "2"                    # error enums
anyhow = "1"                       # error propagation in main

[dev-dependencies]
tokio-test = "0.4"
testcontainers = "0.23"
```

- [ ] **Clippy strict** — `cargo clippy -- -D warnings -W clippy::pedantic`. CI gate. No dead code, no unused imports, no implicit conversions.
- [ ] **`rustfmt`** — `rustfmt.toml`. Consistent style. `edition = "2024"`.
- [ ] **Binary per job** — `[[bin]]` sections in `Cargo.toml` or separate crates in workspace. Each job compiles to its own binary.

```toml
[[bin]]
name = "order-sync"
path = "src/bin/order_sync.rs"

[[bin]]
name = "report-gen"
path = "src/bin/report_gen.rs"
```

- [ ] **Workspace for multi-job projects** — Shared library crate (`batch-core`) + binary crates per job. Compile once, share types and utilities.

```
batch-jobs/
├── Cargo.toml          ← workspace
├── crates/
│   ├── batch-core/     ← shared: traits, checkpoint, metrics, error types
│   ├── order-sync/     ← binary: order sync job
│   └── report-gen/     ← binary: report generation job
```

---

## 2. Project Structure (Single Job)

```
src/
├── main.rs             ← Bootstrap. Config, DB pool, signal handling, run job.
├── config.rs           ← Config struct + env loading
├── error.rs            ← Error enum (thiserror)
├── job.rs              ← Job orchestration (chunk loop, progress, shutdown check)
├── reader.rs           ← Data source (DB cursor, file, API)
├── processor.rs        ← Transform/validate (pure functions)
├── writer.rs           ← Data sink (bulk insert, file, API)
├── checkpoint.rs       ← Cursor persistence
├── metrics.rs          ← Prometheus metrics
└── models.rs           ← Domain types, input/output DTOs
```

- [ ] **`main.rs` is thin** — Load config → connect DB → setup tracing → run job → exit with code.
- [ ] **Traits for abstraction** — `Reader`, `Processor`, `Writer` traits. Implementations are swappable (test doubles, different sources).
- [ ] **No `unwrap()` in production code** — `?` propagation everywhere. `unwrap()` only in tests or truly impossible cases (with comment explaining why).

---

## 3. Core Traits & Patterns

- [ ] **Reader/Processor/Writer traits** — The chunk-oriented pattern, Rust-style with associated types.

```rust
use async_trait::async_trait;

#[async_trait]
pub trait ItemReader {
    type Item: Send;
    type Cursor: Send + Clone + ToString + Default;

    async fn read_chunk(
        &self,
        cursor: &Self::Cursor,
        chunk_size: usize,
    ) -> Result<ReadResult<Self::Item, Self::Cursor>>;
}

pub struct ReadResult<T, C> {
    pub items: Vec<T>,
    pub next_cursor: Option<C>,
}

pub trait ItemProcessor {
    type Input: Send;
    type Output: Send;

    fn process(&self, item: Self::Input) -> Result<Option<Self::Output>>;
    // None = filtered, Err = skip/abort depending on policy
}

#[async_trait]
pub trait ItemWriter {
    type Item: Send;

    async fn write_batch(&self, items: Vec<Self::Item>) -> Result<()>;
}
```

- [ ] **Chunk orchestrator** — Generic over Reader/Processor/Writer. Handles the loop, checkpointing, error tracking.

```rust
pub struct ChunkOrchestrator<R, P, W> {
    reader: R,
    processor: P,
    writer: W,
    checkpoint: CheckpointStore,
    config: BatchConfig,
}

impl<R, P, W> ChunkOrchestrator<R, P, W>
where
    R: ItemReader,
    P: ItemProcessor<Input = R::Item>,
    W: ItemWriter<Item = P::Output>,
{
    pub async fn run(&self, cancel: CancellationToken) -> Result<BatchStats> {
        let mut cursor = self.checkpoint.load(&self.config.job_name).await?;
        let mut stats = BatchStats::default();

        loop {
            if cancel.is_cancelled() {
                tracing::warn!("Shutdown requested, stopping after current chunk");
                break;
            }

            let result = self.reader.read_chunk(&cursor, self.config.chunk_size).await?;
            if result.items.is_empty() {
                break;
            }
            stats.read += result.items.len();

            let mut outputs = Vec::with_capacity(result.items.len());
            for item in result.items {
                match self.processor.process(item) {
                    Ok(Some(output)) => outputs.push(output),
                    Ok(None) => stats.skipped += 1,
                    Err(e) => {
                        stats.errors += 1;
                        if stats.errors > self.config.skip_limit {
                            return Err(anyhow!("Skip limit exceeded: {e}"));
                        }
                        tracing::warn!(error = %e, "Item skipped");
                    }
                }
            }

            if !outputs.is_empty() {
                self.writer.write_batch(outputs).await?;
                stats.written += outputs.len();
            }

            if let Some(next) = result.next_cursor {
                cursor = next;
                self.checkpoint.save(&self.config.job_name, &cursor).await?;
            } else {
                break;
            }
        }
        Ok(stats)
    }
}
```

- [ ] **`BatchConfig` struct** — Loaded from environment. All tuning parameters externalized.

```rust
#[derive(Debug, Clone, serde::Deserialize)]
pub struct BatchConfig {
    pub job_name: String,
    pub chunk_size: usize,      // default 1000
    pub skip_limit: usize,      // default 100
    pub retry_attempts: usize,  // default 3
    pub timeout_secs: u64,      // default 7200 (2 hours)
    pub concurrency: usize,     // default 4
    pub database_url: String,
    pub dry_run: bool,          // default false
}
```

---

## 4. Configuration & Startup

- [ ] **Config from environment** — `config` crate or `envy`. Struct with defaults. Fail fast on missing required values.

```rust
impl BatchConfig {
    pub fn from_env() -> Result<Self> {
        let config = config::Config::builder()
            .set_default("chunk_size", 1000)?
            .set_default("skip_limit", 100)?
            .set_default("retry_attempts", 3)?
            .set_default("timeout_secs", 7200)?
            .set_default("concurrency", 4)?
            .set_default("dry_run", false)?
            .add_source(config::Environment::default())
            .build()?;
        config.try_deserialize().map_err(Into::into)
    }
}
```

- [ ] **CLI flags with `clap`** — For manual runs: `--date 2026-07-17 --dry-run --from-cursor 5000`.

```rust
#[derive(clap::Parser)]
struct Args {
    /// Date to process (YYYY-MM-DD)
    #[arg(long)]
    date: Option<chrono::NaiveDate>,

    /// Resume from specific cursor
    #[arg(long)]
    from_cursor: Option<i64>,

    /// Dry run — process but don't write
    #[arg(long)]
    dry_run: bool,

    /// Reset checkpoint and start fresh
    #[arg(long)]
    reset: bool,
}
```

- [ ] **Tracing setup** — JSON in production, pretty in dev. Filter by `RUST_LOG` env.

```rust
fn init_tracing() {
    tracing_subscriber::fmt()
        .json()
        .with_env_filter(
            tracing_subscriber::EnvFilter::try_from_default_env()
                .unwrap_or_else(|_| "info".into()),
        )
        .with_target(true)
        .with_thread_ids(true)
        .init();
}
```

- [ ] **Graceful shutdown** — `tokio_util::sync::CancellationToken` or `tokio::signal`. Propagate cancellation to chunk loop.

```rust
#[tokio::main]
async fn main() -> Result<()> {
    init_tracing();
    let config = BatchConfig::from_env()?;
    let pool = PgPool::connect(&config.database_url).await?;

    let cancel = CancellationToken::new();
    let cancel_clone = cancel.clone();

    // Shutdown signal handler
    tokio::spawn(async move {
        tokio::signal::ctrl_c().await.ok();
        tracing::info!("Shutdown signal received");
        cancel_clone.cancel();
    });

    // Run with timeout
    let result = tokio::time::timeout(
        Duration::from_secs(config.timeout_secs),
        run_job(&config, &pool, cancel),
    ).await;

    match result {
        Ok(Ok(stats)) => {
            tracing::info!(?stats, "Job completed successfully");
            Ok(())
        }
        Ok(Err(e)) => {
            tracing::error!(error = %e, "Job failed");
            std::process::exit(1);
        }
        Err(_) => {
            tracing::error!("Job timed out after {}s", config.timeout_secs);
            std::process::exit(2);
        }
    }
}
```

- [ ] **Exit codes** — `0` success, `1` failure, `2` timeout. Scheduler/K8s uses exit code for alerting and retry decisions.

---

## 5. Data Reading

- [ ] **sqlx cursor-based pagination** — Keyset pagination. Low memory. Compile-time checked SQL.

```rust
pub struct OrderReader {
    pool: PgPool,
}

#[async_trait]
impl ItemReader for OrderReader {
    type Item = OrderInput;
    type Cursor = i64;

    async fn read_chunk(&self, cursor: &i64, chunk_size: usize) -> Result<ReadResult<OrderInput, i64>> {
        let items = sqlx::query_as!(
            OrderInput,
            r#"
            SELECT id, amount, status, created_at
            FROM orders
            WHERE id > $1
            ORDER BY id ASC
            LIMIT $2
            "#,
            cursor,
            chunk_size as i64,
        )
        .fetch_all(&self.pool)
        .await?;

        let next_cursor = items.last().map(|item| item.id);
        Ok(ReadResult { items, next_cursor })
    }
}
```

- [ ] **File reading with `tokio::io`** — Async line-by-line. Or sync with `std::io::BufReader` in `spawn_blocking` (often simpler for batch).

```rust
use tokio::io::{AsyncBufReadExt, BufReader};
use tokio::fs::File;

pub async fn read_csv_chunks(path: &Path, chunk_size: usize) -> Result<Vec<Vec<Record>>> {
    let file = File::open(path).await?;
    let reader = BufReader::new(file);
    let mut lines = reader.lines();
    let mut chunks = Vec::new();
    let mut current_chunk = Vec::with_capacity(chunk_size);

    while let Some(line) = lines.next_line().await? {
        let record: Record = parse_csv_line(&line)?;
        current_chunk.push(record);
        if current_chunk.len() >= chunk_size {
            chunks.push(std::mem::take(&mut current_chunk));
        }
    }
    if !current_chunk.is_empty() {
        chunks.push(current_chunk);
    }
    Ok(chunks)
}
```

- [ ] **`Stream` trait for async iteration** — `futures::Stream` + `StreamExt` for composable async pipelines. `sqlx` returns streams natively.

```rust
use futures::StreamExt;

let mut stream = sqlx::query_as!(Order, "SELECT * FROM orders WHERE status = 'pending'")
    .fetch(&pool); // returns a Stream

while let Some(order) = stream.next().await {
    let order = order?;
    // process one at a time — lowest memory, but sequential
}
```

- [ ] **API client with retry** — `reqwest` + manual retry loop or `reqwest-middleware` + `reqwest-retry`.

---

## 6. Data Processing

- [ ] **Pure functions** — Processor is a plain `fn` or method with no side effects. `&self` for config access only. Easy to test.

```rust
pub struct OrderProcessor {
    min_amount: f64,
}

impl ItemProcessor for OrderProcessor {
    type Input = OrderInput;
    type Output = OrderOutput;

    fn process(&self, item: OrderInput) -> Result<Option<OrderOutput>> {
        // Validate
        if item.amount < 0.0 {
            return Err(anyhow!("Negative amount: {}", item.amount));
        }

        // Filter
        if item.amount < self.min_amount {
            return Ok(None); // filtered, not an error
        }

        // Transform
        Ok(Some(OrderOutput {
            order_id: item.id,
            amount_cents: (item.amount * 100.0) as i64,
            status: ProcessedStatus::Ready,
            processed_at: Utc::now(),
        }))
    }
}
```

- [ ] **Type system as validation** — Use newtypes: `struct OrderId(i64)`, `struct AmountCents(i64)`. Invalid states become unrepresentable. Compile-time safety over runtime checks.

```rust
#[derive(Debug, Clone, Copy)]
pub struct PositiveAmount(f64);

impl PositiveAmount {
    pub fn new(value: f64) -> Result<Self> {
        if value > 0.0 { Ok(Self(value)) }
        else { Err(anyhow!("Amount must be positive, got {value}")) }
    }

    pub fn as_cents(&self) -> i64 { (self.0 * 100.0) as i64 }
}
```

- [ ] **`Result<Option<T>>` convention** — `Ok(Some(output))` = success, `Ok(None)` = filtered/skip, `Err(e)` = error (skip or abort based on policy). Clear three-state return.
- [ ] **No `clone()` without reason** — Rust's ownership prevents accidental copies. Pass by reference where possible. `Clone` only when data must live in multiple places.

---

## 7. Data Writing

- [ ] **Bulk insert with sqlx** — Multi-row INSERT or PostgreSQL COPY. Not row-by-row.

```rust
pub struct OrderWriter {
    pool: PgPool,
}

#[async_trait]
impl ItemWriter for OrderWriter {
    type Item = OrderOutput;

    async fn write_batch(&self, items: Vec<OrderOutput>) -> Result<()> {
        let mut tx = self.pool.begin().await?;

        // Build multi-row INSERT with ON CONFLICT (upsert)
        let mut query_builder = sqlx::QueryBuilder::new(
            "INSERT INTO processed_orders (order_id, amount_cents, status, processed_at) "
        );

        query_builder.push_values(&items, |mut b, item| {
            b.push_bind(item.order_id)
             .push_bind(item.amount_cents)
             .push_bind(&item.status)
             .push_bind(item.processed_at);
        });

        query_builder.push(
            " ON CONFLICT (order_id) DO UPDATE SET \
              amount_cents = EXCLUDED.amount_cents, \
              status = EXCLUDED.status, \
              processed_at = EXCLUDED.processed_at"
        );

        query_builder.build().execute(&mut *tx).await?;
        tx.commit().await?;
        Ok(())
    }
}
```

- [ ] **Transaction per chunk** — `pool.begin()` → write → `tx.commit()`. Chunk failure → automatic rollback on `tx` drop. Rust's RAII guarantees cleanup.
- [ ] **COPY protocol for massive inserts** — `sqlx` doesn't support COPY natively. Use `tokio-postgres` directly for COPY, or staging table + `INSERT INTO ... SELECT`.
- [ ] **Upsert for idempotency** — `ON CONFLICT DO UPDATE`. Re-run safe. The writer's single most important property.
- [ ] **Dry-run writer** — Implement `ItemWriter` with no-op. Log what would be written. Same interface, zero mutations.

```rust
pub struct DryRunWriter;

#[async_trait]
impl ItemWriter for DryRunWriter {
    type Item = OrderOutput;

    async fn write_batch(&self, items: Vec<OrderOutput>) -> Result<()> {
        tracing::info!(count = items.len(), "DRY RUN: would write batch");
        Ok(())
    }
}
```


---

## 8. Concurrency & Parallelism

- [ ] **Tokio tasks for parallel chunks** — Spawn tasks per partition. Bounded concurrency with `tokio::sync::Semaphore`.

```rust
use tokio::sync::Semaphore;
use std::sync::Arc;

pub async fn process_parallel(
    items: Vec<OrderInput>,
    concurrency: usize,
    processor: Arc<OrderProcessor>,
) -> Result<Vec<OrderOutput>> {
    let semaphore = Arc::new(Semaphore::new(concurrency));
    let mut handles = Vec::with_capacity(items.len());

    for item in items {
        let permit = semaphore.clone().acquire_owned().await?;
        let processor = processor.clone();

        handles.push(tokio::spawn(async move {
            let result = processor.process(item);
            drop(permit); // release semaphore slot
            result
        }));
    }

    let mut outputs = Vec::new();
    for handle in handles {
        match handle.await? {
            Ok(Some(output)) => outputs.push(output),
            Ok(None) => {} // filtered
            Err(e) => tracing::warn!(error = %e, "Item processing failed"),
        }
    }
    Ok(outputs)
}
```

- [ ] **`futures::stream::buffered`** — Process a stream with bounded concurrency. Elegant for async pipelines.

```rust
use futures::{stream, StreamExt};

let results: Vec<Result<OrderOutput>> = stream::iter(items)
    .map(|item| async { processor.process(item) })
    .buffered(concurrency) // max concurrent futures
    .collect()
    .await;
```

- [ ] **Rayon for CPU-bound processing** — If transform is compute-heavy (parsing, encryption, hashing): `rayon::par_iter()`. Parallel iterator on thread pool. Don't mix with tokio — use `spawn_blocking`.

```rust
use rayon::prelude::*;

let outputs: Vec<OrderOutput> = tokio::task::spawn_blocking(move || {
    items.par_iter()
        .filter_map(|item| processor.process(item.clone()).ok().flatten())
        .collect()
}).await?;
```

- [ ] **Channel-based pipeline** — `tokio::sync::mpsc` for producer-consumer. Bounded channel = backpressure. Reader → channel → workers → channel → writer.

```rust
let (tx, mut rx) = tokio::sync::mpsc::channel::<OrderInput>(1000);

// Reader task
let reader_handle = tokio::spawn(async move {
    while let Some(chunk) = reader.read_chunk(&cursor, chunk_size).await? {
        for item in chunk.items {
            tx.send(item).await?;
        }
    }
    Ok::<_, anyhow::Error>(())
});

// Worker tasks
let worker_handle = tokio::spawn(async move {
    while let Some(item) = rx.recv().await {
        // process...
    }
});
```

- [ ] **No `Mutex` in async hot path** — `tokio::sync::Mutex` is fine for infrequent access. For high-throughput: use channels or partition data so no sharing is needed. Rust's type system prevents data races at compile time — but deadlocks are still possible.
- [ ] **`Send + Sync` bounds** — Tokio requires futures to be `Send`. Most types are automatically. If not: restructure to avoid holding non-Send types across `.await` points.

---

## 9. Error Handling

- [ ] **`thiserror` for domain errors** — Typed error variants. Pattern matching in callers.

```rust
#[derive(Debug, thiserror::Error)]
pub enum BatchError {
    #[error("Database error: {0}")]
    Database(#[from] sqlx::Error),

    #[error("Validation failed for item {item_id}: {reason}")]
    Validation { item_id: i64, reason: String },

    #[error("Skip limit exceeded ({count}/{limit})")]
    SkipLimitExceeded { count: usize, limit: usize },

    #[error("Timeout after {0:?}")]
    Timeout(Duration),

    #[error("Shutdown requested")]
    Shutdown,

    #[error(transparent)]
    Other(#[from] anyhow::Error),
}

impl BatchError {
    pub fn is_retryable(&self) -> bool {
        matches!(self, Self::Database(_))
    }
}
```

- [ ] **`anyhow` for propagation in `main`** — Use `anyhow::Result` in binary crate (`main.rs`). Use typed errors (`thiserror`) in library code. Don't mix — library exposes typed errors, binary converts to `anyhow`.
- [ ] **`?` operator everywhere** — Never `unwrap()` in production paths. `?` propagates cleanly. Add context with `.context("reading orders")?` (from `anyhow`).
- [ ] **Retry with backoff** — `tokio-retry` or manual loop. Only for `is_retryable()` errors.

```rust
use tokio_retry::strategy::{ExponentialBackoff, jitter};
use tokio_retry::Retry;

let strategy = ExponentialBackoff::from_millis(100)
    .factor(2)
    .map(jitter)
    .take(3);

let result = Retry::spawn(strategy, || async {
    writer.write_batch(chunk.clone()).await
}).await?;
```

- [ ] **Panic = bug, not error handling** — Rust panics are for programmer errors, not runtime errors. `panic!` / `unwrap()` in batch code = crash. Use `Result` for all fallible operations. Set `panic = "abort"` in release profile for clean exits.

---

## 10. Checkpoint & Resumability

- [ ] **Checkpoint store** — Database table. Save after each chunk commit.

```rust
pub struct CheckpointStore {
    pool: PgPool,
}

impl CheckpointStore {
    pub async fn save(&self, job_name: &str, cursor: &impl ToString) -> Result<()> {
        sqlx::query!(
            r#"
            INSERT INTO batch_checkpoints (job_name, cursor_value, updated_at)
            VALUES ($1, $2, now())
            ON CONFLICT (job_name) DO UPDATE SET cursor_value = $2, updated_at = now()
            "#,
            job_name,
            cursor.to_string(),
        )
        .execute(&self.pool)
        .await?;
        Ok(())
    }

    pub async fn load(&self, job_name: &str) -> Result<Option<String>> {
        let row = sqlx::query_scalar!(
            "SELECT cursor_value FROM batch_checkpoints WHERE job_name = $1",
            job_name,
        )
        .fetch_optional(&self.pool)
        .await?;
        Ok(row.flatten())
    }

    pub async fn reset(&self, job_name: &str) -> Result<()> {
        sqlx::query!("DELETE FROM batch_checkpoints WHERE job_name = $1", job_name)
            .execute(&self.pool)
            .await?;
        Ok(())
    }
}
```

- [ ] **Atomic checkpoint + write** — If writer and checkpoint share the same database: save checkpoint in the same transaction as the write. Zero gap between "data committed" and "progress recorded."
- [ ] **Generic cursor type** — Cursor can be `i64` (ID), `String` (composite key), `DateTime` (timestamp). Trait bound: `ToString + FromStr + Send + Clone`.

---

## 11. Observability

- [ ] **`tracing` spans per chunk** — Structured, zero-cost when disabled. Correlate all logs from one job run.

```rust
use tracing::{info, info_span, Instrument};

async fn process_chunk(chunk_num: usize, items: Vec<OrderInput>) -> Result<BatchStats> {
    let span = info_span!("chunk", number = chunk_num, size = items.len());

    async move {
        let start = Instant::now();
        // ... processing
        info!(
            read = items.len(),
            written = stats.written,
            skipped = stats.skipped,
            duration_ms = start.elapsed().as_millis() as u64,
            "Chunk complete"
        );
        Ok(stats)
    }
    .instrument(span)
    .await
}
```

- [ ] **Prometheus metrics** — `prometheus` crate. Push to Pushgateway before exit (short-lived process). Or expose `/metrics` endpoint if long-running worker.

```rust
use prometheus::{IntCounter, IntCounterVec, Histogram, register_int_counter_vec, register_histogram};

lazy_static::lazy_static! {
    static ref ITEMS_PROCESSED: IntCounterVec = register_int_counter_vec!(
        "batch_items_total", "Items processed", &["job", "outcome"]
    ).unwrap();

    static ref CHUNK_DURATION: Histogram = register_histogram!(
        "batch_chunk_duration_seconds", "Chunk processing duration",
        vec![0.1, 0.5, 1.0, 2.0, 5.0, 10.0, 30.0, 60.0]
    ).unwrap();
}
```

- [ ] **OpenTelemetry** — `opentelemetry` + `tracing-opentelemetry`. Export traces to Jaeger/Tempo. Spans propagate to downstream HTTP calls via `reqwest-tracing`.
- [ ] **Job summary on exit** — Final structured log with all stats. Machine-parseable for alerting.

```rust
info!(
    job = config.job_name,
    total_read = stats.read,
    total_written = stats.written,
    total_skipped = stats.skipped,
    total_errors = stats.errors,
    duration_secs = start.elapsed().as_secs(),
    "Job finished"
);
```

- [ ] **Memory tracking** — Rust gives you control but doesn't track for you. Use `jemalloc` + `jemalloc-ctl` for allocation stats. Or `procfs` crate to read `/proc/self/status` RSS.

---

## 12. Testing

- [ ] **Unit tests for processor** — Pure functions. No async. Fast. In-module `#[cfg(test)]`.

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_process_valid_order() {
        let processor = OrderProcessor { min_amount: 10.0 };
        let input = OrderInput { id: 1, amount: 100.0, status: "pending".into(), created_at: Utc::now() };
        let result = processor.process(input).unwrap();
        assert!(result.is_some());
        assert_eq!(result.unwrap().amount_cents, 10000);
    }

    #[test]
    fn test_filter_low_amount() {
        let processor = OrderProcessor { min_amount: 10.0 };
        let input = OrderInput { id: 2, amount: 5.0, status: "pending".into(), created_at: Utc::now() };
        let result = processor.process(input).unwrap();
        assert!(result.is_none()); // filtered
    }

    #[test]
    fn test_reject_negative_amount() {
        let processor = OrderProcessor { min_amount: 10.0 };
        let input = OrderInput { id: 3, amount: -50.0, status: "pending".into(), created_at: Utc::now() };
        assert!(processor.process(input).is_err());
    }
}
```

- [ ] **Integration tests with testcontainers** — Real PostgreSQL. Run full job. Verify output.

```rust
#[tokio::test]
async fn test_full_job_run() {
    let pg = testcontainers::postgres::Postgres::default();
    let container = pg.start().await;
    let pool = PgPool::connect(&container.connection_string()).await.unwrap();

    sqlx::migrate!().run(&pool).await.unwrap();
    seed_test_data(&pool, 100).await;

    let config = BatchConfig { chunk_size: 25, skip_limit: 10, ..test_defaults() };
    let stats = run_job(&config, &pool, CancellationToken::new()).await.unwrap();

    assert_eq!(stats.read, 100);
    assert_eq!(stats.written, 100);
    assert_eq!(stats.errors, 0);
}
```

- [ ] **Idempotency test** — Run twice, verify no duplicates.
- [ ] **Crash recovery test** — Cancel token after N chunks → restart → verify resume.
- [ ] **`cargo test -- --nocapture`** — See tracing output in tests for debugging.
- [ ] **Miri for unsafe code** — If using `unsafe` anywhere (unlikely for batch): `cargo +nightly miri test`. Detects undefined behavior.


---

## 13. Deployment & Operations

- [ ] **Minimal container** — Static binary. No runtime dependencies. `FROM scratch` or `distroless`.

```dockerfile
FROM rust:1.80-alpine AS build
WORKDIR /src
RUN apk add --no-cache musl-dev
COPY Cargo.toml Cargo.lock ./
COPY src ./src
RUN cargo build --release --target x86_64-unknown-linux-musl

FROM scratch
COPY --from=build /src/target/x86_64-unknown-linux-musl/release/order-sync /app
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
ENTRYPOINT ["/app"]
```

- [ ] **Binary size** — Release builds: `strip = true`, `lto = true`, `opt-level = "z"` (or `"s"`). Typical batch binary: 5–20MB. Tiny containers = fast deploys.

```toml
[profile.release]
strip = true
lto = true
opt-level = "s"
panic = "abort"     # smaller binary, no unwinding
codegen-units = 1   # better optimization, slower compile
```

- [ ] **K8s CronJob** — Same pattern as Go. Static binary starts fast (no JVM warmup, no interpreter). Cold start: milliseconds.
- [ ] **Resource limits** — Rust batch jobs are memory-efficient. 32–128MB typical unless holding large buffers. Set conservative limits. Rust won't OOM silently — the allocator panics.
- [ ] **Cross-compilation** — Build on CI (x86_64-unknown-linux-musl) for Alpine/scratch containers. `cross` tool simplifies cross-compilation for ARM (Graviton).
- [ ] **Runbook** — Same as other stacks: trigger, check status, reset checkpoint, replay, kill stuck, backfill.

---

## 14. Rust-Specific Advantages for Batch

- [ ] **No GC pauses** — Predictable latency per chunk. No "stop the world" spikes. Critical for SLO-bound batch jobs.
- [ ] **Zero-cost abstractions** — Iterators, trait dispatch, generics compile to the same code as hand-written loops. Abstraction doesn't cost performance.
- [ ] **Fearless concurrency** — Compiler prevents data races. `Send`/`Sync` traits enforced at compile time. No "works in dev, races in prod" surprises.
- [ ] **Memory safety without GC** — No null pointer crashes, no use-after-free, no buffer overflows. Batch jobs processing untrusted data (files, external APIs) are inherently safer.
- [ ] **Small binaries, fast startup** — 5–20MB binary. Starts in milliseconds. Perfect for K8s CronJobs where startup time matters (many short jobs per day).
- [ ] **Excellent for CPU-bound transforms** — If your batch job does heavy parsing, encryption, compression, or computation: Rust is 10–50x faster than Python/Node, 2–5x faster than Go/Java for CPU-bound work.

---

## Quick Sanity Check Before Launch

- [ ] Job is idempotent — upserts in writer, tested by running twice
- [ ] Resumable — cancel token mid-run, restart, verify cursor-based resume
- [ ] No `unwrap()` in production paths — `?` propagation, no panics on bad data
- [ ] `cargo clippy -- -D warnings` passes — no dead code, no implicit conversions
- [ ] Memory bounded — tested with production-scale data, no unbounded `Vec` growth
- [ ] Connection pool sized — `max_connections` ≈ concurrency
- [ ] Graceful shutdown — SIGTERM → current chunk finishes → checkpoint saved → exit 0
- [ ] Error threshold works — skip limit triggers abort, not infinite skip
- [ ] Metrics pushed before exit — Pushgateway or OTLP export for short-lived binary
- [ ] Exit code correct — 0 success, 1 failure, 2 timeout
- [ ] `cargo test` all green — unit + integration (with `--features integration` flag)
- [ ] Release build tested — `cargo build --release` (optimizations can surface different behavior)
- [ ] Static binary in scratch container — no runtime deps, minimal attack surface

---

## Rust Batch Libraries Reference

| Concern | Crate | Notes |
|---------|-------|-------|
| **Async runtime** | `tokio` | Multi-threaded, work-stealing. The standard. |
| **DB (Postgres)** | `sqlx` | Compile-time checked SQL, async, pool |
| **DB (any)** | `diesel` | ORM-style, sync (use with `spawn_blocking`) |
| **Migrations** | `sqlx` (built-in) | `sqlx migrate run` |
| **Config** | `config` | YAML + env + defaults |
| **Config (simple)** | `envy` | Env vars → struct |
| **CLI** | `clap` | Derive or builder API |
| **Error types** | `thiserror` | Derive `Error` trait |
| **Error propagation** | `anyhow` | Erased errors for binaries |
| **Serialization** | `serde` + `serde_json` | Universal |
| **CSV** | `csv` | Fast, streaming |
| **HTTP client** | `reqwest` | Async, built on hyper |
| **Retry** | `tokio-retry` | Strategy-based retry |
| **Concurrency** | `tokio::sync::Semaphore` | Bounded parallelism |
| **CPU parallelism** | `rayon` | Data parallelism, thread pool |
| **Streams** | `futures` | `Stream`, `StreamExt`, combinators |
| **Tracing** | `tracing` + `tracing-subscriber` | Structured, zero-cost |
| **Metrics** | `prometheus` | Counters, histograms, push |
| **OpenTelemetry** | `opentelemetry` + `tracing-opentelemetry` | OTLP export |
| **Shutdown** | `tokio-util` (`CancellationToken`) | Cooperative cancellation |
| **Time** | `chrono` | DateTime, timezone-aware |
| **UUID** | `uuid` | v4 random, v7 time-ordered |
| **Testing** | `testcontainers` | Real DB in tests |
| **Orchestration** | Temporal (via gRPC) | No native Rust SDK yet — use Go/TS SDK or gRPC |
