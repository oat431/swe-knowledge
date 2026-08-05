# Service Discovery Checklist

> Tick every box before service discovery hits production. Your services need to find each other without hardcoded IPs. Deep reference: [[microservice-infrastructure]] Part 1.

---

## Architecture Decision

- [ ] **Client-side vs server-side discovery chosen** — document reasoning → [[06 Service Discovery]]
- [ ] Client-side: service queries registry directly, owns LB decision (Eureka, Consul client)
- [ ] Server-side: service sends to LB, LB queries registry (AWS ALB, Traefik, Envoy)
- [ ] Hybrid: external via server-side (gateway), internal via client-side — most common

---

## Registry Choice

- [ ] **AP vs CP trade-off decided** → [[microservice-infrastructure]]
- [ ] AP (Eureka): always available, slightly stale data OK. Better for runtime routing
- [ ] CP (Consul, Etcd, Zookeeper): strong consistency, may be unavailable during leader election. Better for config/locks
- [ ] Self-hosted (Eureka/Consul) vs cloud-managed (AWS Cloud Map, GCP Service Directory)
- [ ] Registry technology documented with reasoning

---

## Registration & Deregistration

- [ ] Services register on startup — `@EnableDiscoveryClient` or equivalent
- [ ] Services deregister on graceful shutdown — shutdown hook or `@PreDestroy`
- [ ] Registration includes: service name, host/IP, port, health check URL, metadata (version, region, zone)
- [ ] Services re-register on restart — registry data is ephemeral
- [ ] No manual registration — fully automated

---

## Health Checking

- [ ] Health check endpoint on every service (`/health`, `/actuator/health`) → [[05 Health Checks]]
- [ ] Heartbeat interval: 30s (caller reports "I'm healthy") — lighter on registry
- [ ] Or active health check: registry pings `/health` every 15-30s — faster to detect failure
- [ ] Know your registry's failure detection: heartbeats vs active checks
- [ ] Dead instances evicted within 60-90s
- [ ] Health check includes real dependency checks — DB, message broker, disk space
- [ ] `/health` is NOT rate-limited at gateway

---

## High Availability

- [ ] Registry is HA — ≥2 nodes (production minimum is 3) → [[microservice-infrastructure]]
- [ ] Know your registry's replication model: Eureka = peer-to-peer, Consul/Etcd = Raft
- [ ] Nodes spread across availability zones / hosts
- [ ] Self-preservation mode understood (Eureka: preserves registry on network partition)
- [ ] Test: kill one registry node. Services still discover each other.

---

## Discovery in Practice

- [ ] Services call each other by logical name, never IP — `http://user-service/api/users`
- [ ] Client-side LB integrated: Spring Cloud LoadBalancer, gRPC client LB, or custom
- [ ] `@LoadBalanced RestTemplate` / `WebClient` or framework equivalent
- [ ] Cache service list locally — refresh on TTL or change event. Don't query registry per request
- [ ] Timeout on discovery calls — 5s. Fail fast if registry unreachable. Use cached list

---

## Security

- [ ] Registry management endpoints secured — basic auth minimum → [[microservice-infrastructure]]
- [ ] Registry NOT exposed to internet — internal network only
- [ ] No business data stored in registry — service names + IPs + health only
- [ ] TLS for registry communication if across untrusted networks

---

## Testing

- [ ] New instance registers and appears in registry within 30s
- [ ] Instance deregisters on graceful shutdown — removed within 60s
- [ ] Instance dies (kill -9) → evicted within 90s
- [ ] Service A resolves service B by logical name → returns healthy instances only
- [ ] Kill one registry node → discovery still works (HA verified)
- [ ] Unhealthy instance removed from pool → traffic stops routing to it
- [ ] Recovered instance re-added to pool → traffic resumes

---

## Quick Sanity Check

- [ ] No hardcoded IPs or ports in any service config
- [ ] All services register automatically on startup
- [ ] All services deregister on shutdown
- [ ] Registry itself is HA (≥2 nodes)
- [ ] Dead instances evicted within 90s
- [ ] Health checks include real dependency status
- [ ] Registry dashboard/API accessible for debugging
- [ ] Registry secured — not publicly accessible
- [ ] Discovery tested under failure conditions (kill instance, kill registry node)

---

## Sources

- [[06 Service Discovery]] — service discovery patterns and implementation
- [[microservice-infrastructure]] — full infrastructure reference (Part 1)
- [[Microservice Launch]] — system-wide launch checklist
