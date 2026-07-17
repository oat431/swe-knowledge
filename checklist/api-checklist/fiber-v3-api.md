# Fiber v3 API Checklist

> Go + Fiber v3 specific companion to [[API Launch]]. Tick the general checklist first, then this one for Fiber-specific concerns.
> Fiber v3 requires Go 1.22+. Last updated: 2026-06-30

---

## Project Setup

- [ ] **Go 1.22+** — Fiber v3 minimum. `go mod init github.com/you/project`
- [ ] **Module path** — lowercase, no underscores, follows `domain.com/user/repo` convention
- [ ] **`go.sum` committed** — reproducible builds
- [ ] **Linter** — `golangci-lint` with `.golangci.yml`. At minimum: `errcheck`, `gosimple`, `govet`, `ineffassign`, `staticcheck`, `unused`
- [ ] **Air for hot reload** — `go install github.com/air-verse/air@latest`. `.air.toml` at project root

---

## App Structure

```
cmd/
└── server/
    └── main.go          ← Entry point. Wires everything. Starts server.
internal/
├── handler/              ← HTTP handlers (thin — parse request, call service, write response)
├── service/              ← Business logic
├── repository/           ← Data access
├── middleware/            ← Custom middleware (if standard middleware package doesn't cover it)
├── model/                ← Domain types, request/response DTOs
└── config/               ← Config loading
```

- [ ] **Package-by-feature, not by-layer** — `internal/` grouped by domain (order/, user/), not by layer (handlers/, services/). Avoids import cycles.
- [ ] **`internal/` for private packages** — Go compiler enforces. Packages outside `internal/` CAN'T import them.
- [ ] **`cmd/` is thin** — `main.go` creates deps, wires middleware, calls `app.Listen()`. No business logic.

---

## Fiber App Setup

- [ ] **Error handling** — Fiber v3 `Config.ErrorHandler` returns JSON. Custom handler for all routes:
```go
app := fiber.New(fiber.Config{
    ErrorHandler: func(c fiber.Ctx, err error) error {
        code := fiber.StatusInternalServerError
        if e, ok := err.(*fiber.Error); ok { code = e.Code }
        return c.Status(code).JSON(fiber.Map{"error": err.Error()})
    },
})
```

- [ ] **`fiber.Config.Immutable`** — set `true`. Prevents accidental config mutation post-startup.
- [ ] **`fiber.Config.StructValidator`** — `gookit/validate` is Fiber's default. Custom validator: `fiber.SetValidator(customValidator)`.
- [ ] **Trusted proxies** — `fiber.Config.TrustedProxies` or `fiber.Config.TrustedProxy` for X-Forwarded-* headers.
- [ ] **Prefork** — `fiber.Config.Prefork: false` (default). Only enable if you know you need it (saturates all cores with child processes).
- [ ] **Body limit** — `fiber.Config.BodyLimit: 10 * 1024 * 1024` (10MB). Prevent OOM on large uploads.
- [ ] **Disable startup banner** — `fiber.Config.DisableStartupMessage: true` (production).

---

## Middleware Chain

- [ ] **Request ID** — `fiber/middleware/requestid`. Custom `Generator` using UUID v7 or similar.
- [ ] **CORS** — `fiber/middleware/cors`. Specific origins per environment. Not `*`.
- [ ] **Recover** — `fiber/middleware/recover`. Built-in. Panics caught, written to stderr, returns 500.
- [ ] **Logger** — `fiber/middleware/logger`. Structure: `"${time} ${status} ${method} ${path} ${latency}"`. Skip health check noise: `Next: skipHealthCheck`.
- [ ] **Helmet** — `fiber/middleware/helmet`. Security headers. Enable at minimum: `XSSProtection`, `ContentTypeNosniff`, `XFrameOptions`, `HSTS`.
- [ ] **Rate Limiter** — `fiber/middleware/limiter`. Per-IP. `Max: 100`, `Expiration: 1 * time.Minute`. Use Redis store for multi-instance.
- [ ] **Compression** — `fiber/middleware/compress`. Level: `compress.LevelDefault`. Skip tiny responses.
- [ ] **ETag** — `fiber/middleware/etag`. Reduces bandwidth for unchanged responses.
- [ ] **Cache** — `fiber/middleware/cache`. Short TTL (5-30s) for semi-dynamic content. Redis store for multi-instance.

