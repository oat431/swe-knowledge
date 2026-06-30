# Rust + Axum API Checklist

> Rust + Axum companion to [[API Launch]]. Tick the general checklist first. Axum is built on tokio + tower + hyper — the standard Rust async stack.

---

## Project Setup

- [ ] **Rust toolchain** — `rustup default stable`. `cargo new project-name`
- [ ] **Dependencies** — `Cargo.toml`:
```toml
[dependencies]
axum = "0.8"
tokio = { version = "1", features = ["full"] }
serde = { version = "1", features = ["derive"] }
serde_json = "1"
tower-http = { version = "0.6", features = ["cors", "trace", "compression", "limit"] }
sqlx = { version = "0.8", features = ["runtime-tokio", "postgres"] }
tracing = "0.1"
tracing-subscriber = { version = "0.3", features = ["json", "env-filter"] }
```
- [ ] **Clippy** — `cargo clippy -- -D warnings`. Strict linting. CI gate.
- [ ] **`rustfmt`** — `rustfmt.toml` at project root. `edition = "2021"`, `max_width = 120`.
- [ ] **`cargo-watch`** — `cargo install cargo-watch`. `cargo watch -x run` for dev hot-reload.
- [ ] **`.cargo/config.toml`** — `[build] rustflags = ["-D", "warnings"]`. No warnings in CI.

---

## Project Structure

```
src/
├── main.rs              ← Bootstrap. Tokio runtime. Router. Shutdown.
├── config.rs            ← Config loading + validation (envy/config crate)
├── error.rs             ← AppError type + IntoResponse impl
├── router.rs            ← All route definitions. Thin — calls handlers.
├── handlers/
│   ├── mod.rs
│   ├── users.rs         ← Extractors → service → response
│   └── orders.rs
├── services/
│   ├── mod.rs
│   ├── users.rs         ← Business logic. No HTTP concerns.
│   └── orders.rs
├── repositories/
│   ├── mod.rs
│   └── users.rs         ← SQL queries. Returns domain types.
├── models/
│   ├── mod.rs
│   ├── user.rs          ← Domain types + serde DTOs
│   └── order.rs
└── middleware/
    ├── mod.rs
    └── auth.rs          ← JWT extraction layer
```

- [ ] **`main.rs` is thin** — creates router, starts server, handles shutdown signal. ~30 lines.
- [ ] **`lib.rs` if integration tests needed** — `pub mod` all modules. Integration tests import `use crate::`.

---

## Axum App Setup

- [ ] **Tokio runtime** — `#[tokio::main]`. Multi-threaded by default. Work-stealing scheduler.
- [ ] **Router** — `axum::Router::new().route("/api/v1/users", get(handlers::users::list))`.
- [ ] **Nest routers by domain** — `let app = Router::new().nest("/api/v1/users", users::router()).nest("/api/v1/orders", orders::router())`.
- [ ] **State** — `axum::extract::State`. `Arc<AppState>` shared across handlers. Contains DB pool, config. `Router::new().with_state(state)`.
- [ ] **Shutdown** — `axum::serve(listener, app).with_graceful_shutdown(shutdown_signal())`. Catch SIGTERM.
- [ ] **Graceful period** — `tokio::select!` between server and shutdown signal. Drain connections within timeout.

---

## Extractors (Request Parsing)

- [ ] **`axum::Json<T>`** — parses JSON body. `Json(payload): Json<CreateUserRequest>`. Returns 400 on parse failure.
- [ ] **`axum::extract::Path<T>`** — `Path(id): Path<i64>`. Returns 400 on parse failure.
- [ ] **`axum::extract::Query<T>`** — `Query(params): Query<PaginationParams>`. Query string params.
- [ ] **`axum::extract::State`** — `State(state): State<Arc<AppState>>`. Shared application state.
- [ ] **Custom extractors** — `impl FromRequestParts<S> for CurrentUser`. Type-safe, reusable auth extraction.
- [ ] **`axum::extract::Request`** — raw request when needed (middleware compatibility checks).

---

## Responses

