---
title: Resilience Checklist (System-Wide)
checklist_type: microservice-domain
version: 2.0
status: active
scope: failure isolation, deadlines, retries, degradation, and recovery testing
last_updated: 2026-08-06
---

# Resilience Checklist (System-Wide)

> Tick every box before your system survives failure. One dead service should not cascade-kill everything. Deep reference: [[041 Circuit Breaker]], [[043 Bulkhead Pattern]], [[042 Retry & Timeout]].

---

## Circuit Breakers

- [ ] **Failure-isolation policy per remote dependency** — circuit breaker, timeout-only, bulkhead, queue buffering, or not applicable; selection is justified → [[041 Circuit Breaker]]
- [ ] Per-destination config — payment gateway stricter than notification service
- [ ] States: closed (normal) → open (fail fast) → half-open (probe) → closed (recovered)
- [ ] Failure threshold: trip after N failures within window (e.g., 5/10 requests fail)
- [ ] Open duration: 30s default, longer for critical/money paths
- [ ] Half-open: permit N probe requests, any failure → re-open immediately
- [ ] Fail fast on open circuit — don't make callers wait for timeout
- [ ] Fallback response returned — cached stale data, degraded response, or domain error. Never null

---

## Bulkheads

- [ ] **Critical services isolated** — separate thread pools or instances → [[043 Bulkhead Pattern]]
- [ ] Payment service can't exhaust threads needed by auth service
- [ ] Thread pool isolation: max concurrent per downstream. Reject when full → circuit breaker picks it up
- [ ] Semaphore isolation: lighter than thread pools. Good for non-blocking calls
- [ ] Instance isolation: critical services on dedicated instances (most isolation, most cost)

---

## Retries

- [ ] **Retry only when the operation and failure window are safe** — use application-level idempotency for business side effects → [[042 Retry & Timeout]]
- [ ] Exponential backoff: 100ms → 200ms → 400ms → 800ms
- [ ] Jitter: randomize backoff to avoid thundering herd
- [ ] Max retries: 3. Max total time: shorter than client timeout
- [ ] Retry ownership is defined at one layer per dependency; retry amplification across gateway, service, SDK, and broker is prevented
- [ ] Retryable exceptions: connection refused, timeout. NOT: 400 Bad Request, 404 Not Found

---

## Timeouts

- [ ] **Deadline/timeout on every remote call** → [[042 Retry & Timeout]]
- [ ] Connect, pool-wait, read, total, and cancellation deadlines are defined from the caller's budget
- [ ] Per-route/dependency timeouts are measured and documented; examples are not universal defaults
- [ ] Inner dependency deadline < service deadline < caller deadline, with retry time included
- [ ] Timeout + circuit breaker paired — circuit opens before timeout exhausts threads

---

## Fallbacks

- [ ] **Every critical path has an explicit failure outcome** — stale read, partial response, queue for later, fail closed, fail open, or hard error; a fallback is not mandatory for every operation → [[041 Circuit Breaker]]
- [ ] Cache: return last known good response (stale-while-revalidate)
- [ ] Degraded: return partial data with `degraded: true` flag
- [ ] Default/partial responses never fabricate successful business state
- [ ] Fail closed versus fail open is documented per endpoint; authentication, authorization, payments, inventory, balances, and compliance decisions default to safe behavior
- [ ] Fallback logged and metered — know how often you're degraded

---

## Cascading Failure Prevention

- [ ] **Failure blast radius is bounded** — critical dependency failures have defined user-visible degradation and recovery behavior → [[043 Bulkhead Pattern]]
- [ ] Backpressure: slow consumers signal producers to slow down (reactive streams, queue limits)
- [ ] Load shedding: reject low-priority requests when overloaded (return 503 early)
- [ ] Graceful degradation: if user profile service is down, show homepage without avatar
- [ ] No unbounded queues — bounded queues + rejection policy
- [ ] Retry budgets, concurrency limits, queue limits, and priority classes are defined

---

## Testing Resilience

- [ ] **Failure tested** — kill or isolate a service, verify documented degradation, bounded blast radius, and recovery time
- [ ] Circuit breaker opens on repeated failure
- [ ] Fallback returned while circuit is open
- [ ] Circuit half-opens after cooldown, probe succeeds, closes
- [ ] Bulkhead contains failure — one service exhaust doesn't affect others
- [ ] Retry exhausts → circuit breaker opens (not infinite retry loop)
- [ ] Timeout/cancellation fires before the caller gives up and does not leave unsafe work untracked

---

## Observing Resilience

- [ ] **Metrics: circuit state per dependency** (closed/open/half-open) → [[04 API Monitoring]]
- [ ] Metrics: fallback invocation rate, retry count, timeout rate
- [ ] Alerts: any circuit open, fallback rate rising, timeout rate spike
- [ ] Dashboard: circuit breaker states, bulkhead saturation, retry success/failure ratio

---

## Quick Sanity Check

- [ ] Every remote dependency has a documented failure-isolation policy
- [ ] Retry only on idempotent operations
- [ ] Every critical path has a documented safe failure outcome
- [ ] Deadlines/timeouts exist for every remote call and are matched to the dependency
- [ ] Bulkheads prevent cascading thread exhaustion
- [ ] No unbounded queues anywhere
- [ ] Failure test: kill or isolate a selected dependency → system follows the documented degradation and recovery path
- [ ] All resilience states observable (metrics + alerts)

## Decision, Evidence & Exceptions

- Dependency failure matrix:
- Timeout/deadline and retry ownership:
- Circuit-breaker/bulkhead policy:
- Fallback or safe-failure policy:
- Load-shedding/backpressure policy:
- Recovery objectives (RTO/RPO where applicable):
- Evidence links (failure tests, metrics, recovery drill):
- N/A items and reason:
- Exceptions, approver, and review/expiry date:

---

## Sources

- [[041 Circuit Breaker]] — circuit breaker patterns and implementation
- [[043 Bulkhead Pattern]] — isolation strategies
- [[042 Retry & Timeout]] — retry strategies and timeout tuning
- [[Microservice Launch]] — system-wide launch checklist
- HTTP Semantics and idempotent methods — https://www.rfc-editor.org/info/rfc9110/
- AWS saga orchestration and idempotency — https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/saga-orchestration.html
