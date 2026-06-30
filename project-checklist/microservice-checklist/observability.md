# Observability Checklist (System-Wide)

> Tick every box before your system is observable. If you can't see it, you can't fix it. Deep reference: [[05 Logging & Monitoring]], [[05 Distributed Tracing]], [[05 Health Checks]].

---

## The Three Pillars

- [ ] **Logs** — what happened. Structured, searchable, centralized
- [ ] **Metrics** — how much, how fast. Aggregated, dashboarded, alerted
- [ ] **Traces** — where and how long. End-to-end request journey across services
- [ ] All three pillars connected — trace ID in logs, metrics tagged with service name

---

## Logging

- [ ] **Structured logging — JSON in production** → [[05 Logging & Monitoring]]
- [ ] Every log line: timestamp (ISO 8601, UTC), level, service, trace_id, span_id, message, context
- [ ] Log levels: DEBUG (dev), INFO (prod default), WARN, ERROR
- [ ] No secrets in logs — filter/redact: password, token, authorization header, credit card
- [ ] Centralized: Loki, ELK, Datadog, Splunk. Never SSH into a server to read logs
- [ ] Retention: how long? Compliance requirements? Storage cost vs debugging value

---

## Metrics — The Golden Signals

- [ ] **Rate** — requests/sec per service, per endpoint → [[04 API Monitoring]]
- [ ] **Errors** — 5xx rate per service, per endpoint. 4xx rate monitored separately (client vs server)
- [ ] **Duration** — p50, p95, p99 latency per endpoint
- [ ] **Saturation** — thread pool, connection pool, queue depth, CPU, memory
- [ ] Metrics exported to Prometheus/Datadog/CloudWatch
- [ ] Custom business metrics: orders created/min, payment success rate, signup conversion

---

## Distributed Tracing

- [ ] **Trace ID propagated across all service boundaries** → [[05 Distributed Tracing]]
- [ ] Generated at gateway (or first service). Passed via `traceparent` (W3C TraceContext)
- [ ] Every service adds span: service name, operation, duration, status
- [ ] Trace context propagated: HTTP headers, message headers, gRPC metadata
- [ ] Sampling: 100% in dev, 1-10% in production (avoid overhead)
- [ ] Trace visualization: Jaeger, Tempo, Zipkin, Datadog APM
- [ ] Critical paths always traced (payment, auth) even at low sample rate

---

## Dashboards

- [ ] **Service dashboard** — per service: RED metrics, health, circuit state, thread pool → [[05 Logging & Monitoring]]
- [ ] Infrastructure dashboard — CPU, memory, disk, network per instance
- [ ] Business dashboard — orders/day, revenue, active users. Not just tech metrics
- [ ] Dashboards as code (Grafana JSON, Terraform) — versioned, reproducible
- [ ] Dashboard organized by audience: dev (per-service detail), ops (infra overview), business (KPIs)

---

## Alerting

- [ ] **Alerts on symptoms, not causes** → [[04 API Monitoring]]
- [ ] Symptom: "5xx rate > 1% for 5 minutes" (not "CPU > 80%" — that's a cause, not a symptom)
- [ ] Critical: service down, 5xx spike, circuit open, DB unreachable, TLS expiring
- [ ] Warning: p95 latency rising, disk > 80%, connection pool near max, retry rate increasing
- [ ] Alert fatigue prevention: debounce (must persist for N minutes), group related alerts
- [ ] On-call rotation defined — who gets paged at 3am?
- [ ] Runbooks for every alert — "if this fires, do these steps"

---

## Health Checks

- [ ] **Every service has `/health` endpoint** → [[05 Health Checks]]
- [ ] Liveness: "am I alive?" — lightweight, no dependency checks. K8s liveness probe
- [ ] Readiness: "am I ready to serve traffic?" — includes DB, broker, cache checks. K8s readiness probe
- [ ] Startup: "am I initialised?" — for slow-starting services. K8s startup probe
- [ ] Health endpoint secured — not publicly accessible. Internal network or basic auth
- [ ] Detailed health: `show-details: when-authorized` — not exposed to everyone

---

## Error Tracking

- [ ] **Exceptions captured with context** — stack trace + request data (sanitized) → [[04 API Monitoring]]
- [ ] Sentry, Datadog Error Tracking, or equivalent
- [ ] Errors grouped by fingerprint — same bug, different users = one issue
- [ ] Errors correlated with traces — click from error to the trace that produced it
- [ ] Source maps for frontend — minified stack traces resolved to original code

---

## SLI / SLO / SLA

- [ ] **SLI defined** (Service Level Indicator) — what you measure → [[04 API Monitoring]]
- [ ] Availability: `successful_requests / total_requests`
- [ ] Latency: `p95_latency < 200ms`
- [ ] Error rate: `5xx_responses / total_responses < 0.1%`
- [ ] **SLO defined** (Service Level Objective) — the target: "99.9% availability, p95 < 200ms"
- [ ] **Error budget** — how much downtime is acceptable before action: 0.1% = 43 min/month
- [ ] Burn rate alerts: fast burn (1h of budget in 5min) → page, slow burn → ticket

---

## Testing Observability

- [ ] Health endpoints return correct status (liveness ≠ readiness)
- [ ] Logs appear in centralized system within acceptable latency
- [ ] Trace from gateway → service A → service B → DB visible end-to-end
- [ ] Metrics dashboards populated with real data
- [ ] Alert fires → notification reaches on-call → acknowledged
- [ ] Test: kill a service → health check fails, readiness probe fails, alert fires

---

## Quick Sanity Check

- [ ] Logs: structured JSON, centralized, no secrets
- [ ] Metrics: RED per service, custom business metrics, dashboards as code
- [ ] Traces: end-to-end, W3C TraceContext, sampled appropriately
- [ ] Dashboards: per-service, per-infra, per-business. Version controlled
- [ ] Alerts: symptom-based, debounced, runbooked, on-call rotation defined
- [ ] Health checks: liveness, readiness, startup probes configured
- [ ] Error tracking: grouped, correlated with traces, source maps
- [ ] SLI/SLO/SLA: defined, measured, error budget tracked
- [ ] Observability tested: kill service → alert fires → trace shows failure point

---

## Sources

- [[05 Logging & Monitoring]] — structured logging and metrics
- [[05 Distributed Tracing]] — trace propagation and visualization
- [[05 Health Checks]] — liveness, readiness, startup probes
- [[04 API Monitoring]] — SLI/SLO/SLA, alerting
- [[Microservice Launch]] — system-wide launch checklist
