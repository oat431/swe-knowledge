# Go Testing Checklist

> The **Go testing** checklist — everything from `testing.T` to fuzzing, benchmarks, and coverage.
> Go ships with a first-class testing framework in the standard library. No framework wars — just `go test`.
> Companion to [[qa]] (general testing strategy). Fiber v3 specifics: [[fiber-v3-api]].
> Requires Go 1.18+ for fuzzing; Go 1.22+ for Fiber v3 projects.
> Last updated: 2026-08-07

---

## 1. Project & Test Organization

- [ ] **`*_test.go` alongside source** — test files live in the same package directory. `foo.go` → `foo_test.go`. Go toolchain discovers them automatically.
- [ ] **Same package vs external test package** — same package (`package foo`) tests private members. External (`package foo_test`) tests only the public API. Use external for handler/integration tests to enforce the contract boundary.
- [ ] **`testdata/` directory** — Go toolchain ignores it. Store fixtures, golden files, sample inputs here. Access via `filepath.Join("testdata", "file.json")` — works regardless of working directory.
- [ ] **`go test ./...` from module root** — runs all tests in all packages. CI must use this, not `go test .` (misses sub-packages).
- [ ] **`t.Parallel()` at top of test** — opt in to parallel execution. Not default. Tests without it run sequentially within their package.

---

## 2. testing.T Lifecycle

- [ ] **`t.Fatal()` / `t.Fatalf()`** — marks test failed AND stops execution immediately. Use when continuing makes no sense (nil pointer, missing dependency).
- [ ] **`t.Error()` / `t.Errorf()`** — marks test failed but continues. Use for collecting multiple failures in one test.
- [ ] **`t.Skip()` / `t.Skipf()`** — skip with reason. Use for environment-dependent tests (`t.Skip("requires DATABASE_URL")`) behind build tags or env checks.
- [ ] **`t.Helper()`** — call in helper functions so failure reports point to the *caller*, not the helper. Critical for custom assertion helpers.
- [ ] **`t.Cleanup(func())`** — register teardown. Runs after test completes (pass or fail). LIFO order. Prefer over `defer` for test-level resources (DB connections, temp dirs, goroutine cancellation).
- [ ] **`t.Setenv(key, value)`** — sets env var, auto-restores after test. No manual `os.Unsetenv` in cleanup.
- [ ] **`t.TempDir()`** — creates temp directory, auto-cleaned. No `os.MkdirTemp` + manual `os.RemoveAll`.
- [ ] **`t.Log()` / `t.Logf()`** — only printed when test fails or `-v` flag used. Use for debug context, not noise.
- [ ] **`t.Failed()`** — check if test has already failed. Useful in cleanup to conditionally dump state.

---

## 3. Table-Driven Tests

- [ ] **Struct slice pattern** — the idiomatic Go way:
```go
tests := []struct {
    name     string
    input    Input
    expected Output
}{
    {"empty input", Input{}, Output{}},
    {"single item", Input{ID: 1}, Output{Name: "one"}},
    {"boundary max", Input{Size: math.MaxInt32}, Output{Truncated: true}},
}
for _, tt := range tests {
    t.Run(tt.name, func(t *testing.T) {
        got := DoThing(tt.input)
        if got != tt.expected {
            t.Errorf("DoThing(%v) = %v, want %v", tt.input, got, tt.expected)
        }
    })
}
```
- [ ] **Every case has a `name` field** — unnamed cases = mystery failures in CI logs.
- [ ] **Boundary values explicit** — min, min+1, nominal, max-1, max, overflow, zero-value, nil.
- [ ] **Error cases included** — invalid input, edge states, timeout scenarios. Happy path only = incomplete.
- [ ] **`tt := tt` NOT needed in Go 1.22+** — loop variable capture fixed. Older Go: capture to avoid goroutine closure bugs.

---

## 4. Subtests (t.Run)

