# Resilience Checklist (System-Wide)

> Tick every box before your system survives failure. One dead service should not cascade-kill everything. Deep reference: [[04 Circuit Breaker]], [[04 Bulkhead Pattern]], [[04 Retry & Timeout]].

---

## Circuit Breakers

- [ ] **Circuit breaker on every service-to-service call** → [[04 Circuit Breaker]]
- [ ] Per-destination config — payment gateway stricter than notification service
- [ ] States: closed (normal) → open (fail fast) → half-open (probe) → closed (recovered)
- [ ] Failure threshold: trip after N failures within window (e.g., 5/10 requests fail)
- [ ] Open duration: 30s default, longer for critical/money paths
- [ ] Half-open: permit N probe requests, any failure → re-open immediately
- [ ] Fail fast on open circuit — don't make callers wait for timeout
- [ ] Fallback response returned — cached stale data, degraded response, or domain error. Never null

---

## Bulkheads

- [ ] **Critical services isolated** — separate thread pools or instances → [[04 Bulkhead Pattern]]
- [ ] Payment service can't exhaust threads needed by auth service
- [ ] Thread pool isolation: max concurrent per downstream. Reject when full → circuit breaker picks it up
- [ ] Semaphore isolation: lighter than thread pools. Good for non-blocking calls
- [ ] Instance isolation: critical services on dedicated instances (most isolation, most cost)

---

## Retries

- [ ] **Retry only on idempotent requests** — GET, PUT, DELETE. Never POST without idempotency key → [[04 Retry & Timeout]]
- [ ] Exponential backoff: 100ms → 200ms → 400ms → 800ms
- [ ] Jitter: randomize backoff to avoid thundering herd
- [ ] Max retries: 3. Max total time: shorter than client timeout
- [ ] Retry inside circuit breaker — breaker counts all retries as one failure
- [ ] Retryable exceptions: connection refused, timeout. NOT: 400 Bad Request, 404 Not Found

---

## Timeouts

- [ ] **Timeout on every external call** → [[04 Retry & Timeout]]
- [ ] Connect timeout: 3s. Read timeout: 30s
- [ ] Per-route timeouts: auth 200ms, payment 5s, report 60s
- [ ] Timeout < client timeout — otherwise client retries while server is still processing
- [ ] Timeout + circuit breaker paired — circuit opens before timeout exhausts threads

---

## Fallbacks

- [ ] **Every critical path has a fallback** — not just a 500 → [[04 Circuit Breaker]]
- [ ] Cache: return last known good response (stale-while-revalidate)
- [ ] Degraded: return partial data with `degraded: true` flag
- [ ] Default: return sensible empty/default (`[]`, `0`, `{ "status": "unavailable" }`)
- [ ] Fail closed (allow) vs fail open (block): document per-endpoint decision
- [ ] Fallback logged and metered — know how often you're degraded

---

## Cascading Failure Prevention

- [ ] **No single service failure takes down the system** → [[04 Bulkhead Pattern]]
- [ ] Backpressure: slow consumers signal producers to slow down (reactive streams, queue limits)
- [ ] Load shedding: reject low-priority requests when overloaded (return 503 early)
- [ ] Graceful degradation: if user profile service is down, show homepage without avatar
- [ ] No unbounded queues — bounded queues + rejection policy

---

## Testing Resilience

- [ ] **Chaos tested** — kill a service, verify system degrades gracefully
- [ ] Circuit breaker opens on repeated failure
- [ ] Fallback returned while circuit is open
- [ ] Circuit half-opens after cooldown, probe succeeds, closes
- [ ] Bulkhead contains failure — one service exhaust doesn't affect others
- [ ] Retry exhausts → circuit breaker opens (not infinite retry loop)
- [ ] Timeout fires before client gives up

---

## Observing Resilience

- [ ] **Metrics: circuit state per dependency** (closed/open/half-open) → [[04 API Monitoring]]
- [ ] Metrics: fallback invocation rate, retry count, timeout rate
- [ ] Alerts: any circuit open, fallback rate rising, timeout rate spike
- [ ] Dashboard: circuit breaker states, bulkhead saturation, retry success/failure ratio

---

## Quick Sanity Check

- [ ] Circuit breaker on every remote call (service-to-service + external APIs)
- [ ] Retry only on idempotent operations
- [ ] Fallback for every critical path
- [ ] Timeouts on every external call, matched per dependency
- [ ] Bulkheads prevent cascading thread exhaustion
- [ ] No unbounded queues anywhere
- [ ] Chaos test: kill random service → system degrades, doesn't die
- [ ] All resilience states observable (metrics + alerts)

---

## Sources

- [[04 Circuit Breaker]] — circuit breaker patterns and implementation
- [[04 Bulkhead Pattern]] — isolation strategies
- [[04 Retry & Timeout]] — retry strategies and timeout tuning
- [[Microservice Launch]] — system-wide launch checklist
