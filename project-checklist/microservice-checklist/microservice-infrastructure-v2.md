# Microservice Infrastructure Launch Checklist (v2)

> Tick every box before service discovery, load balancing, and auth infrastructure hits production. Deep reference: [[microservice-infrastructure]] (292 lines). Assumes [[Microservice Launch]] checklist done.

---

## Service Discovery

- [ ] Services find each other without hardcoded URLs → [[06 Service Discovery]]
- [ ] New instances register on startup, deregister on graceful shutdown
- [ ] Dead instances evicted within 90s
- [ ] Registry is HA — ≥2 nodes → [[microservice-infrastructure]]
- [ ] Client-side discovery for internal (service → service), server-side for external (gateway → service)
- [ ] Registry secured — basic auth on management endpoints at minimum
- [ ] No business data in registry — it's ephemeral

---

## Load Balancing

- [ ] Edge LB is the ONLY internet entry point → [[03 Load Balancing & Proxies]]
- [ ] TLS terminated at edge with auto-renewed certs → [[03 Network & TLS]]
- [ ] Health checks active on all upstreams — unhealthy removed automatically → [[05 Health Checks]]
- [ ] Algorithm chosen with documented reason (round-robin default) → [[microservice-infrastructure]]
- [ ] L7 at edge (content-based routing), L4 internal (IP:port is enough)
- [ ] Sticky sessions avoided unless specifically justified
- [ ] Edge LB itself is not a SPOF (multi-instance or cloud-managed)

---

## API Gateway

- [ ] Gateway is single entry point — no direct service exposure → [[api-gateway-v2]]
- [ ] All routes configured and tested
- [ ] JWT validated at gateway — claims forwarded as headers or tokens stripped
- [ ] Rate limiting active per client/service
- [ ] CORS handled at gateway, not per service

---

## Authentication (System-Wide)

- [ ] Dedicated auth server (Keycloak/Auth0) — never embedded → [[03 Authentication]]
- [ ] Authorization Code + PKCE for user login flows → [[microservice-infrastructure]]
- [ ] Client Credentials for service-to-service calls
- [ ] Access tokens short-lived (≤15 min), RS256/ES256
- [ ] Refresh tokens long-lived, rotated on each use
- [ ] JWKS endpoint available, keys cached by all services → [[03 API Security]]
- [ ] Every service validates JWT independently (not just trusting gateway headers)

---

## Security (Infrastructure-Level)

- [ ] mTLS for service-to-service if zero-trust required → [[03 Network & TLS]]
- [ ] TLS 1.2 minimum, prefer 1.3 everywhere → [[03 Load Balancing & Proxies]]
- [ ] Certificates auto-renewed — no manual rotation, expiry alerts
- [ ] Gateway admin API on separate port, NOT internet-facing → [[api-gateway-v2]]
- [ ] Auth server database backed up regularly — it IS your user store → [[03 Backup & Recovery]]

---

## Observability (Infrastructure-Level)

- [ ] Trace ID propagated from gateway → all services → logs → [[05 Distributed Tracing]]
- [ ] Centralized logging (Loki, ELK, or equivalent) → [[05 Logging & Monitoring]]
- [ ] Infrastructure dashboard: per-service health, circuit states, error rates
- [ ] Alerts: any service down, any circuit open, inter-service latency spike, TLS expiry

---

## Deployment

- [ ] Infrastructure as Code — Docker Compose (dev), Terraform/Pulumi (prod) → [[microservice-infrastructure]]
- [ ] Service startup order handled (dep retries, not fragile sequencing)
- [ ] Rolling deployments with health checks — no downtime → [[07 Deployment Strategies]]
- [ ] Feature toggles for cross-service changes → [[06 Configuration Management]]
- [ ] Service mesh considered if mTLS + traffic splitting needed → [[07 Service Mesh]]

---

## Build vs Adopt

- [ ] Don't build your own service discovery — use Eureka/Consul/K8s DNS
- [ ] Don't build your own OAuth2 server — use Keycloak/Auth0/Okta
- [ ] Don't build your own edge LB — use NGINX/Traefik/HAProxy/cloud LB
- [ ] Don't build your own JWT validation — use framework libraries
- [ ] Do use established tooling for everything in this checklist

---

## Quick Sanity Check

- [ ] All services discoverable without hardcoded URLs
- [ ] All external traffic goes through gateway + edge LB — nothing exposed directly
- [ ] Auth enforced everywhere — no unauthenticated requests reach business logic
- [ ] TLS everywhere — edge → internal (HTTP or mTLS depending on trust model)
- [ ] All certificates auto-renewed
- [ ] All infrastructure config in git, deployable to fresh environment
- [ ] Every "build vs adopt" decision documented

---

## Sources

- Deep reference: [[microservice-infrastructure]] (original v1 with 5 parts + technology comparisons)
- `[[Microservice Launch]]` — service-level checklist
- `[[api-gateway-v2]]` — gateway-specific launch checklist
