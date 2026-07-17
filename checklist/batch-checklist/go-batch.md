# Go Batch Processing Checklist

> Go-specific companion to the general [Batch Processing Checklist](batch.md).
> Idiomatic Go patterns — stdlib, goroutines, channels, context. No heavy framework needed.
> Go 1.22+. Last updated: 2026-07-17

---

## 1. Project Setup & Structure

```
cmd/
└── jobname/
    └── main.go              ← Entry point. Parse flags/env, wire deps, run job, exit.
internal/
├── job/                      ← Job orchestration (step sequencing, lifecycle)
├── reader/                   ← Data source readers (DB cursor, file, API)
├── processor/                ← Transform/validate logic (pure functions)
├── writer/                   ← Data sink writers (DB batch insert, file, API)
├── model/                    ← Domain types, DTOs
├── config/                   ← Config loading (env, flags, YAML)
└── worker/                   ← Worker pool, concurrency primitives
```

- [ ] **Go 1.22+** — Rangefunc, enhanced routing patterns, `slices`/`maps` stdlib. `go.mod` + `go.sum` committed.
- [ ] **`cmd/` per job** — One binary per batch job. `cmd/order-sync/`, `cmd/report-gen/`. Each is independently deployable.
- [ ] **`internal/` for private packages** — Compiler-enforced encapsulation. Shared logic in `internal/shared/` or `pkg/` if truly reusable across repos.
- [ ] **Linter** — `golangci-lint` with strict config. Batch-critical linters: `errcheck` (never ignore errors), `bodyclose` (HTTP client leaks), `sqlclosecheck` (DB row leaks), `contextcheck`.
- [ ] **Build tags for integration tests** — `//go:build integration`. Separate from unit tests. `go test -tags=integration ./...`
- [ ] **Single binary output** — `CGO_ENABLED=0 go build -o /app ./cmd/jobname`. Static binary. Scratch or distroless container.

---

## 2. Configuration & Startup

- [ ] **Config from environment** — `os.Getenv`, `envconfig`, or Viper. Twelve-factor. No config files baked into image.

```go
type Config struct {
    DBUrl        string        `env:"DB_URL" required:"true"`
    ChunkSize    int           `env:"CHUNK_SIZE" default:"1000"`
    Concurrency  int           `env:"CONCURRENCY" default:"4"`
    SkipLimit    int           `env:"SKIP_LIMIT" default:"100"`
    Timeout      time.Duration `env:"JOB_TIMEOUT" default:"2h"`
    DryRun       bool          `env:"DRY_RUN" default:"false"`
}
```

- [ ] **CLI flags for manual runs** — `flag` package or `cobra` for complex CLIs. `--date=2026-07-17`, `--dry-run`, `--from-id=5000`. Same binary, different invocations.
- [ ] **Validate config at startup** — Fail fast if required config missing or invalid. Don't discover missing DB_URL 30 minutes into processing.
- [ ] **Context with timeout** — Top-level `context.WithTimeout(ctx, cfg.Timeout)`. Entire job bounded. Cancel propagates to all goroutines.

```go
func main() {
    cfg := loadConfig()
    
    ctx, cancel := context.WithTimeout(context.Background(), cfg.Timeout)
    defer cancel()
    
    // Wire dependencies
    db := mustConnectDB(ctx, cfg.DBUrl)
    defer db.Close()
    
    // Run job
    if err := runJob(ctx, cfg, db); err != nil {
        slog.Error("job failed", "error", err)
        os.Exit(1)
    }
    slog.Info("job completed successfully")
}
```

- [ ] **Graceful shutdown on signal** — `signal.NotifyContext(ctx, syscall.SIGTERM, syscall.SIGINT)`. SIGTERM → context cancelled → current chunk finishes → exit cleanly.

```go
ctx, stop := signal.NotifyContext(context.Background(), syscall.SIGTERM, syscall.SIGINT)
defer stop()
```

- [ ] **Exit codes** — `0` success, `1` failure, `2` partial (some records skipped). Scheduler/K8s uses exit code for alerting.

---

## 3. Data Reading Patterns

