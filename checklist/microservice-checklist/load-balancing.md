---
title: Load Balancing Checklist
checklist_type: microservice-domain
version: 2.0
status: active
scope: edge and internal traffic distribution
last_updated: 2026-08-06
---

# Load Balancing Checklist

> Tick every box before your load balancer hits production. Traffic must be distributed, not dumped on one instance. Deep reference: [[microservice-infrastructure]] Part 2.

---

## Architecture Decision

- [ ] **L4 vs L7 chosen** — document reasoning; do not assume edge is always L7 or internal traffic is always L4 → [[03 Load Balancing & Proxies]]
- [ ] L4 (transport): route by IP + port. Faster, simpler. Internal service-to-service
- [ ] L7 (application): route by URL, headers, cookies. Can do auth, rate limit, transform. Edge/external
- [ ] Edge: L7. Internal: L4 is often enough

---

## Algorithm Choice

- [ ] **Algorithm chosen with documented reasoning** → [[microservice-infrastructure]]
- [ ] Round-robin: default, fair, all instances receive equal traffic. Best when instances are equal
- [ ] Least connections: send to instance with fewest active. Good for variable-latency workloads
- [ ] Weighted: assign different capacities (4 CPU vs 2 CPU). Canary deployments
- [ ] IP hash: sticky sessions without cookies. Same client → same instance
- [ ] Latency-based: route to fastest-responding. Geo-distributed or heterogeneous

---

## Health Checks

- [ ] **Active health checks configured** → [[053 Health Checks]]
- [ ] Interval: 5-30s. Shorter = faster detection, more load
- [ ] Timeout: shorter than interval. Failed checks → mark unhealthy
- [ ] Healthy threshold: N consecutive successes before re-admitting (prevents flapping)
- [ ] Unhealthy threshold: N consecutive failures before evicting
- [ ] **Health endpoint on every service returns meaningful status (not just 200 always) → [[053 Health Checks]]
- [ ] Passive checks: observe actual response codes. 5xx spike → mark unhealthy

---

## Failure Detection & Recovery

- [ ] **Outlier detection** — separate behavioral ejection from basic health readiness; define comparison window and false-positive protection → [[microservice-infrastructure]]
- [ ] If one instance has 10x latency of peers → eject it even if technically "healthy"
- [ ] Circuit breaking policy is explicit and distinct from health checks/outlier ejection; stop routing or fail fast only when the selected mechanism requires it → [[041 Circuit Breaker]]
- [ ] Half-open state: send probe request after cooldown → succeed → resume routing
- [ ] Unhealthy instances removed from pool automatically
- [ ] Recovered instances re-added automatically

---

## Timeouts

- [ ] **Connect timeout** — how long to wait for TCP handshake; select from the service deadline budget → [[042 Retry & Timeout]]
- [ ] **Read/total timeout** — how long to wait after connecting; select from the service deadline budget rather than a universal 30s default
- [ ] **Idle timeout** — close idle connections. Free up resources
- [ ] **Deadline hierarchy** — client deadline > LB total deadline > service deadline > dependency deadline; connection-pool wait, retries, and queue time are included
- [ ] Timeouts tuned per upstream and recorded with the SLO/deadline rationale

---

## TLS & Security

- [ ] **TLS terminated at edge LB** → [[03 Network & TLS]]
- [ ] Valid certificates with auto-renew (Let's Encrypt / cert-manager)
- [ ] Minimum TLS 1.2, prefer 1.3
- [ ] HSTS header set at LB
- [ ] Certificates NOT self-signed in production
- [ ] Internal traffic protection selected — encrypted transport by default; any plaintext trusted-network exception has documented isolation, threat model, and approval

---

## Session Handling

- [ ] **Sticky sessions avoided unless specifically justified** → [[microservice-infrastructure]]
- [ ] If unavoidable: cookie-based (L7) or IP hash (L4). Document why
- [ ] Sticky sessions make instances stateful — plan for instance death
- [ ] Session replication (Redis) over sticky sessions when possible

---

## Connection Management

- [ ] **Connection pooling to upstreams** — HTTP keep-alive → [[03 Load Balancing & Proxies]]
- [ ] Pool size tuned: max idle connections, max connections per host
- [ ] Don't open new connection per request
- [ ] **Request retry on connection failure** — only when the operation and failure window are safe to retry; HTTP idempotency is not a substitute for application-level idempotency keys

---

## Deployment

- [ ] **Edge LB is NOT a single point of failure** → [[microservice-infrastructure]]
- [ ] HA/failure-domain design selected — multiple instances, floating IP/DNS failover, managed HA, or an explicitly accepted single-instance risk
- [ ] Graceful draining: stop accepting new connections → drain existing → shutdown
- [ ] Zero-downtime config reload — no dropped connections
- [ ] Config in git — routes, upstreams, health check rules versioned

---

## Observability

- [ ] **Metrics exported: requests/sec, active connections, error rate, latency per upstream** → [[04 API Monitoring]]
- [ ] LB access logs: method, path, status, upstream, latency, client IP
- [ ] Alerts: upstream unhealthy, error rate spike, LB instance down, connection pool exhausted
- [ ] Dashboard: per-upstream health, traffic distribution, latency percentiles

---

## Testing

- [ ] Distribution behavior tested using realistic concurrency, connection reuse, request duration, and the selected algorithm; define an acceptable tolerance
- [ ] Unhealthy instance removed — traffic stops within configured threshold
- [ ] Recovered instance re-added — traffic resumes after healthy threshold
- [ ] Kill one LB instance — secondary takes over without dropped requests
- [ ] Graceful draining — active connections complete before shutdown
- [ ] Load test: throughput through LB vs direct to service; overhead is within the documented latency budget

---

## Decision, Evidence & Exceptions

- Selected L4/L7 topology and algorithm:
- Health/outlier/circuit-breaker model:
- Timeout and deadline hierarchy:
- TLS and internal-traffic policy:
- HA/failure-domain decision:
- Evidence links (failover, draining, health, load, certificate tests):
- N/A items and reason:
- Exceptions, approver, and review/expiry date:

## Quick Sanity Check

- [ ] For internet-facing systems, edge LB is the only public entry point
- [ ] TLS terminated with valid, auto-renewed certificates
- [ ] Health readiness removes unhealthy instances reliably; behavioral ejection and circuit breaking are tested separately
- [ ] Algorithm documented and tested
- [ ] Sticky sessions avoided or justified
- [ ] LB itself is HA (≥2 instances or cloud-managed)
- [ ] Connection pooling to upstreams enabled
- [ ] Timeouts and deadlines are configured at every layer with a documented hierarchy and retry budget
- [ ] Zero-downtime deploys tested

---

## Sources

- [[03 Load Balancing & Proxies]] — L4 vs L7, algorithms, NGINX/HAProxy config
- [[microservice-infrastructure]] — full infrastructure reference (Part 2)
- [[Microservice Launch]] — system-wide launch checklist