Middleware order matters:
```
RequestID → Logger → Recover → Helmet → CORS → Compress → RateLimiter → Routes
```

---

## Routing

- [ ] **Group routes by version** — `v1 := app.Group("/api/v1")`
- [ ] **Route naming** — `v1.Get("/users/:id", handler.GetUser).Name("get-user")`
- [ ] **`fiber.All` for 405** — `app.All("/api/v1/users", func(c fiber.Ctx) error { return c.SendStatus(405) })` when a path has specific methods.
- [ ] **404 handler** — Custom `app.Use(func(c fiber.Ctx) error { return c.Status(404).JSON(...) })` at the end.
- [ ] **Validation middleware** — custom or `fiber/middleware/validate` route-level validation before handler.
- [ ] **`c.Params` always validated** — never trust route params. `id := c.Params("id")` → validate format immediately.
- [ ] **`c.ParamsInt` / `c.ParamsFloat`** — use Fiber's typed param helpers instead of manual conversion.

---

## Request / Response

- [ ] **`c.BodyParser`** — parses JSON. Check error: `if err := c.BodyParser(&req); err != nil { return fiber.ErrBadRequest }`
- [ ] **`c.QueryParser`** — for GET query params. Same pattern.
- [ ] **`c.Locals` for request-scoped values** — user context from auth middleware, request ID, logger instance. Don't use globals.
- [ ] **`c.SendStatus` for empty responses** — `c.SendStatus(204)` cleaner than `c.Status(204).JSON(nil)`
- [ ] **Consistent JSON casing** — `fiber.Config.JSONEncoder: json.Marshal` (Go's default is PascalCase for exported fields). Use `json:"camelCase"` tags everywhere.
- [ ] **Pagination** — query params `?page=1&limit=20`. Defaults enforced. Response includes `total`, `page`, `totalPages`.
- [ ] **Content negotiation** — `c.Accepts("application/json")` check if your API supports multiple formats.

---

## Authentication

- [ ] **JWT middleware** — `fiber/middleware/jwt`. `jwtware.New(jwtware.Config{SigningKey: ...})`. RS256/ES256, not HMAC in production.
- [ ] **Route protection** — `v1.Use(jwtMiddleware)` on protected groups. Public routes (health, login) before middleware.
- [ ] **`c.Locals("user")`** — JWT middleware stores claims. Type-assert in handler: `user := c.Locals("user").(*jwt.Token)`.
- [ ] **API key middleware** — custom `func(c fiber.Ctx) error` that checks `X-API-Key` header. For service-to-service calls.

---

## Config & Secrets

- [ ] **Viper** — `github.com/spf13/viper`. `config.yaml` defaults, env overrides, no secrets in config files.
- [ ] **`.env` gitignored** — always. `.env.example` committed.
- [ ] **Config struct** — `type Config struct { Server ServerConfig; DB DBConfig }` loaded once at startup. Immutable.
- [ ] **Graceful shutdown** — `app.ShutdownWithTimeout(30 * time.Second)`. Listen for `os.Signal` (SIGINT, SIGTERM).
- [ ] **Health check** — `app.Get("/health", func(c fiber.Ctx) error { return c.SendStatus(200) })`. Simple first. Add DB ping if needed.

---

## Database (SQL)

- [ ] **sqlx over GORM** — Unless you need change tracking/complex ORM features. `sqlx` is lighter, faster, closer to SQL.
- [ ] **Connection pool** — `db.SetMaxOpenConns(25)`, `db.SetMaxIdleConns(10)`, `db.SetConnMaxLifetime(5 * time.Minute)`.
- [ ] **Migrations** — `golang-migrate/migrate` or `pressly/goose`. Versioned SQL files. Not auto-migrate.
- [ ] **Context propagation** — `db.QueryContext(ctx, ...)` with context from `c.UserContext()`. Respects timeouts and cancellation.
- [ ] **Repository pattern** — interfaces in `internal/repository/`, implementations in same package or sub-package. Injectable to services.

---

## Logging

- [ ] **slog (stdlib)** — `log/slog`. Go 1.21+. Structured JSON output in production (`slog.NewJSONHandler`).
- [ ] **Log levels** — `slog.LevelDebug` dev, `slog.LevelInfo` prod. Configurable via `LOG_LEVEL` env.
- [ ] **Request-scoped logger** — middleware adds `requestID`, `userID` to context. Handlers use `slog.InfoContext(ctx, ...)`.
- [ ] **Redact secrets** — `slog.Handler` wrapper that masks password/token fields. Or don't log them.

---

## Testing

- [ ] **`testing` + `testify`** — Go stdlib `testing` + `github.com/stretchr/testify` for `assert` and `require`.
- [ ] **Table-driven tests** — idiomatic Go. `tests := []struct { name; input; expected }`. Loop + `t.Run`.
- [ ] **`httptest`** — `fiber.Test(app)` creates a test request. No server needed. Fast.
- [ ] **Handler tests** — `app.Test(httptest.NewRequest(...))`. Expect status code + response body.
- [ ] **Integration tests** — `TestMain` sets up DB (Testcontainers or Docker), runs migrations, runs tests, tears down.
- [ ] **Test helpers** — `internal/testutil/` for shared setup (test app, test DB, fixtures).
- [ ] **Parallel tests** — `t.Parallel()` where tests are independent. Faster CI.

---

## Observability

- [ ] **OpenTelemetry** — `go.opentelemetry.io/otel`. Fiber OTel middleware: `otelgofiber`. Trace propagation via W3C TraceContext.
- [ ] **Metrics** — Prometheus via `fiber/middleware/adaptor` + `promhttp`. Or custom middleware that counts requests/errors.
- [ ] **pprof** — `net/http/pprof` on separate port (`:6060`) in dev. Firewalled in prod.

## OpenAPI / Swagger

- [ ] **swaggo/swag** — `go install github.com/swaggo/swag/cmd/swag@latest`. Annotations on handlers: `@Summary`, `@Param`, `@Success`, `@Failure`, `@Router`.
- [ ] **swag init** — `swag init -g cmd/server/main.go -o docs/`. Generates `docs/` (commit it).
- [ ] **gofiber/swagger** — `fiber/middleware/swagger`. `app.Get("/swagger/*", swagger.HandlerDefault)`. Serves Swagger UI from generated `docs/`.
- [ ] **`@Security ApiKeyAuth`** — marks endpoints needing JWT. `@SecurityDefinitions` in main.go for global auth scheme.
- [ ] **`@Tags`** on handler functions** — groups endpoints in Swagger UI.

## Build & Deploy

- [ ] **Multi-stage Dockerfile** — `golang:1.22-alpine` build, `scratch` or `alpine:3.20` runtime. `CGO_ENABLED=0` for static binary.
- [ ] **Binary size** — `-ldflags="-s -w"` strips debug info. `upx` if you need smaller.
- [ ] **`GOOS` / `GOARCH`** — cross-compile: `GOOS=linux GOARCH=amd64 go build`. CI matrix for target platforms.
- [ ] **Health check endpoint** — Docker/K8s probes hit `/health`. Return 200 only when app is ready.

---

## Quick Sanity Check

- [ ] `fiber.Config.Immutable: true` — config frozen after start
- [ ] `fiber.Config.BodyLimit` set — no unbounded request bodies
- [ ] `fiber.Config.TrustedProxies` configured — behind reverse proxy
- [ ] Recover middleware before all routes
- [ ] JWT middleware on protected route groups (not individual routes)
- [ ] `c.UserContext()` passed to all downstream calls (DB, external APIs)
- [ ] `go.sum` committed, `go mod tidy` runs in CI
- [ ] Graceful shutdown with timeout
- [ ] No global variables holding request-scoped state
- [ ] `slog` JSON output in production

---

## Sources

- Fiber v3 docs — https://docs.gofiber.io/
- `[[API Launch]]` — general API checklist (tick first)
- `[[03 Authentication]]`, `[[03 API Security]]` — auth patterns