- [ ] **Database cursor (streaming)** — `sql.Rows` with `rows.Next()` loop. Low memory. Don't `SELECT *` into a slice.

```go
func (r *OrderReader) ReadChunk(ctx context.Context, cursor int64, limit int) ([]Order, int64, error) {
    rows, err := r.db.QueryContext(ctx,
        `SELECT id, amount, status FROM orders WHERE id > $1 ORDER BY id LIMIT $2`,
        cursor, limit,
    )
    if err != nil {
        return nil, cursor, fmt.Errorf("query orders: %w", err)
    }
    defer rows.Close()

    var orders []Order
    var lastID int64
    for rows.Next() {
        var o Order
        if err := rows.Scan(&o.ID, &o.Amount, &o.Status); err != nil {
            return nil, cursor, fmt.Errorf("scan order: %w", err)
        }
        orders = append(orders, o)
        lastID = o.ID
    }
    return orders, lastID, rows.Err()
}
```

- [ ] **Keyset pagination over OFFSET** — `WHERE id > $lastID ORDER BY id LIMIT $chunkSize`. O(1) per page regardless of offset. OFFSET slows down at high values.
- [ ] **File streaming** — `bufio.Scanner` for line-by-line. `encoding/csv` reader for CSV. Never `os.ReadFile` on large files.
- [ ] **API pagination** — Follow cursor/next-page pattern. Respect rate limits with `time.Ticker` or `golang.org/x/time/rate.Limiter`.
- [ ] **Channel-based reader** — Reader goroutine sends items to a channel. Workers consume from channel. Decouples reading from processing.

```go
func readOrders(ctx context.Context, db *sql.DB, out chan<- Order) error {
    defer close(out)
    // ... paginated reads, send each item to out channel
    for _, order := range chunk {
        select {
        case out <- order:
        case <-ctx.Done():
            return ctx.Err()
        }
    }
    return nil
}
```

---

## 4. Processing Patterns

- [ ] **Pure transform functions** — No side effects. Input → Output. Testable in isolation. `func ProcessOrder(in OrderInput) (OrderOutput, error)`.
- [ ] **Return error to skip, don't panic** — Processor returns `error` → caller decides: skip, retry, or abort. Never `log.Fatal` inside processing logic.
- [ ] **Batch validation** — Validate before transform. `func Validate(in OrderInput) error`. Separate concern from transformation.
- [ ] **Filter with nil return** — Convention: processor returns `(nil, nil)` to signal "skip this item, not an error." Caller checks both return values.

```go
func processOrder(ctx context.Context, in OrderInput) (*OrderOutput, error) {
    if err := validate(in); err != nil {
        return nil, fmt.Errorf("validation: %w", err) // error → skip/retry
    }
    if in.Status == "cancelled" {
        return nil, nil // filtered out, not an error
    }
    out := transform(in)
    return &out, nil
}
```

---

## 5. Writing Patterns

- [ ] **Batch INSERT** — `pgx.CopyFrom` (PostgreSQL COPY protocol, fastest), multi-row INSERT, or `sqlx.NamedExec` with slice. Never row-by-row INSERT in a loop.

```go
func (w *OrderWriter) WriteBatch(ctx context.Context, orders []OrderOutput) error {
    tx, err := w.db.BeginTx(ctx, nil)
    if err != nil {
        return fmt.Errorf("begin tx: %w", err)
    }
    defer tx.Rollback()

    stmt, err := tx.PrepareContext(ctx,
        `INSERT INTO processed_orders (id, amount, status, processed_at)
         VALUES ($1, $2, $3, $4)
         ON CONFLICT (id) DO UPDATE SET amount = EXCLUDED.amount, status = EXCLUDED.status`)
    if err != nil {
        return fmt.Errorf("prepare: %w", err)
    }
    defer stmt.Close()

    for _, o := range orders {
        if _, err := stmt.ExecContext(ctx, o.ID, o.Amount, o.Status, time.Now()); err != nil {
            return fmt.Errorf("exec order %d: %w", o.ID, err)
        }
    }
    return tx.Commit()
}
```

