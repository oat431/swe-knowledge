---
title: Observability Checklist (System-Wide)
checklist_type: microservice-domain
version: 2.0
status: active
scope: logs, metrics, traces, health, SLOs, and alerting
last_updated: 2026-08-06
---

# Observability Checklist (System-Wide)

> Tick every box before your system is observable. If you can't see it, you can't fix it. Deep reference: [[051 Logging & Monitoring]], [[052 Distributed Tracing]], [[053 Health Checks]].

---

## The Three Pillars

- [ ] **Logs** — what happened. Structured, searchable, centralized
- [ ] **Metrics** — how much, how fast. Aggregated, dashboarded, alerted
- [ ] **Traces** — where and how long. End-to-end request journey across services
- [ ] All three pillars connected — trace ID in logs, metrics tagged with service name

---

## Logging

- [ ] **Structured logging — JSON in production** → [[051 Logging & Monitoring]]
- [ ] Every request- or message-scoped log includes timestamp (ISO 8601, UTC), level, service, correlation context when available, message, and sanitized context. Startup and background work use an operation/execution ID where a trace does not exist.
- [ ] Log levels: DEBUG (dev), INFO (prod default), WARN, ERROR
- [ ] No secrets in logs — filter/redact: password, token, authorization header, credit card
- [ ] Centralized: Loki, ELK, Datadog, Splunk. Centralized logs are the normal operational path; break-glass host access is restricted, audited, and documented.
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

- [ ] **Trace context propagated across all service boundaries** → [[052 Distributed Tracing]]
- [ ] Generated at gateway (or first service). Passed via `traceparent` (W3C TraceContext)
- [ ] Every service adds span: service name, operation, duration, status
- [ ] Trace context propagated: HTTP headers, message headers, gRPC metadata
- [ ] Sampling policy is defined by environment, cost, and risk; critical paths remain observable even when ordinary traces are sampled
- [ ] Trace visualization: Jaeger, Tempo, Zipkin, Datadog APM
- [ ] Critical paths always traced (payment, auth) even at low sample rate

---

## Dashboards

- [ ] **Service dashboard** — per service: RED metrics, health, circuit state, thread pool → [[051 Logging & Monitoring]]
- [ ] Infrastructure dashboard — CPU, memory, disk, network per instance
- [ ] Business dashboard — orders/day, revenue, active users. Not just tech metrics
- [ ] Dashboards as code (Grafana JSON, Terraform) — versioned, reproducible
- [ ] Dashboard organized by audience: dev (per-service detail), ops (infra overview), business (KPIs)

---

## Alerting

- [ ] **Alerts on symptoms, not causes** → [[04 API Monitoring]]
- [ ] Symptom-based alert examples are tied to the service SLO and user impact; resource alerts are supporting signals, not automatic pages by themselves
- [ ] Critical: service down, 5xx spike, circuit open, DB unreachable, TLS expiring
- [ ] Warning: p95 latency rising, disk > 80%, connection pool near max, retry rate increasing
- [ ] Alert fatigue prevention: debounce (must persist for N minutes), group related alerts
- [ ] On-call rotation defined — who gets paged at 3am?
- [ ] Runbooks for every alert — "if this fires, do these steps"

---

## Health Checks

- [ ] **Every service has a platform-appropriate health contract** → [[053 Health Checks]]
- [ ] Liveness: "am I alive and making progress?" — lightweight; dependency failure does not automatically create restart storms
- [ ] Readiness: "am I ready to serve traffic?" — may include required DB, broker, or cache checks and controls traffic eligibility
- [ ] Startup: "am I initialized?" — protects slow-starting services where the platform supports it
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

- [ ] **SLI defined** (Service Level Indicator) — what you measure, including inclusion/exclusion rules → [[04 API Monitoring]]
- [ ] Availability: `successful_requests / total_requests`
- [ ] Latency: `p95_latency < 200ms`
- [ ] Error rate: `5xx_responses / total_responses < 0.1%`
- [ ] **SLO defined** (Service Level Objective) — service-specific target, window, user-impact model, and ownership; do not assume universal availability or latency numbers
- [ ] **Error budget** — how much downtime is acceptable before action: 0.1% = 43 min/month
- [ ] Burn-rate alert windows and page/ticket thresholds are defined from the service error budget

---

## Testing Observability

- [ ] Health endpoints return correct status (startup ≠ liveness ≠ readiness)
- [ ] Logs appear in centralized system within acceptable latency
- [ ] Trace from gateway → service A → service B → DB visible end-to-end
- [ ] Metrics dashboards populated with real data
- [ ] Alert fires → notification reaches on-call → acknowledged
- [ ] Test: kill or isolate a service → readiness changes, traffic is removed as intended, the correct alert fires, and liveness does not cause an avoidable restart loop

---

## Metrics Safety & Telemetry Operations

- [ ] Metric labels use bounded dimensions; no raw user ID, trace ID, request ID, email, tenant ID, or unbounded URL is used as a metric label
- [ ] Route templates are used instead of raw request paths
- [ ] Cardinality and telemetry-cost budgets are defined and monitored
- [ ] Logs, traces, and dashboards have access control, retention, and deletion policies
- [ ] Telemetry collectors/backends are monitored for dropped data, queue growth, and storage failure
- [ ] Deployment version markers allow release-to-telemetry correlation

## Decision, Evidence & Exceptions

- SLI/SLO/SLA references:
- Alert severity, ownership, and escalation:
- Sampling and cardinality policy:
- Retention, privacy, and access policy:
- Evidence links (dashboard, alert test, trace test, log-redaction test):
- N/A items and reason:
- Exceptions, approver, and review/expiry date:

## Quick Sanity Check

- [ ] Logs: structured JSON, centralized, no secrets
- [ ] Metrics: RED per service, custom business metrics, dashboards as code
- [ ] Traces: end-to-end, W3C TraceContext, sampled according to a documented policy
- [ ] Dashboards: per-service, per-infra, per-business. Version controlled
- [ ] Alerts: symptom-based, debounced, runbooked, on-call rotation defined
- [ ] Health checks: liveness, readiness, startup probes configured
- [ ] Error tracking: grouped, correlated with traces, source maps
- [ ] SLI/SLO/SLA: defined, measured, error budget tracked
- [ ] Observability tested: kill service → alert fires → trace shows failure point

---

## Sources

- [[051 Logging & Monitoring]] — structured logging and metrics
- [[052 Distributed Tracing]] — trace propagation and visualization
- [[053 Health Checks]] — liveness, readiness, startup probes
- [[04 API Monitoring]] — SLI/SLO/SLA, alerting
- [[Microservice Launch]] — system-wide launch checklist
- W3C Trace Context — https://www.w3.org/TR/trace-context/
- OpenTelemetry context propagation — https://opentelemetry.io/docs/concepts/context-propagation/
- Prometheus instrumentation and cardinality guidance — https://prometheus.io/docs/practices/instrumentation/
- Kubernetes probe semantics — https://kubernetes.io/docs/concepts/workloads/pods/probes/
