# Microservice Launch Checklist

> Tick every box before a multi-service system hits production. Assumes you've already done the [[API Launch]] checklist for each individual service.

---

## Service Discovery

- [ ] All services find each other without hardcoded URLs → [[../../software-engineering-note/02_Software_Architecture/Microservice/06 Infrastructure/061 Service Discovery|Service Discovery]]
- [ ] New instances register on startup, deregister on graceful shutdown
- [ ] Dead instances evicted from registry within 90s
- [ ] Registry itself is highly available (≥2 nodes for production)
- [ ] Registry secured (at minimum, basic auth on management endpoints)
- [ ] No business data stored in the registry

---

## Load Balancing

- [ ] Edge LB is the ONLY entry point from the internet → [[../../software-engineering-note/02_Software_Architecture/Microservice/07 Deployment/071 Containers & Orchestration|Containers & Orchestration]]
- [ ] TLS terminated at the edge with auto-renewed certificates → [[03 Network & TLS]]
- [ ] Health checks active on all upstream instances → [[../../software-engineering-note/02_Software_Architecture/Microservice/05 Observability/053 Health Checks|Health Checks]]
- [ ] Unhealthy instances removed from pool automatically
- [ ] Algorithm chosen with documented reasoning (round-robin default) → [[032 Load Balancing & Proxies]]
- [ ] Sticky sessions avoided unless specifically justified
- [ ] Edge LB itself is not a SPOF

---

## API Gateway

- [ ] Gateway is the single entry point: no direct service exposure → [[../../software-engineering-note/02_Software_Architecture/Microservice/02 Integration/021 API Gateway|API Gateway]]
- [ ] Routes configured for all services (YAML or programmatic)
- [ ] Rate limiting per client/service at the gateway → [[03 API Security]]
- [ ] Request/response logging at gateway (audit trail)
- [ ] CORS handled at gateway, not in individual services
- [ ] Gateway validates JWT before forwarding to services → [[03 Authentication]]

---

## Authentication (System-Wide)

- [ ] Dedicated auth server (Keycloak/Auth0/Okta): never embedded in an app → [[03 Authentication]]
- [ ] Authorization Code + PKCE for user-facing login flows
- [ ] Client Credentials for service-to-service calls
- [ ] Access tokens: short-lived (≤15 min), signed (RS256/ES256)
- [ ] Refresh tokens: long-lived, rotated on each use
- [ ] JWKS endpoint available, keys cached by resource servers
- [ ] Every service validates JWT independently (not just gateway) → [[03 API Security]]

---

## Resilience (System-Wide)

- [ ] Circuit breaker on every service-to-service call → [[../../software-engineering-note/02_Software_Architecture/Microservice/04 Resilience/041 Circuit Breaker|Circuit Breaker]]
- [ ] Bulkheads: isolate critical services (separate thread pools/instances) → [[../../software-engineering-note/02_Software_Architecture/Microservice/04 Resilience/043 Bulkhead Pattern|Bulkhead Pattern]]
- [ ] Retry + timeout on all inter-service calls → [[../../software-engineering-note/02_Software_Architecture/Microservice/04 Resilience/042 Retry & Timeout|Retry & Timeout]]
- [ ] Fallback responses for degraded dependencies
- [ ] No cascading failures possible (one service down ≠ system down)

---

## Data & Messaging

- [ ] Each service owns its database: no shared DB access → [[../../software-engineering-note/02_Software_Architecture/Microservice/03 Data/031 Database per Service|Database per Service]]
- [ ] Async communication via events where synchronous isn't needed → [[../../software-engineering-note/02_Software_Architecture/Microservice/02 Integration/023 Event-Driven Architecture|Event-Driven Architecture]]
- [ ] Dead letter queue for failed messages → [[../../software-engineering-note/02_Software_Architecture/Microservice/02 Integration/024 Messaging Patterns|Messaging Patterns]]
- [ ] Saga pattern for distributed transactions (not 2PC) → [[../../software-engineering-note/02_Software_Architecture/Microservice/03 Data/033 Saga Pattern|Saga Pattern]]
- [ ] Idempotent consumers (duplicate messages don't cause duplicate effects)

---

## Observability (System-Wide)

- [ ] Distributed tracing across all services → [[../../software-engineering-note/02_Software_Architecture/Microservice/05 Observability/052 Distributed Tracing|Distributed Tracing]]
- [ ] Centralized logging (ELK stack, Grafana Loki, or equivalent) → [[../../software-engineering-note/02_Software_Architecture/Microservice/05 Observability/051 Logging & Monitoring|Logging & Monitoring]]
- [ ] Service dashboard: per-service health, circuit states, error rates
- [ ] Alert on: any circuit open, any service down, inter-service latency spike

---

## Deployment

- [ ] Infrastructure as Code (Docker Compose for dev, Terraform/Pulumi for prod)
- [ ] Service startup order handled (dependencies start first, or services retry)
- [ ] Rolling deployments with health checks: no downtime
- [ ] Feature toggles for cross-service changes → [[../../software-engineering-note/02_Software_Architecture/Microservice/06 Infrastructure/063 Configuration Management|Configuration Management]]
- [ ] Service mesh considered (mTLS, traffic splitting) → [[../../software-engineering-note/02_Software_Architecture/Microservice/07 Deployment/073 Service Mesh|Service Mesh]]

---

## Sources

- Distilled from: [[Microservice Overview]], [[Cybersecurity Overview]], [[API Overview]].
- Original deep reference: `../microservice-checklist/microservice-infrastructure.md`