- [ ] **Upsert for idempotency** — `ON CONFLICT DO UPDATE` (Postgres), `ON DUPLICATE KEY UPDATE` (MySQL). Re-run safe.
- [ ] **File writer with buffering** — `bufio.Writer` wrapping `os.File`. Flush per chunk. `defer writer.Flush()`.
- [ ] **Transaction per chunk** — Begin TX → write chunk → commit. Not per-record (slow) and not per-job (risky). Chunk failure → rollback one chunk only.

---

## 6. Concurrency & Worker Pool

- [ ] **`errgroup` for bounded concurrency** — The Go pattern for worker pools. Limits goroutines, propagates errors, respects context.

```go
func processParallel(ctx context.Context, items []Order, concurrency int) error {
    g, ctx := errgroup.WithContext(ctx)
    g.SetLimit(concurrency)

    for _, item := range items {
        g.Go(func() error {
            return processAndWrite(ctx, item)
        })
    }
    return g.Wait()
}
```

- [ ] **Channel + worker pool pattern** — For streaming: reader → channel → N workers → results channel → writer. Fan-out/fan-in.

```go
func runPipeline(ctx context.Context, cfg Config) error {
    items := make(chan OrderInput, cfg.ChunkSize)
    results := make(chan OrderOutput, cfg.ChunkSize)

    g, ctx := errgroup.WithContext(ctx)

    // Reader
    g.Go(func() error { return readAll(ctx, db, items) })

    // Workers (fan-out)
    for range cfg.Concurrency {
        g.Go(func() error { return worker(ctx, items, results) })
    }

    // Writer (fan-in)
    g.Go(func() error { return writeAll(ctx, db, results) })

    return g.Wait()
}
```

- [ ] **Buffered channels** — Size = chunk size or N * workers. Prevents goroutine blocking. Unbuffered only when you need synchronization.
- [ ] **No shared mutable state** — Communicate via channels, not shared maps/slices. If you must share: `sync.Mutex` or `sync.Map`. But prefer channels.
- [ ] **`sync.WaitGroup` for simple fan-out** — When `errgroup` is overkill (fire-and-forget, errors handled per-item). `wg.Add(1)` before goroutine, `wg.Done()` inside.
- [ ] **Rate limiter for external calls** — `golang.org/x/time/rate.NewLimiter(rate.Every(time.Second/10), 1)`. Call `limiter.Wait(ctx)` before each external API call.

```go
limiter := rate.NewLimiter(rate.Every(time.Second/10), 1) // 10 req/sec

for _, item := range batch {
    if err := limiter.Wait(ctx); err != nil {
        return err // context cancelled
    }
    // make API call
}
```

---

## 7. Error Handling & Resilience

- [ ] **Error wrapping** — Always `fmt.Errorf("context: %w", err)`. Preserve chain. Caller can `errors.Is` / `errors.As` for classification.
- [ ] **Skip-on-error with threshold** — Track error count. Skip bad records up to limit. Exceed → abort job.

```go
type SkipTracker struct {
    mu       sync.Mutex
    skipped  []SkippedItem
    limit    int
}

func (s *SkipTracker) RecordSkip(item any, err error) error {
    s.mu.Lock()
    defer s.mu.Unlock()
    s.skipped = append(s.skipped, SkippedItem{Item: item, Err: err, At: time.Now()})
    if len(s.skipped) > s.limit {
        return fmt.Errorf("skip limit exceeded (%d): last error: %w", s.limit, err)
    }
    return nil
}
```

- [ ] **Retry with backoff** — `cenkalti/backoff/v4` or hand-rolled. Exponential + jitter. Only for transient errors (network, deadlock). Max attempts.

```go
func withRetry(ctx context.Context, maxAttempts int, fn func() error) error {
    var err error
    for attempt := range maxAttempts {
        if err = fn(); err == nil {
            return nil
        }
        if !isTransient(err) {
            return err // permanent error, don't retry
        }
        wait := time.Duration(1<<attempt) * 100 * time.Millisecond
        select {
        case <-time.After(wait):
        case <-ctx.Done():
            return ctx.Err()
        }
    }
    return fmt.Errorf("max retries exceeded: %w", err)
}
```