- [ ] **`t.Run(name, func)`** — creates a named subtest. Appears as `TestFoo/subcase` in output. Filterable with `go test -run TestFoo/subcase`.
- [ ] **Subtests can fail independently** — parent passes even if subtests fail (unless `t.Run` result is checked).
- [ ] **Parallel subtests** — `t.Parallel()` inside `t.Run` goroutine. Parent `t.Run` blocks until all parallel children finish.
- [ ] **Setup/teardown scoping** — `t.Cleanup()` registered inside `t.Run` only applies to that subtest.
- [ ] **Nested subtests** — `t.Run` inside `t.Run` is valid. Use for multi-level organization (e.g., `TestHandler/GET_/users/valid_token`).

---

## 5. Interfaces & Mocking

- [ ] **Accept interfaces, return structs** — the Go proverb. Callers depend on behavior (interface), not implementation.
- [ ] **Interfaces defined at consumer** — not at the provider. `internal/handler/` defines the `UserRepository` interface it needs; `internal/repository/` implements it. Avoids tight coupling.
- [ ] **Hand-written mocks first** — small interfaces (1–3 methods) are faster to mock by hand than to generate. Name them `MockXxx` in `_test.go`.
- [ ] **`gomock` / `mockgen`** — for larger interfaces or strict call-count verification:
```go
//go:generate mockgen -destination=mocks/user_repo.go -package=mocks . UserRepository
```
- [ ] **`testify/mock`** — alternative with fluent API: `m.On("GetUser", mock.Anything, 1).Return(&User{Name: "alice"}, nil)`.
- [ ] **`httptest` as boundary mock** — mock external HTTP dependencies with `httptest.NewServer`, not by replacing the HTTP client. See §8.
- [ ] **Clock abstraction** — inject `func() time.Time` or a `Clock` interface. Tests control time without `time.Sleep`.
- [ ] **No mock of internal logic** — mock the boundary (DB, HTTP, filesystem, clock). Mocking the thing under test = testing the mock.

---

## 6. Race Detector (-race)

- [ ] **Always enable in CI** — `go test -race ./...`. Non-negotiable for any concurrent code.
- [ ] **Enable locally during development** — `go test -race ./...` on every run, not just CI. Races found late cost 10x to debug.
- [ ] **`-race` adds overhead** — ~2-10x slower, ~5-10x more memory. Accept it; races in production cost more.
- [ ] **Common race patterns caught**:
  - Shared slice/map written from multiple goroutines without mutex
  - Closing a channel that another goroutine may write to
  - Reading a struct field written by another goroutine without sync
  - `t.Parallel()` tests sharing package-level state
- [ ] **`sync.RWMutex`, `sync.Mutex`, `atomic`** — the fix primitives. Choose `atomic` for simple counters/flags; mutex for complex critical sections.
- [ ] **`-count=10` to surface flaky races** — `go test -race -count=10 ./...`. Runs 10 times; intermittent races become visible.
- [ ] **Race in test code itself** — `t.Parallel()` tests sharing `t` methods incorrectly, or goroutines in tests not joined with `sync.WaitGroup`.

---

## 7. Benchmarks (testing.B)

- [ ] **`BenchmarkXxx(b *testing.B)`** — function name prefix `Benchmark`, parameter `*testing.B`.
- [ ] **`b.N` loop** — the function under test goes inside `for i := 0; i < b.N; i++`. Go adjusts `N` automatically.
- [ ] **`b.ResetTimer()`** — call after expensive setup that shouldn't be measured.
- [ ] **`b.ReportAllocs()`** — reports allocations per operation. Critical for hot-path optimization.
- [ ] **`b.SetBytes(n)`** — reports throughput (MB/s). Useful for serialization, compression, hashing benchmarks.
- [ ] **`b.RunParallel(func(pb *testing.PB))`** — parallel benchmark. Measures throughput under concurrent load.
- [ ] **`go test -bench=. -benchmem ./...`** — run all benchmarks, report memory. `-benchtime=5s` for longer runs.
- [ ] **`-bench=. -count=5`** — multiple runs for statistical significance. Use `benchstat` to compare:
```bash
go test -bench=. -count=5 ./... | tee old.txt
# make changes
go test -bench=. -count=5 ./... | tee new.txt
benchstat old.txt new.txt
```
- [ ] **No benchmarks in CI gate** — benchmarks measure, they don't assert. Run nightly or on demand, compare with `benchstat`.

