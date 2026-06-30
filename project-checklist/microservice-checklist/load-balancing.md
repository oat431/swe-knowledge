# Load Balancing Checklist

> Tick every box before your load balancer hits production. Traffic must be distributed, not dumped on one instance. Deep reference: [[microservice-infrastructure]] Part 2.

---

## Architecture Decision

- [ ] **L4 vs L7 chosen** — document reasoning → [[03 Load Balancing & Proxies]]
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

- [ ] **Active health checks configured** → [[05 Health Checks]]
- [ ] Interval: 5-30s. Shorter = faster detection, more load
- [ ] Timeout: shorter than interval. Failed checks → mark unhealthy
- [ ] Healthy threshold: N consecutive successes before re-admitting (prevents flapping)
- [ ] Unhealthy threshold: N consecutive failures before evicting
- [ ] Health endpoint on every service returns real status (not just 200 always) → [[05 Health Checks]]
- [ ] Passive checks: observe actual response codes. 5xx spike → mark unhealthy

---

## Failure Detection & Recovery

- [ ] **Outlier detection** — not just "is it up?" but "is it behaving?" → [[microservice-infrastructure]]
- [ ] If one instance has 10x latency of peers → eject it even if technically "healthy"
- [ ] Circuit breaking: N failures → stop routing to instance entirely → [[04 Circuit Breaker]]
- [ ] Half-open state: send probe request after cooldown → succeed → resume routing
- [ ] Unhealthy instances removed from pool automatically
- [ ] Recovered instances re-added automatically

---

## Timeouts

- [ ] **Connect timeout** — how long to wait for TCP handshake. 3s typical → [[04 Retry & Timeout]]
- [ ] **Read timeout** — how long to wait for response after connected. 30s typical
- [ ] **Idle timeout** — close idle connections. Free up resources
- [ ] Client timeout > LB timeout — otherwise client retries while LB is still waiting
- [ ] Timeouts tuned per upstream: auth service 200ms, report service 30s

---

## TLS & Security

- [ ] **TLS terminated at edge LB** → [[03 Network & TLS]]
- [ ] Valid certificates with auto-renew (Let's Encrypt / cert-manager)
- [ ] Minimum TLS 1.2, prefer 1.3
- [ ] HSTS header set at LB
- [ ] Certificates NOT self-signed in production
- [ ] Internal traffic: HTTP (trusted network) or mTLS (zero-trust)

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
- [ ] Request retry on connection failure — but only for idempotent methods (GET/PUT/DELETE)

---

## Deployment

- [ ] **Edge LB is NOT a single point of failure** → [[microservice-infrastructure]]
- [ ] ≥2 instances behind floating IP or DNS failover, or cloud-managed HA
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

- [ ] Round-robin distributes evenly — send 100 requests, each instance gets ~25 (4 instances)
- [ ] Unhealthy instance removed — traffic stops within configured threshold
- [ ] Recovered instance re-added — traffic resumes after healthy threshold
- [ ] Kill one LB instance — secondary takes over without dropped requests
- [ ] Graceful draining — active connections complete before shutdown
- [ ] Load test: throughput through LB vs direct to service. Overhead < 5ms p95

---

## Quick Sanity Check

- [ ] Edge LB is the ONLY thing exposed to the internet
- [ ] TLS terminated with valid, auto-renewed certificates
- [ ] Health checks removing unhealthy instances reliably
- [ ] Algorithm documented and tested
- [ ] Sticky sessions avoided or justified
- [ ] LB itself is HA (≥2 instances or cloud-managed)
- [ ] Connection pooling to upstreams enabled
- [ ] Timeouts configured at every layer (connect < read < client)
- [ ] Zero-downtime deploys tested

---

## Sources

- [[03 Load Balancing & Proxies]] — L4 vs L7, algorithms, NGINX/HAProxy config
- [[microservice-infrastructure]] — full infrastructure reference (Part 2)
- [[Microservice Launch]] — system-wide launch checklist