- [ ] **Error classification** — Define sentinel errors or error types. `var ErrValidation = errors.New("validation")`. Check with `errors.Is(err, ErrValidation)` to decide skip vs retry vs abort.
- [ ] **Dead letter table** — Write failed items + error to DB/file. Inspectable. Replayable later.
- [ ] **Panic recovery in workers** — `defer func() { if r := recover(); r != nil { ... } }()` in each goroutine. Log stack trace. Don't let one panic kill all workers.


---

## 8. Checkpoint & Resumability

- [ ] **Cursor persistence** — Store "last processed ID" in database or file after each chunk commit. On restart: read cursor, resume from there.

```go
type Checkpoint struct {
    db *sql.DB
}

func (c *Checkpoint) Save(ctx context.Context, jobName string, cursor int64) error {
    _, err := c.db.ExecContext(ctx,
        `INSERT INTO batch_checkpoints (job_name, cursor_value, updated_at)
         VALUES ($1, $2, now())
         ON CONFLICT (job_name) DO UPDATE SET cursor_value = $2, updated_at = now()`,
        jobName, cursor,
    )
    return err
}

func (c *Checkpoint) Load(ctx context.Context, jobName string) (int64, error) {
    var cursor int64
    err := c.db.QueryRowContext(ctx,
        `SELECT cursor_value FROM batch_checkpoints WHERE job_name = $1`, jobName,
    ).Scan(&cursor)
    if errors.Is(err, sql.ErrNoRows) {
        return 0, nil // fresh start
    }
    return cursor, err
}
```

- [ ] **Checkpoint per chunk commit** — Update cursor AFTER successful chunk write (in same transaction if possible). Crash between chunks → restart from last committed cursor.
- [ ] **Idempotent from any checkpoint** — Re-processing from cursor N must produce same result as first run from N. Upserts in writer enable this.
- [ ] **Reset checkpoint for backfill** — Manual override: `--reset-cursor` flag or API call to clear checkpoint. Reprocess from beginning.

---

## 9. Observability

- [ ] **Structured logging with `slog`** — Go 1.21+ stdlib. JSON output. Context fields: job name, run ID, chunk number.

```go
logger := slog.New(slog.NewJSONHandler(os.Stdout, &slog.HandlerOptions{Level: slog.LevelInfo}))
logger = logger.With("job", "order-sync", "runID", uuid.NewString())

logger.Info("chunk processed",
    "chunk", chunkNum,
    "read", readCount,
    "written", writeCount,
    "skipped", skipCount,
    "duration_ms", elapsed.Milliseconds(),
)
```

- [ ] **Metrics with Prometheus client** — `prometheus/client_golang`. Counters: items read, processed, written, skipped, errored. Histogram: chunk duration. Gauge: active goroutines, progress percentage.

```go
var (
    itemsProcessed = promauto.NewCounterVec(prometheus.CounterOpts{
        Name: "batch_items_processed_total",
        Help: "Total items processed by outcome",
    }, []string{"job", "outcome"}) // outcome: success, skipped, error

    chunkDuration = promauto.NewHistogramVec(prometheus.HistogramOpts{
        Name:    "batch_chunk_duration_seconds",
        Help:    "Time to process one chunk",
        Buckets: prometheus.ExponentialBuckets(0.1, 2, 10),
    }, []string{"job"})
)
```

- [ ] **Push metrics on exit** — Batch jobs are short-lived. Prometheus pull won't catch them. Use Prometheus Pushgateway, or emit metrics to OTLP/Datadog/CloudWatch directly before exit.
- [ ] **OpenTelemetry tracing** — `go.opentelemetry.io/otel`. Span per job, span per chunk. Propagate to downstream API calls. Export to Jaeger/Tempo/Datadog.
- [ ] **Progress logging** — Every N chunks or every N seconds: log progress. `"processed 15000/50000 (30%) — 2m12s elapsed, ~5m08s remaining"`.
- [ ] **Job summary on exit** — Final log line with totals: read, written, skipped, errored, duration. Machine-parseable for alerting.