---

## 8. Fuzzing (Go 1.18+)

- [ ] **`FuzzXxx(f *testing.F)`** — function name prefix `Fuzz`, parameter `*testing.F`.
- [ ] **`f.Add(seed)`** — seed corpus. Provide representative inputs (empty, typical, boundary).
- [ ] **`f.Fuzz(func(t *testing.T, input string))`** — the fuzz target. Go generates random inputs based on seed types.
- [ ] **Supported types** — `[]byte`, `string`, `bool`, `int`, `int8`–`int64`, `uint`, `uint8`–`uint64`, `float32`, `float64`, `time.Time`.
- [ ] **`go test -fuzz=FuzzParse -fuzztime=30s ./...`** — run fuzzer for 30 seconds. Crashes saved to `testdata/fuzz/FuzzParse/`.
- [ ] **Crash reproduction** — failing inputs become regression test cases. `go test ./...` replays them automatically.
- [ ] **Fuzz for parsers, decoders, validators** — anywhere untrusted input is processed. SQL, JSON, custom formats, protocol buffers.
- [ ] **Shared corpus** — `testdata/fuzz/` committed to git. Corpus grows across CI runs.
- [ ] **`f.Add` in testdata** — read corpus files from `testdata/fuzz/FuzzXxx/` to bootstrap with known-good and known-bad inputs.

---

## 9. httptest (HTTP Testing)

- [ ] **`httptest.NewServer(handler)`** — spins up a real HTTP server on a random port. Use for integration tests of HTTP clients.
- [ ] **`httptest.NewRecorder()`** — captures response without a real server. Use for unit-testing handlers.
- [ ] **Handler unit test pattern** (Fiber v3 with httptest):
```go
func TestGetUser(t *testing.T) {
    app := fiber.New()
    app.Get("/users/:id", handler.GetUser)

    req := httptest.NewRequest(http.MethodGet, "/users/123", nil)
    resp, err := app.Test(req)
    if err != nil {
        t.Fatal(err)
    }
    if resp.StatusCode != http.StatusOK {
        t.Errorf("status = %d, want %d", resp.StatusCode, http.StatusOK)
    }
    body, _ := io.ReadAll(resp.Body)
    // assert body
}
```
- [ ] **`httptest.Server.Client()`** — returns an `*http.Client` configured to trust the test server's TLS cert (for `httptest.NewTLSServer`).
- [ ] **Mock external APIs** — `httptest.NewServer` with a stub handler to simulate third-party responses, error codes, slow responses.
- [ ] **Timeout testing** — use `httptest.Server` with a handler that `time.Sleep`s to verify client timeout behavior.

---

## 10. testify (Assertions & Suites)

- [ ] **`require` vs `assert`** — `require.Equal(t, a, b)` stops the test on failure (like `t.Fatal`). `assert.Equal(t, a, b)` continues (like `t.Error`). Choose deliberately.
- [ ] **`require.NoError(t, err)`** — the most common assertion. Stops immediately if err is non-nil.
- [ ] **`assert.JSONEq(t, expected, actual)`** — JSON deep-equal ignoring key order. Better than string comparison for JSON responses.
- [ ] **`assert.ElementsMatch(t, expected, actual)`** — slice equality ignoring order.
- [ ] **`assert.Contains(t, haystack, needle)`** — substring/slice membership.
- [ ] **`suite.Suite`** — for test classes with shared setup/teardown. Use sparingly — table-driven + `t.Cleanup` is usually cleaner.
- [ ] **`mock.Mock`** — fluent mock framework. `On()`, `Return()`, `AssertExpectations(t)`. Pairs well with `testify/assert`.
- [ ] **Don't over-rely on testify** — standard library `if got != want { t.Errorf(...) }` is fine for simple comparisons. testify shines for complex assertions (JSON, nested structs, error types).