- [ ] **`impl IntoResponse`** — handlers return types implementing `IntoResponse`. `Json` for JSON, `StatusCode` for empty, `Html` for templates.
- [ ] **`(StatusCode, Json<T>)`** — explicit status + body. `(StatusCode::CREATED, Json(user))`.
- [ ] **Error responses** — custom `AppError` enum. `impl IntoResponse for AppError`. Maps variants to status codes.
- [ ] **Pagination wrapper** — `Json(PaginatedResponse { data, total, page, total_pages })`.

---

## Error Handling

- [ ] **`AppError` enum** — variants: `NotFound`, `BadRequest`, `Unauthorized`, `Forbidden`, `Internal`.
- [ ] **`impl IntoResponse for AppError`** — single place mapping errors to HTTP responses. Consistent shape: `{ "error": { "code": "NOT_FOUND", "message": "..." } }`.
- [ ] **`impl From<sqlx::Error> for AppError`** — auto-convert DB errors. `RowNotFound` → `AppError::NotFound`.
- [ ] **`impl From<validator::ValidationErrors> for AppError`** — validation errors → 400 with field-level messages.
- [ ] **500 catch-all** — `impl From<anyhow::Error> for AppError` for truly unexpected errors. Log the full error, return sanitized message.

---

## Validation

- [ ] **`validator` crate** — `#[derive(Validate)]` on request DTOs. `#[validate(email)]`, `#[validate(length(min = 8))]`.
- [ ] **Validate in handler** — `payload.validate().map_err(AppError::from)?`. Early return on invalid input.
- [ ] **Custom validators** — `#[validate(custom(function = "validate_password_strength"))]`. Pure functions, unit-testable.

---

## Database (sqlx)

- [ ] **sqlx** over Diesel — sqlx is query-first (write SQL, get typed results). Diesel is ORM-first (DSL, more magic). sqlx is simpler.
- [ ] **`sqlx::PgPool`** — connection pool created once in `main.rs`. `Arc<AppState>` holds it. `PgPool::connect(&config.database_url).await?`.
- [ ] **`sqlx::query_as!(T, ...)`** — compile-time checked SQL. Column names/types validated against DB at build time (requires `DATABASE_URL` env).
- [ ] **`sqlx::query_as::<_, T>(...)`** — runtime-checked. Works without compile-time DB access. Slower compile, same safety at runtime.
- [ ] **Migrations** — `sqlx migrate run`. `.sql` files in `migrations/`. Committed. Run in `main()` before binding listener.
- [ ] **Transactions** — `pool.begin().await?`. `tx.commit().await?`. Auto-rollback on drop if not committed.
- [ ] **Connection pool** — `PgPoolOptions::new().max_connections(20).connect(...)`. Tune per instance.

---

## Authentication (JWT)

- [ ] **jsonwebtoken crate** — `encode`, `decode`. `Header`, `Validation`. RS256 (private key on auth server, public key on resource servers).
- [ ] **Auth middleware** — `axum::middleware::from_fn_with_state(auth::require_auth)`. Runs before handlers. Extracts JWT → validates → injects claims into request extensions.
- [ ] **`CurrentUser` extractor** — `impl FromRequestParts<S> for CurrentUser`. Reads claims from request extensions. Fails 401 if missing.
- [ ] **`RequireRole` extractor** — `RequireRole("admin")`. Like `CurrentUser` but checks role. Returns 403.
- [ ] **Public routes** — auth middleware applied only on protected `Router::nest()` groups, not globally. Health, login are public.

---

## Middleware (tower-http)

- [ ] **CORS** — `tower_http::cors::CorsLayer`. `allow_origin(config.cors_origins)`. `allow_methods`, `allow_headers`.
- [ ] **Compression** — `tower_http::compression::CompressionLayer`. Gzip + Brotli. `compress_br()` for modern browsers.
- [ ] **Request body limit** — `tower_http::limit::RequestBodyLimitLayer::new(10 * 1024 * 1024)`. Applied per-route or globally.
- [ ] **Tracing** — `tower_http::trace::TraceLayer`. Request ID, method, path, status, latency. `on_request`, `on_response`, `on_failure`.
- [ ] **Sensitive headers** — `TraceLayer` defaults to redacting `authorization` and `cookie`. Good. Keep it.
- [ ] **Timeout** — `tower_http::timeout::TimeoutLayer::new(Duration::from_secs(30))`. Per-request timeout.
- [ ] **Rate limiting** — `tower_governor` crate. Per-IP or per-token. Redis backend for multi-instance.