```go
logger.Info("job complete",
    "total_read", stats.Read,
    "total_written", stats.Written,
    "total_skipped", stats.Skipped,
    "total_errors", stats.Errors,
    "duration", time.Since(start).String(),
)
```

---

## 10. Database Specifics

- [ ] **Connection pool tuning** — `db.SetMaxOpenConns(cfg.Concurrency + 2)`. Match to worker count. Too many → DB overwhelmed. Too few → workers idle waiting for connections.
- [ ] **`db.SetConnMaxLifetime`** — Shorter than DB server timeout. `5 * time.Minute` reasonable. Prevents stale connection errors mid-batch.
- [ ] **Prepared statements for hot paths** — `db.PrepareContext` once, reuse in loop. Avoid re-parsing SQL per chunk.
- [ ] **`pgx` over `database/sql` for PostgreSQL** — Native driver. COPY protocol for bulk inserts. Batch queries. Connection pool built-in (`pgxpool`). Significantly faster for batch workloads.
- [ ] **Transaction isolation** — `sql.LevelReadCommitted` (default) usually fine. `LevelRepeatableRead` if you need snapshot consistency across chunks. `LevelSerializable` only if correctness demands it (performance cost).
- [ ] **Advisory locks for job concurrency** — `SELECT pg_try_advisory_lock($1)`. Prevent two instances of same job running simultaneously. Release on exit.

```go
func acquireLock(ctx context.Context, db *sql.DB, lockID int64) (bool, error) {
    var acquired bool
    err := db.QueryRowContext(ctx, "SELECT pg_try_advisory_lock($1)", lockID).Scan(&acquired)
    return acquired, err
}
```

---

## 11. Testing

- [ ] **Unit tests for processors** — Pure functions. Table-driven tests. No DB, no I/O.

```go
func TestProcessOrder(t *testing.T) {
    tests := []struct {
        name    string
        input   OrderInput
        want    *OrderOutput
        wantErr bool
    }{
        {"valid order", OrderInput{ID: 1, Amount: 100}, &OrderOutput{ID: 1, Amount: 100, Status: "processed"}, false},
        {"zero amount", OrderInput{ID: 2, Amount: 0}, nil, true},
        {"cancelled filtered", OrderInput{ID: 3, Status: "cancelled"}, nil, false},
    }
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got, err := processOrder(context.Background(), tt.input)
            if tt.wantErr { assert.Error(t, err); return }
            assert.Equal(t, tt.want, got)
        })
    }
}
```

- [ ] **Integration tests with Testcontainers** — `testcontainers-go`. Real PostgreSQL. Seed data → run job → verify output. `//go:build integration`.
- [ ] **Idempotency test** — Run job → run again with same cursor start. Verify: no duplicates in target table, same final state.
- [ ] **Crash recovery test** — Start job → cancel context after N items → restart with checkpoint → verify no gaps, no duplicates.
- [ ] **Benchmark tests** — `func BenchmarkProcessChunk(b *testing.B)`. Measure throughput. Compare chunk sizes, concurrency levels.
- [ ] **Race detector** — `go test -race ./...`. Run on every CI build. Catches data races in concurrent code.

---

## 12. Deployment & Operations

- [ ] **Minimal container** — Multi-stage build. Final image: `scratch` or `gcr.io/distroless/static`. Static binary. ~5-15MB image.

```dockerfile
FROM golang:1.22-alpine AS build
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /app ./cmd/order-sync

FROM scratch
COPY --from=build /app /app
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
ENTRYPOINT ["/app"]
```

- [ ] **K8s CronJob** — Same pattern as Spring Boot batch. `concurrencyPolicy: Forbid`. `restartPolicy: OnFailure`. `backoffLimit: 2`.
- [ ] **Resource limits** — Know your memory profile. Go batch jobs: typically 50-200MB unless holding large buffers. Set limits accordingly. OOMKilled = chunk too large or unbounded slice growth.
- [ ] **Liveness vs one-shot** — CronJob (one-shot): runs, exits, K8s tracks completion. Long-running worker (consuming queue): needs health endpoint (`/healthz`) and liveness probe.
- [ ] **Runbook** — How to: trigger manually (`kubectl create job --from=cronjob/order-sync manual-run`), check logs, reset checkpoint, replay from DLQ, adjust concurrency via env var.