---

## 11. Coverage

- [ ] **`go test -cover ./...`** — basic coverage percentage per package.
- [ ] **`go test -coverprofile=coverage.out ./...`** — generates coverage profile.
- [ ] **`go tool cover -html=coverage.out`** — opens browser with line-by-line coverage highlighting. Green = covered, red = not.
- [ ] **`go tool cover -func=coverage.out`** — per-function coverage. Find the uncovered functions.
- [ ] **Coverage target: 80% on business logic** — not total. Handler wiring, main.go, and trivial getters don't need coverage.
- [ ] **`-covermode=atomic`** — required with `-race`. Default `set` is wrong for parallel tests.
- [ ] **CI coverage gate** — block on *decreasing* coverage, not on a magic number. Use `goveralls` or Codecov for PR diff coverage.
- [ ] **Coverage ≠ correctness** — 100% coverage with no assertions is useless. Mutation testing (§13) tells you if coverage is meaningful.

---

## 12. Parallel Tests

- [ ] **`t.Parallel()`** — marks test safe to run concurrently with other parallel tests. Default is sequential.
- [ ] **No shared mutable state** — parallel tests must not share package-level variables, DB rows, or files. Each test gets its own scope.
- [ ] **`t.Run` + `t.Parallel()`** — subtests can be parallelized independently. Parent waits for all children.
- [ ] **Loop variable capture (Go < 1.22)** — `tt := tt` before the `t.Run` goroutine. Go 1.22+ fixes this.
- [ ] **`-parallel=N`** — controls max parallel tests (default: `GOMAXPROCS`). Increase for I/O-bound tests.
- [ ] **DB test isolation** — each parallel test uses a unique schema, database, or transaction. `testcontainers-go` with per-test containers, or unique table names.
- [ ] **`sync.WaitGroup` for goroutine tests** — ensure all goroutines complete before test exits. Leaked goroutines = race detector noise and flaky tests.

---

## 13. Common Patterns

### Test Setup Helper
```go
func setupTestDB(t *testing.T) *sql.DB {
    t.Helper()
    db, err := sql.Open("postgres", "postgres://localhost:5432/test?sslmode=disable")
    if err != nil {
        t.Fatal(err)
    }
    t.Cleanup(func() { db.Close() })
    return db
}
```

### Golden File Testing
```go
func TestRender(t *testing.T) {
    got := Render(input)
    golden := filepath.Join("testdata", "expected.html")
    if *update {
        os.WriteFile(golden, got, 0644)
    }
    want, _ := os.ReadFile(golden)
    if !bytes.Equal(got, want) {
        t.Errorf("Render() output differs from golden file")
    }
}
var update = flag.Bool("update", false, "update golden files")
```

### Context Cancellation in Tests
```go
func TestWithTimeout(t *testing.T) {
    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    t.Cleanup(cancel)
    result, err := DoWork(ctx)
    require.NoError(t, err)
    assert.Equal(t, "done", result)
}
```

### Integration Test with Testcontainers
```go
func TestIntegration(t *testing.T) {
    if testing.Short() {
        t.Skip("skipping integration test in short mode")
    }
    ctx := context.Background()
    req := testcontainers.ContainerRequest{
        Image:        "postgres:16-alpine",
        ExposedPorts: []string{"5432/tcp"},
        Env:          map[string]string{"POSTGRES_PASSWORD": "test"},
        WaitingFor:   wait.ForLog("database system is ready to accept connections"),
    }
    c, err := testcontainers.GenericContainer(ctx, testcontainers.GenericContainerRequest{
        ContainerRequest: req, Started: true,
    })
    require.NoError(t, err)
    t.Cleanup(func() { c.Terminate(ctx) })
    // use c.Endpoint(ctx, "5432/tcp") to connect
}
```