---

## Logging (tracing)

- [ ] **tracing subscriber** — `tracing_subscriber::fmt().json().with_env_filter("info").init()`. JSON in production.
- [ ] **`tracing::instrument`** — `#[instrument(skip(pool))]` on handlers. Auto-logs function entry/exit, args, return. `skip` for sensitive params.
- [ ] **Span context** — request ID, user ID in span. `tracing::Span::current().record("user_id", user.id)`.
- [ ] **`tracing::info!` / `error!` / `warn!`** — structured fields: `info!(user_id = %id, "User created")`. No string interpolation for fields.
- [ ] **Redact secrets** — never log tokens, passwords, API keys. Skip via `#[instrument(skip(password))]`.

---

## Testing

- [ ] **Unit tests** — `#[cfg(test)] mod tests { ... }`. Inline with source. `cargo test`.
- [ ] **Handler tests** — `axum::body::Body`, `axum::http::Request`. `router.oneshot(request).await`. No real server. Fast.
- [ ] **Integration tests** — `tests/` directory. Full app with test DB. `sqlx::migrate!()` in test setup.
- [ ] **Test utilities** — `tests/common/mod.rs`. `async fn test_app() -> (Router, PgPool)`. Shared across integration tests.
- [ ] **`rstest`** — `use rstest::rstest`. Fixtures, parameterized tests. Cleaner than manual `test_case` macros.
- [ ] **`fake` crate** — `use fake::Fake`. Generate realistic test data. `let user: UserParams = fake::Faker.fake()`.

---

## Observability

- [ ] **OpenTelemetry** — `tracing-opentelemetry` layer. Traces exported to OTel collector. W3C trace context propagation.
- [ ] **Metrics** — `tower_http::metrics` or custom middleware. Count requests, errors, latency histograms. Prometheus endpoint on `/metrics`.
- [ ] **Health check** — `axum::Router::new().route("/health", get(|| async { StatusCode::OK }))`. Separate router or path.

---

## Build & Deploy

- [ ] **Release profile** — `[profile.release] lto = true`, `codegen-units = 1`, `opt-level = 3`. Binary size vs compile time trade-off.
- [ ] **`cargo build --release`** — single static binary. No runtime needed (unlike Go, Rust has no GC).
- [ ] **Docker** — Multi-stage: `rust:slim` build, `debian:slim` runtime (need libssl, ca-certificates). Or `FROM scratch` if `tokio` doesn't need libssl.
- [ ] **`dockerignore`** — `target/`, `.git/`. Otherwise context is huge (Rust `target/` can be 10GB+).
- [ ] **sccache** — `cargo install sccache`. Speeds up CI builds 2-3x. Configured in `.cargo/config.toml`.

---

## Quick Sanity Check

- [ ] `cargo clippy -- -D warnings` passes — no warnings in CI
- [ ] `cargo test` passes — all tests green
- [ ] `#[tokio::main]` — async runtime bootstrapped
- [ ] `axum::serve` with graceful shutdown — no dropped connections on SIGTERM
- [ ] `AppState` behind `Arc` — cheap clone, thread-safe
- [ ] `tower-http` middleware layered in correct order (`TraceLayer` first, `CorsLayer`, then `CompressionLayer`)
- [ ] `sqlx` migrations run at startup — `sqlx::migrate!().run(&pool).await?`
- [ ] JWT validation not trusting `alg: none` — `jsonwebtoken::Validation::default()` rejects it
- [ ] Error shape consistent — all errors return `{ "error": { "code": "...", "message": "..." } }`
- [ ] `tracing` JSON format in production — structured logs, not human-readable

---

## Sources

- Axum docs — https://docs.rs/axum/latest/axum/
- `[[API Launch]]` — general API checklist (tick first)
- `[[03 Authentication]]`, `[[03 API Security]]` — auth patterns