---

## 13. Orchestration (When stdlib isn't enough)

- [ ] **Temporal for complex workflows** — Multi-step jobs with retries, compensation, long-running. Go SDK is first-class. Activities = individual steps. Workflows = orchestration.

```go
// Temporal workflow
func OrderSyncWorkflow(ctx workflow.Context, params SyncParams) error {
    opts := workflow.ActivityOptions{StartToCloseTimeout: 30 * time.Minute}
    ctx = workflow.WithActivityOptions(ctx, opts)

    var extracted []Order
    if err := workflow.ExecuteActivity(ctx, ExtractOrders, params.Date).Get(ctx, &extracted); err != nil {
        return err
    }
    if err := workflow.ExecuteActivity(ctx, TransformAndLoad, extracted).Get(ctx, nil); err != nil {
        return err
    }
    return workflow.ExecuteActivity(ctx, SendReport, params.Date).Get(ctx, nil)
}
```

- [ ] **Asynq for Redis-based queues** — Lightweight. Define tasks, enqueue, process. Scheduled tasks, retries, dead letter. Good for "BullMQ but in Go."
- [ ] **River for Postgres-based queues** — No Redis dependency. Transactional enqueue (enqueue job in same TX as business write). Newer, growing fast.
- [ ] **When to use what** — Simple cron job → plain Go + K8s CronJob. Queue worker → Asynq or River. Complex DAG/saga → Temporal. Don't over-engineer: most Go batch jobs are just `main()` with good structure.

---

## Quick Sanity Check Before Launch

- [ ] Job is idempotent — tested by running twice, verified no duplicates (upserts work)
- [ ] Resumable — kill mid-run, restart, verify cursor-based resume (no gaps)
- [ ] Context propagation — timeout and cancellation respected in all goroutines
- [ ] `errgroup` or worker pool bounded — not spawning unbounded goroutines
- [ ] Memory bounded — tested with production-scale data, no unbounded slice growth
- [ ] Connection pool sized — `MaxOpenConns` ≈ concurrency, not default unlimited
- [ ] Race detector clean — `go test -race` passes
- [ ] Graceful shutdown — SIGTERM → current chunk finishes → checkpoint saved → exit 0
- [ ] Error threshold works — too many skips → job fails (not infinite skip)
- [ ] Metrics pushed before exit — Pushgateway or direct export (short-lived process)
- [ ] Exit code correct — success=0, failure=1, scheduler detects failure
- [ ] Advisory lock or CronJob Forbid — no overlapping runs
- [ ] Structured logs — JSON, filterable by job name + run ID

---

## Go Libraries Quick Reference

| Concern | Library | Notes |
|---------|---------|-------|
| **DB (Postgres)** | `jackc/pgx/v5` | Native driver, COPY, batch, pool |
| **DB (generic)** | `database/sql` + `sqlx` | Standard, works with any driver |
| **Migrations** | `golang-migrate/migrate` | CLI + Go library |
| **Config** | `kelseyhightower/envconfig` | Simple env-based |
| **Config (complex)** | `spf13/viper` | YAML + env + flags |
| **Concurrency** | `golang.org/x/sync/errgroup` | Bounded worker pools |
| **Rate limiting** | `golang.org/x/time/rate` | Token bucket |
| **Retry** | `cenkalti/backoff/v4` | Exponential + jitter |
| **Logging** | `log/slog` (stdlib) | Structured, JSON |
| **Metrics** | `prometheus/client_golang` | Counters, histograms, push |
| **Tracing** | `go.opentelemetry.io/otel` | OTLP export |
| **Testing** | `testcontainers/testcontainers-go` | Real DB in tests |
| **Testing** | `stretchr/testify` | Assertions, mocks |
| **Queue (Redis)** | `hibiken/asynq` | Task queue, scheduler |
| **Queue (Postgres)** | `riverqueue/river` | Transactional enqueue |
| **Orchestration** | `go.temporal.io/sdk` | Durable workflows |
| **CLI** | `spf13/cobra` | Subcommands, flags |
