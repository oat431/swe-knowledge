# API Gateway Launch Checklist (v2)

> Tick every box before an API gateway hits production. Deep reference: [[api-gateway]].
> Assumes you've done [[Microservice Launch]] and [[API Launch]] for downstream services.

---

## Architecture

- [ ] Single entry point — all external traffic through gateway, no direct service exposure → [[api-gateway]]
- [ ] Gateway handles cross-cutting only (routing, auth, rate limiting, logging) — no business logic
- [ ] High availability — ≥2 instances behind a load balancer
- [ ] Stateless instances — shared state (rate limits, sessions) in Redis

---

## Routing

- [ ] All routes defined and tested — path-based (`/api/users/**` → user-service) → [[api-gateway]]
- [ ] Route ordering explicit — specific routes before wildcards
- [ ] Service discovery integrated — picks up new instances, drops dead ones → [[06 Service Discovery]]
- [ ] Catch-all 404 handler returns standardized error (not raw gateway 404)
- [ ] Route versioning decided — `/v1/**`, `/v2/**` or header-based

---

## Authentication

- [ ] JWT validated at gateway — signature, issuer, audience, expiry → [[03 Authentication]]
- [ ] JWKS cached from auth server — no introspection call per request
- [ ] `Authorization` header stripped before forwarding (or passed if services need it)
- [ ] API keys hashed in DB, scoped, revocable → [[03 API Security]]
- [ ] Route-level roles — admin routes blocked at gateway, not in 15 services

---

## Rate Limiting & Resilience

- [ ] Rate limiting active — per-IP, per-token, per-endpoint → [[03 Authorization & Rate Limiting]]
- [ ] Redis-backed for multi-instance — not per-instance memory
- [ ] Rate limit headers on all responses (`X-RateLimit-*`, `Retry-After`)
- [ ] Circuit breaker per upstream — N failures → open → fast-fail → [[04 Circuit Breaker]]
- [ ] Retry with backoff on idempotent requests only (GET/PUT/DELETE)
- [ ] Timeout per route — different SLA per upstream
- [ ] Bulkhead — max concurrent requests per upstream

---

## CORS & Security Headers

- [ ] CORS at gateway only — specific origins, not `*` → [[03 API Security]]
- [ ] Security headers: HSTS, `X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`, `Permissions-Policy`
- [ ] Content-Security-Policy set — Report-Only first, enforce after no violations
- [ ] TLS terminated at gateway — minimum TLS 1.2, prefer 1.3 → [[03 Network & TLS]]
- [ ] Certificates auto-renew (Let's Encrypt / cert-manager) — expiry alerts
- [ ] Request body size limit (10MB default) — prevents OOM
- [ ] Gateway admin API on separate port, NOT internet-facing

---

## Observability

- [ ] Trace ID on every request — generated at gateway, propagated downstream → [[05 Distributed Tracing]]
- [ ] Structured logs — JSON, includes trace_id, method, path, status, latency, upstream
- [ ] RED metrics (Rate, Errors, Duration) per route and per upstream → [[04 API Monitoring]]
- [ ] Alerts: 5xx spike, p95 latency high, circuit open, TLS expiring, upstream down

---

## Caching

- [ ] Cache for GET requests with appropriate TTL → [[api-gateway]]
- [ ] Bypass cache for authenticated requests (unless shared cache by design)
- [ ] Cache invalidation — purge by URL or tag, CI/CD hook on deploy

---

## API Versioning & Docs

- [ ] Versions routed at gateway — services don't need version logic → [[api-gateway]]
- [ ] Deprecation headers on old versions (`Sunset`, `Deprecation`, `Link`)
- [ ] OpenAPI docs aggregated at single endpoint (dev portal or `/openapi.json`)

---

## Testing

- [ ] Every route resolves correctly — CI test suite
- [ ] Auth tested: valid → 200, expired → 401, wrong role → 403
- [ ] Rate limit tested: N requests → N+1 gets 429
- [ ] Circuit breaker tested: kill upstream → circuit opens → recovers
- [ ] Load tested — gateway overhead < 5ms p95

---

## Deployment

- [ ] Config in git (GitOps) — routes, policies, plugins versioned → [[api-gateway]]
- [ ] Zero-downtime config reload — no dropped connections on route changes
- [ ] Graceful shutdown — drain connections within timeout → [[07 Containers & Orchestration]]
- [ ] Gateway config deploys to fresh environment in minutes (disaster recovery)

---

## Quick Sanity Check

- [ ] All routes resolve and return correct upstream
- [ ] Auth enforced on all non-public routes
- [ ] Rate limiting active and tested
- [ ] TLS terminated with valid certificates
- [ ] Structured logs include trace_id on every line
- [ ] Circuit breakers tested (kill upstream → verify fast-fail)
- [ ] Gateway admin API NOT internet-facing
- [ ] Graceful shutdown tested (rolling restart, zero errors)
- [ ] Config in git, deployable to any environment

---

## Sources

- Deep reference: [[api-gateway]] (original v1 with 18 sections + tech comparisons)
- `[[Microservice Launch]]` — system-wide checklist
- `[[API Launch]]` — per-service checklist