- [ ] **`testing.Short()` gate** — skip slow tests with `go test -short ./...`. CI runs full suite; local dev runs `-short`.
- [ ] **`go test -run TestFoo -v ./...`** — run a single test with verbose output. Essential for debugging.
- [ ] **`go test -count=1 ./...`** — disables test caching. Use when tests depend on external state that changed.
- [ ] **Build tags for test isolation** — `//go:build integration` on integration test files. Run with `go test -tags=integration ./...`.

---

## 14. Pitfalls & Gotchas

- [ ] **`init()` side effects** — `init()` runs when the package is imported, including by tests. Database connections, HTTP calls, file reads in `init()` = test pollution. Move to explicit `New()` constructors.
- [ ] **`os.Exit()` in tested code** — untestable. Return errors instead; let `main()` decide exit codes.
- [ ] **`time.Sleep` in tests** — flaky and slow. Use channels, `context`, or inject a clock interface. `time.Sleep(100*time.Millisecond)` is a red flag.
- [ ] **`t.Parallel()` with shared state** — the #1 source of flaky tests. If test A modifies a package-level var and test B reads it, both running parallel = data race and nondeterminism.
- [ ] **Goroutine leaks** — goroutines spawned in tests that outlive the test. Use `goleak` (`go.uber.org/goleak`) in `TestMain` to detect: `goleak.VerifyTestMain(m)`.
- [ ] **`t.Fatal` in goroutine** — `t.Fatal` calls `runtime.Goexit()`, which only exits the current goroutine. In a spawned goroutine, it doesn't fail the test. Use channels or `sync.WaitGroup` + `t.Error`.
- [ ] **Missing `t.Helper()`** — without it, assertion helpers report failures at the helper's line, not the caller's. Makes debugging painful.
- [ ] **`go test` caching** — Go caches test results. If tests depend on env vars, files, or DB state that change between runs, use `-count=1` to force re-execution.
- [ ] **Race in table-driven + `t.Parallel()`** — pre-Go 1.22: `for _, tt := range tests { t.Run(tt.name, func(t *testing.T) { t.Parallel(); use(tt) }) }` captures the *last* `tt`. Fix: `tt := tt` before `t.Run`.
- [ ] **`TestMain` misuse** — `TestMain(m *testing.M)` replaces the default test runner. Must call `m.Run()` and `os.Exit(code)`. Common mistake: forgetting `os.Exit(code)` → exit code always 0, CI passes even on failures.
- [ ] **Comparing errors with `==`** — use `errors.Is(err, target)` for wrapped errors. `err == ErrNotFound` fails when the error is wrapped with `fmt.Errorf("...: %w", err)`.
- [ ] **`panic` in library code** — untestable and rude to callers. Return errors. Only `panic` in `main()` or truly unrecoverable situations (corrupt binary state).

---

## 15. CI & Tooling

- [ ] **`golangci-lint` in CI** — `golangci-lint run ./...`. Catches issues tests miss (unused variables, error handling, naming).
- [ ] **`go vet ./...`** — static analysis. Runs automatically with `go test` but run explicitly in CI for early failure.
- [ ] **`staticcheck ./...`** — deeper static analysis. Finds concurrency bugs, performance issues, API misuse.
- [ ] **`go test -race -coverprofile=coverage.out ./...`** — the CI command. Race detection + coverage in one pass.
- [ ] **`goveralls` or Codecov** — upload coverage. PR comments show diff coverage.
- [ ] **`-timeout=10m`** — default test timeout is 10m. Increase for integration suites; decrease to catch hung tests faster.
- [ ] **`go test -json ./...`** — machine-readable output. CI tools (GitHub Actions, GitLab) parse it for annotations.

---

## Quick Sanity Check

- [ ] `go test -race -cover ./...` passes
- [ ] All table-driven tests have named cases
- [ ] `t.Cleanup()` for all acquired resources (DB, files, goroutines)
- [ ] No `time.Sleep` in tests (use channels, context, or clock injection)
- [ ] No shared mutable state in parallel tests
- [ ] `testing.Short()` gates integration tests
- [ ] `t.Helper()` on all custom assertion functions
- [ ] Error comparisons use `errors.Is()`, not `==`
- [ ] Fuzz tests exist for parsers/decoders/validators
- [ ] Coverage ≥ 80% on business logic (verified with `go tool cover`)
- [ ] `goleak` or equivalent detects goroutine leaks
- [ ] CI runs `-race` and `-coverprofile`

---

## Project Tier Scoping Matrix

> **How to use:** Pick your tier, focus on ✅ (required) and 🟡 (recommended) sections. Skip ❌.
>
> **Legend:** ✅ Required · 🟡 Recommended / partial · ❌ Skip

| # | Section | 🧪 POC | 🔧 Prototype | 🏠 Internal | 🟢 Small Prod | 🔵 Medium Prod | 🟣 Production Grade | 🔴 Mission-Critical |
|---|---------|:------:|:------------:|:-----------:|:-------------:|:--------------:|:-------------------:|:-------------------:|
| 1 | Project & Test Organization | 🟡 same dir | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 2 | testing.T Lifecycle | 🟡 Fatal/Error | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 3 | Table-Driven Tests | ❌ | 🟡 happy path | ✅ | ✅ + boundaries | ✅ | ✅ | ✅ + formal |
| 4 | Subtests | ❌ | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 5 | Interfaces & Mocking | ❌ | 🟡 hand-written | ✅ | ✅ + gomock | ✅ + boundary mocks | ✅ + full DI | ✅ + formal |
| 6 | Race Detector | ❌ | 🟡 on CI | ✅ always | ✅ + count=10 | ✅ | ✅ + nightly stress | ✅ + formal |
| 7 | Benchmarks | ❌ | ❌ | 🟡 hot paths | 🟡 + benchstat | ✅ + regression | ✅ + CI trend | ✅ + SLO |
| 8 | Fuzzing | ❌ | ❌ | ❌ | 🟡 parsers | ✅ decoders | ✅ + shared corpus | ✅ + continuous |
| 9 | httptest | ❌ | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 10 | testify | ❌ | 🟡 | 🟡 | ✅ | ✅ | ✅ | ✅ |
| 11 | Coverage | ❌ | 🟡 manual | 🟡 basic | ✅ 80% biz logic | ✅ + CI gate | ✅ + mutation | ✅ + formal |
| 12 | Parallel Tests | ❌ | ❌ | 🟡 | ✅ | ✅ + DB isolation | ✅ + sharding | ✅ + formal |
| 13 | Common Patterns | 🟡 | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 14 | Pitfalls Awareness | 🟡 | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ + review |
| 15 | CI & Tooling | ❌ | 🟡 basic | ✅ | ✅ + coverage | ✅ + staticcheck | ✅ + full pipeline | ✅ + signed |

---

## Sources

- Complements [[qa]] (general testing strategy), [[fiber-v3-api]] (Fiber handler testing specifics).
- Go standard library: `testing`, `net/http/httptest`, `os/exec`, `context`.
- Third-party: `github.com/stretchr/testify`, `go.uber.org/goleak`, `github.com/testcontainers/testcontainers-go`.
- `golang.org/x/tools/cmd/benchstat` for benchmark comparison.
- Effective Go — Testing: https://go.dev/doc/effective_go#testing
