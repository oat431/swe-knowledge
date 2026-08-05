---
title: Microservice Infrastructure Checklist
checklist_type: microservice-system-baseline
version: 2.0
status: active
scope: system-level microservice architecture and production infrastructure
last_updated: 2026-08-06
---

# Microservice Infrastructure Checklist

> Practical, no-fluff checklist for production microservice infrastructure.
> Service discovery, load balancing, service mesh, API gateway, authentication, observability, and deployment.
> Framework-agnostic system baseline. Platform-specific controls belong in a deployment overlay or runbook.

---

## 1. Service Boundaries & Design

- [ ] **Domain-Driven Design** — Each service owns a bounded context. One business capability per service. If a service can't handle a request end-to-end without synchronous calls to another service, the boundary is wrong.
- [ ] **Data ownership boundary** — Each service owns its data store or explicitly assigned data boundary. Other services do not directly read/write its owned schema. Transitional shared storage requires an owner, access boundary, migration plan, and expiry.
- [ ] **Loose coupling, high cohesion** — Changing one service doesn't require changing others. Everything inside a service relates to the same domain. If services deploy in lockstep, you have a distributed monolith.
- [ ] **Right-sized services** — Not nano-services (five calls to do one thing) and not mini-monoliths. A service should be ownable by one team (2-pizza rule) and deployable independently.
- [ ] **API contracts first** — Define service interfaces (OpenAPI, protobuf, AsyncAPI) before implementation. Contracts are versioned. Breaking changes go through deprecation cycle.
- [ ] **Async over sync where justified** — Synchronous chains compound latency and failure propagation. Use asynchronous communication where the business interaction permits it; define a measured hop/latency budget rather than treating two hops as a universal limit.
- [ ] **Event-driven for cross-service workflows** — Service A publishes `order.created`. Service B consumes it. No direct coupling. Eventual consistency is the default model. Saga pattern for distributed transactions.

## 2. Service Discovery

- [ ] **No hardcoded service locations** — IPs change on redeploy. Use the selected discovery mechanism (DNS, registry, orchestrator service, or managed discovery) as the source of truth for what is running and where.
- [ ] **Registration on startup** — Every service announces itself: name, address, port, health endpoint. Automatic, not manual.
- [ ] **Deregistration on shutdown** — Graceful shutdown removes the instance. Crash → heartbeat timeout → registry evicts (within 60-90s).
- [ ] **Client-side vs server-side discovery** — Client-side (service queries registry, picks instance): no extra hop, common for internal traffic. Server-side (load balancer queries registry): simpler caller, one more hop, common for external. Hybrid is the standard: gateway for external, client-side for internal.
- [ ] **Discovery availability** — The selected discovery mechanism has an availability/recovery design appropriate to the service SLO. Record managed-service guarantees or the risk acceptance for a single-node registry; understand the registry's failure behavior.
- [ ] **Health checking** — Active (registry pings services) or heartbeat (services report to registry). Configure interval (10-30s), timeout, and eviction threshold.
- [ ] **Self-preservation** — When registry can't reach many services (network partition), should it evict all (risky) or preserve state (stale but safe)? Know your registry's default behavior.
- [ ] **Logical names, not IPs** — Services call `order-service`, not `10.0.1.5:8080`. DNS or registry resolves the logical name to current instances.
- [ ] **Registry secured** — Management endpoints behind auth. No public exposure. Read access may be open internally; write access restricted.

**Technology options:**

| Tool | Model | Availability/consistency note | Best for |
|------|-------|-----|----------|
| Kubernetes DNS + Service | Server-side, built-in | Platform-managed behavior; validate cluster recovery | Already on Kubernetes |
| HashiCorp Consul | Both | Quorum/consensus and partition behavior must be designed | Multi-platform, service mesh, KV store |
| Netflix Eureka | Client-side | Registry self-preservation and stale-instance behavior must be understood | Spring Boot shops, simpler setups |
| AWS Cloud Map | Server-side, managed | Provider-managed availability and eventual registration behavior | AWS-only deployments |

## 3. Load Balancing

### Edge (External → Internal)

- [ ] **Public entry-point policy** — For internet-facing systems, the edge LB/gateway is the only public entry point. Internal-only systems document their access boundary and do not expose service ports unnecessarily.
- [ ] **TLS termination at edge** — Auto-renewed certificates (Let's Encrypt, ACM, cert-manager). TLS 1.2 minimum, prefer 1.3. HSTS enabled.
- [ ] **Edge LB is not a SPOF** — Multi-instance or cloud-managed (ALB, Cloud Load Balancer). Auto-scaling under load.
- [ ] **Edge routing layer selected** — Use L7 when content-based routing, authentication, or WAF policy is required; use L4 where that is sufficient. The choice is documented.
- [ ] **Connection pooling and deadlines** — Reuse connections where supported. Configure connection, pool-wait, request, and dependency deadlines from the end-to-end latency budget.

### Internal (Service → Service)

- [ ] **Internal LB for service-to-service** — Client-side (Spring Cloud LoadBalancer, gRPC built-in, Envoy sidecar) or centralized (internal LB, K8s Service). Client-side eliminates an extra hop.
- [ ] **Internal routing layer selected** — L4 may be sufficient for simple internal traffic; L7 is justified where policy, protocol, routing, or observability requirements need it.
- [ ] **Algorithm chosen with reason** — Round-robin (default, equal instances), least-connections (variable request duration), weighted (canary/mixed-size instances), latency-based (geo-distributed).

### Health & Failure

- [ ] **Active health checks** — The traffic layer uses the service's readiness contract to remove ineligible instances; interval, timeout, and thresholds are selected for the workload.
- [ ] **Passive health checks / outlier detection** — Observe actual request outcomes. Too many 5xx from one instance → eject even if health endpoint passes.
- [ ] **Sticky sessions avoided** — Makes instances stateful. Breaks on scale-down/crash. Use shared state (Redis/DB) instead. Only if absolutely required (legacy WebSocket without session replication).
- [ ] **Request retry on connection failure** — Only when the operation and failure window are safe to retry; HTTP method idempotency does not replace application-level idempotency for business side effects.

**Technology options:**

| Tool | Strength | Best for |
|------|----------|----------|
| Traefik | Auto-discovery, Let's Encrypt built-in, K8s native | Container-native, auto-config |
| NGINX / OpenResty | Battle-tested, Lua scripting, huge community | Static configs, high customization |
| HAProxy | Maximum performance, rich ACLs | High-throughput, fine-grained routing |
| Caddy | Simplest config, auto-HTTPS | Internal tools, simple sites |
| Cloud ALB/NLB | Zero ops, auto-scaling | Cloud-native deployments |
| Envoy | Programmable, xDS API, sidecar | Service mesh data plane |

## 4. API Gateway

- [ ] **Single entry point for external traffic** — All client requests route through the gateway. Internal services are never exposed directly.
- [ ] **Authentication at the gateway** — Validate credentials at the edge for early rejection and coarse policy. Services do not re-implement login flows, but remain responsible for independently validating service/user identity and authorizing their own resources.
- [ ] **Rate limiting** — Per-client, per-endpoint. Token bucket or sliding window. Different limits for different tiers. 429 with `Retry-After` header.
- [ ] **CORS handled at gateway** — One place for CORS config, not duplicated across services.
- [ ] **Request routing** — Path-based (`/api/v1/users` → user-service), header-based, or host-based. Version routing (v1 → old service, v2 → new service).
- [ ] **Protocol translation** — REST (external) → gRPC (internal) if needed. Client sees REST, services communicate efficiently.
- [ ] **Request/response transformation** — Aggregate responses from multiple services (BFF pattern). Strip internal headers before responding to client.
- [ ] **Circuit breaker at gateway level** — Protect downstream from overload. Return fallback/cached response when circuit is open.
- [ ] **Observability** — Every request logged with trace ID, latency, status. Gateway metrics: request rate, error rate, latency percentiles per route.
- [ ] **Admin API on separate port** — Not internet-facing. Management endpoints (routes, plugins, health) isolated from traffic.

**Technology options:**

| Tool | Type | Best for |
|------|------|----------|
| Kong | Plugin-based, Lua + Go | Flexible, large plugin ecosystem |
| Spring Cloud Gateway | Java, reactive | Spring Boot microservices |
| NGINX + OpenResty | Lua scripting | Performance-critical, custom logic |
| Traefik | Go, auto-discovery | K8s-native, simple config |
| AWS API Gateway | Managed | Serverless, AWS ecosystem |
| Envoy + control plane | Programmable proxy | Service mesh ingress |

## 5. Authentication & Authorization

### Architecture Patterns

- [ ] **Pattern choice** — (1) Gateway validation plus verifiable internal identity propagation, (2) every service validates JWT independently via issuer metadata/JWKS, or (3) token introspection. Document the trust boundary, failure mode, and revocation trade-off.
- [ ] **Recommended: every service validates** — Gateway handles login flow AND validates. Downstream services ALSO validate independently. No single point of trust.

### OAuth2 / OpenID Connect

- [ ] **Dedicated auth server** — Keycloak (self-hosted), Auth0/Okta (SaaS), Cognito (AWS). Never build your own OAuth2 server.
- [ ] **Authorization Code + PKCE** — For all user-facing login flows (SPA, mobile, server-rendered). Never Implicit or Password grants.
- [ ] **Client Credentials** — For service-to-service calls. Machine-to-machine, no user involved. Short-lived tokens.
- [ ] **Access tokens short-lived** — ≤ 15 minutes. Signed with RS256 or ES256 (asymmetric — services validate with public key, never need the private key).
- [ ] **Refresh tokens long-lived + rotated** — 7-30 days. Each use issues new refresh token, invalidates old. Stolen token detected on next legitimate use.
- [ ] **JWKS discovery** — Resource servers use the issuer metadata at `/.well-known/openid-configuration` and its authoritative `jwks_uri`; services cache keys and retain old keys through the token validity window during rotation.
- [ ] **Token claims include essentials** — `iss` (issuer), `sub` (subject), `aud` (audience), `exp` (expiry), `iat` (issued at), roles/permissions.

### Authorization

- [ ] **Authorization model** — RBAC for most apps. ABAC/ReBAC for complex. Claims-based for microservices. Each service owns authorization decisions for its resources.
- [ ] **Enforce at service level** — Don't rely solely on gateway headers for authorization. Services validate token AND check permissions for the specific operation.
- [ ] **Multi-tenancy** — Tenant ID in token claims. Data isolation enforced at query layer. User from Tenant A cannot access Tenant B's data. Never trust client-provided tenant ID.

### Service-to-Service Auth

- [ ] **mTLS or JWT for internal calls** — Services authenticate to each other. Not "trust because same network." Service mesh handles mTLS transparently. OR services use Client Credentials tokens.
- [ ] **API keys for third-party integrations** — Hashed in DB. Prefixed for identification (`sk_live_...`). Scoped, revocable, rotatable.
- [ ] **No auth bypass paths** — Every endpoint (except health checks) requires authentication. If a service is accidentally exposed, auth still protects it.

**Technology options:**

| Tool | Type | Best for |
|------|------|----------|
| Keycloak | Self-hosted, open-source | Full control, any scale, customizable |
| Auth0 / Okta | SaaS | No ops overhead, enterprise SSO |
| AWS Cognito | Cloud-managed | AWS ecosystem, simple cases |
| ORY Hydra + Kratos | Self-hosted, headless | Flexible, OAuth2-focused |

## 6. Service Mesh (When You Need It)

> **Adoption decision:** Consider a mesh when its capabilities (mTLS, policy, traffic management, or consistent telemetry) solve a measured problem and the team can operate the added control plane. Service count alone is not the decision.

- [ ] **What it provides** — mTLS (encryption + identity between services), traffic management (retries, timeouts, circuit breakers), observability (metrics, traces without code), and traffic splitting (canary, A/B) — all without application code changes.
- [ ] **Data-plane model** — Document whether the selected mesh uses sidecars, eBPF, ambient, or another model; record traffic interception and failure behavior.
- [ ] **mTLS everywhere** — All service-to-service traffic encrypted and authenticated. Zero-trust by default. No plaintext internal traffic.
- [ ] **Traffic policies** — Retry budgets, timeouts, circuit breakers configured in mesh policy (not application code). Per-route, per-service tuning.
- [ ] **Traffic splitting** — Canary releases (5% → 25% → 100%), A/B testing, header-based routing to specific versions. Mesh handles this at the infrastructure layer.
- [ ] **Observability built-in** — Request metrics (rate, error, latency), distributed traces, and access logs generated by sidecars. No instrumentation code needed for golden signals.
- [ ] **Authorization policies** — Define which services can talk to which. `allow order-service → payment-service`. Deny by default. Least privilege at the network layer.
- [ ] **Resource overhead** — Measure proxy/control-plane CPU, memory, connection, and startup overhead for the selected version and workload; do not rely on universal sidecar estimates.
- [ ] **Operational complexity** — Mesh control plane is another critical component to maintain, upgrade, and debug. Team must understand mesh networking (not just application networking).

**Technology options:**

| Tool | Model | Best for |
|------|-------|----------|
| Istio | Sidecar (Envoy), feature-rich | Large orgs, advanced traffic management |
| Linkerd | Sidecar (Rust proxy), lightweight | Simplicity, lower resource overhead |
| Consul Connect | Sidecar (Envoy), multi-platform | HashiCorp ecosystem, hybrid cloud |
| Cilium | eBPF-based (sidecarless) | Performance-critical, kernel-level |


## 7. Inter-Service Communication

### Synchronous (Request/Reply)

- [ ] **REST for external + simple internal** — JSON over HTTP is broadly interoperable; select it when the contract and operational model fit.
- [ ] **gRPC for selected internal interactions** — Protobuf and HTTP/2 features may benefit latency, payload size, or streaming; validate with workload measurements and client/platform support.
- [ ] **Timeout on every outbound call** — Connection timeout + read timeout. Shorter than caller's own timeout. No call waits forever.
- [ ] **Failure-isolation policy per outbound dependency** — Circuit breaker, timeout-only, bulkhead, queue buffering, or not applicable; selection is justified. See [[041 Circuit Breaker]].
- [ ] **Retry only when safe** — Use operation-specific idempotency, exponential backoff, jitter, a single retry owner, and a bounded total deadline.
- [ ] **Bulkhead isolation** — Separate thread pools / connection pools per downstream. One slow service doesn't exhaust resources for all others.

### Asynchronous (Event-Driven)

- [ ] **Message broker** — Kafka (event streaming, replay, ordering), RabbitMQ (routing flexibility, mature), NATS (lightweight, cloud-native), SQS/SNS (managed, zero ops).
- [ ] **Event schema versioning** — Avro + Schema Registry, Protobuf, or JSON Schema. Forward and backward compatible. Don't break consumers when producers evolve.
- [ ] **Idempotent consumers** — Messages delivered at-least-once. Consumer handles duplicates gracefully. Dedup by message ID or idempotency key.
- [ ] **Ordering where needed** — Kafka: per-partition ordering (route by aggregate ID). RabbitMQ: per-queue with single consumer. If order doesn't matter: parallelize freely.
- [ ] **Dead letter queue** — Failed messages → DLQ after max retries. Inspect, fix, replay. Alert on DLQ growth. Never silently drop messages.
- [ ] **Saga pattern for distributed transactions** — Choreography (each service reacts to events) or orchestration (central coordinator). Compensating actions for rollback.
- [ ] **Outbox pattern** — Publish events reliably: write event to outbox table in same DB transaction as business data. Background process publishes to broker. No dual-write problem.

## 8. Observability

### The Three Pillars

- [ ] **Structured logging** — JSON format. Every log line: timestamp, level, service, trace_id, span_id, message. Centralized aggregation (Loki, ELK, CloudWatch Logs). Queryable by trace ID across all services.
- [ ] **Distributed tracing** — OpenTelemetry (the standard). Propagate `traceparent` header (W3C) across all service boundaries. Every service contributes spans. Backend: Jaeger, Tempo, Datadog, or Honeycomb. One request → one trace → see the entire path.
- [ ] **Metrics** — RED (Rate, Errors, Duration) for every service. USE (Utilization, Saturation, Errors) for infrastructure resources. Prometheus + Grafana or Datadog. Separate business metrics namespace from infra metrics.

### Implementation

- [ ] **OpenTelemetry everywhere** — Auto-instrumentation for HTTP, gRPC, DB, message broker. Manual spans for business logic. OTLP exporter to collector. One standard for all languages.
- [ ] **Health checks** — Define startup, liveness, and readiness semantics. Readiness controls traffic eligibility; liveness establishes process progress and should not restart a service solely because a dependency is temporarily unavailable.
- [ ] **SLOs defined and measured** — Define service-specific SLIs, SLOs, windows, exclusions, error budget, and burn-rate policy. Do not assume universal availability or latency targets.
- [ ] **Dashboards as code** — Grafana JSON provisioned from git. Not hand-tweaked in production. Dashboard changes go through PR review.
- [ ] **Alerting on symptoms, not causes** — Page on user impact/SLO burn and critical safety signals; resource, breaker, and DLQ signals are classified by impact and ownership.
- [ ] **Correlation across services** — Trace context in request/message logs and error reports; use bounded metric labels and avoid raw trace IDs as metric labels.
- [ ] **Infrastructure-level dashboard** — Per-service: health status, circuit breaker state, error rate, latency p50/p95/p99. Cross-service: dependency map, traffic flow, failure propagation.

## 9. Security (Infrastructure-Level)

- [ ] **Zero trust** — No service trusts another just because they share a network. Every request authenticated and authorized regardless of origin. Network is not a security boundary.
- [ ] **Service-to-service identity and encryption** — mTLS, workload identity, signed tokens, or another selected mechanism authenticates internal callers; encryption is used by default and plaintext exceptions are risk-accepted.
- [ ] **Network policy** — The selected platform enforces least-privilege communication where supported; Kubernetes NetworkPolicies are one implementation, not a universal requirement.
- [ ] **Secrets management** — Secrets are injected or fetched at runtime from the selected secret store, never baked into images or committed to git; rotation and access audit are defined.
- [ ] **TLS and certificate lifecycle** — Edge and internal transport policy, auto-renewal/rotation, expiry alerting, and failure behavior are documented.
- [ ] **Container/workload security** — Non-root, read-only filesystem, dropped capabilities, minimal image, resource isolation, and vulnerability scanning are applied where supported by the selected platform.
- [ ] **Supply chain security** — Image signing (cosign, Notary). Admission controller rejects unsigned images. SBOM generation. Dependency scanning.
- [ ] **Gateway admin isolated** — Admin API on separate port/network. Not internet-facing. Credentials rotated. Access logged.

## 10. Deployment & Orchestration

### Workload Packaging

- [ ] **Packaging model selected** — Container, VM, serverless, or another workload model; the package is reproducible and replaceable.
- [ ] **Container image controls where applicable** — Multi-stage build, minimal final image, non-root user, health contract, approved registry, immutable digest, and no production deployment from `:latest`.
- [ ] **Resource controls where applicable** — CPU, memory, storage, connection, and process limits are configured through the selected platform and tuned from measured usage.

### Selected Deployment Platform

- [ ] **Platform selected** — Docker Compose, Kubernetes, VM/systemd, managed container service, serverless, or another platform; platform-specific controls are documented in an overlay or deployment runbook.
- [ ] **Environment and access isolation** — Dev/staging/production are isolated according to risk; RBAC/access boundaries are defined for the selected platform.
- [ ] **Capacity and disruption controls** — Resource limits, autoscaling or manual capacity, disruption handling, and failure-domain placement are defined where applicable.
- [ ] **Deployment strategy** — Rolling, canary, blue-green, recreate, or job replacement selected with health gates, promotion criteria, abort criteria, and recovery procedure.
- [ ] **Startup and dependency behavior** — Services retry or fail predictably when dependencies are unavailable; no fragile startup ordering is required.

### CI/CD

- [ ] **Independent pipelines per service** — Service A deploys without Service B. Shared library changes trigger affected services only.
- [ ] **Pipeline stages** — Lint → test → build → push image → deploy staging → integration test → deploy production. Fail fast at each stage.
- [ ] **Controlled delivery path** — Desired state and/or deployment commands are versioned, reviewed, auditable, and reproducible. GitOps is used where it fits; unreviewed production click-ops or ad-hoc commands are not the normal path.
- [ ] **Feature flags** — Deploy code dark. Enable gradually. Kill broken features without redeploy. LaunchDarkly, Unleash, or DB-backed flags.
- [ ] **Contract tests in CI** — Consumer-driven contracts (Pact). Catches breaking API changes before deploy. Producer verifies consumer expectations.
- [ ] **Build once, deploy many** — Same image promoted through environments. Config differs via env vars, not rebuilds.

## 11. Resilience Patterns

- [ ] **Failure isolation per downstream** — Different policies and thresholds per dependency, based on measured behavior and business impact. See [[041 Circuit Breaker]].
- [ ] **Timeout/deadline at every boundary** — HTTP client, DB query, message consumer, and external API have bounded work and cancellation; inner deadlines fit within caller deadlines.
- [ ] **Retry with backoff + jitter** — Only for transient failures, with one retry owner, a retry budget, idempotency protection, and a bounded total deadline.
- [ ] **Bulkhead** — Separate thread pools / connection pools per downstream. One slow dependency doesn't exhaust resources for all outbound calls.
- [ ] **Rate limiting** — At gateway (per-client) and between services (per-caller). Protect downstream from burst. Graceful degradation over hard failure.
- [ ] **Graceful degradation** — If non-critical dependency fails: return partial data, cached stale data, or degraded response. Not 500 for the whole request.
- [ ] **Idempotency** — Each retryable mutation has operation-specific idempotency/deduplication semantics; not every write is inherently safe to retry.
- [ ] **Failure testing** — Selected dependencies and failure domains are isolated in controlled experiments; recovery, degradation, data integrity, and observability are verified.

## 12. Data Management

- [ ] **Database per service (enforced)** — No shared tables, no shared schemas. If two services need the same data: one owns it, the other gets a copy via events or API.
- [ ] **Event sourcing (where appropriate)** — Store events as the source of truth only when replay, retention, privacy, schema evolution, and operational cost are accepted. It does not create an audit trail for free.
- [ ] **CQRS (where appropriate)** — Separate write model (command to owning service) from read model (aggregated view for queries spanning services). Eventual consistency between write and read sides.
- [ ] **Data consistency strategy** — Eventual consistency is the default. Strong consistency only within a single service's database. Cross-service: saga for transactions, events for synchronization.
- [ ] **Schema evolution** — Database migrations backward-compatible. Add columns before code reads them. Remove columns after code stops writing. API schema evolution: additive changes only, deprecation for removal.
- [ ] **Multi-tenancy** — Tenant isolation at data layer. Separate schemas, row-level security, or separate databases per tenant. Query layer always filters by tenant. Never leak data across tenants.

---

## Quick Sanity Check Before Launch

### Service Discovery
- [ ] All services find each other without hardcoded URLs
- [ ] New instances register on startup, deregister on shutdown
- [ ] Dead instances evicted within 90s
- [ ] Discovery availability/recovery is appropriate to the service SLO or the single-node risk is explicitly accepted

### Load Balancing & Gateway
- [ ] Edge LB is the only internet entry point
- [ ] TLS terminated at edge with auto-renewed certs
- [ ] Unhealthy instances removed from pool automatically
- [ ] Gateway applies edge authentication/coarse policy and services enforce their own authorization
- [ ] Rate limiting active per client

### Authentication
- [ ] Every request has verified identity before reaching business logic
- [ ] Tokens short-lived (≤ 15 min) with refresh token rotation
- [ ] Services validate JWT independently (not just trusting gateway headers)
- [ ] Service-to-service calls authenticated (mTLS or Client Credentials)

### Observability
- [ ] Trace ID propagated across all service boundaries
- [ ] Structured JSON logs shipped to centralized system
- [ ] RED metrics per service, dashboards, and alerts configured
- [ ] SLOs defined and error budget tracked

### Security
- [ ] Zero trust: no service trusts another by network position
- [ ] mTLS or encrypted internal traffic
- [ ] Network policies enforce least-privilege communication
- [ ] Secrets from vault/secrets manager, never in code or git
- [ ] All certificates auto-renewed

### Deployment
- [ ] Services deploy independently (no lockstep)
- [ ] Selected deployment strategy has health gates, promotion/abort criteria, and a tested recovery path
- [ ] Controlled delivery path is versioned, reviewed, and auditable
- [ ] Feature flags used for risky exposure changes where they fit

### Resilience
- [ ] Every remote dependency has a documented failure-isolation policy
- [ ] Deadline/timeout exists on every outbound call
- [ ] Safe failure/degradation is defined for non-critical dependency failure
- [ ] Selected failure scenario tested and recovery behavior verified

---

## Decision, Evidence & Exceptions

- Service catalog and ownership map:
- Selected discovery mechanism and availability design:
- Public entry point and traffic topology:
- Identity/authentication trust boundary:
- Selected deployment platform and overlay:
- Data/messaging ownership and consistency model:
- SLO/SLI and resilience references:
- Evidence links (architecture review, contract tests, deployment, failover, recovery):
- N/A items and reason:
- Exceptions, approver, and review/expiry date:

---

## Build vs Adopt

| Concern | Default recommendation | Selection rule |
|---------|--------|-------------|
| Service registry | Adopt/managed preferred | Build only with a documented capability and operating-cost case |
| Edge load balancer | Adopt/managed preferred | Select according to traffic, HA, protocol, and operations |
| API Gateway | Adopt/managed preferred | Build only when the gateway capability is genuinely product-specific |
| OAuth2/OIDC server | Adopt | Use a mature provider; do not implement the protocol server from scratch |
| Service mesh | Adopt selectively | Use only when measured policy, identity, or traffic needs justify the control plane |
| JWT validation | Use a maintained library | Configure strict issuer, audience, algorithm, and key policies |
| Circuit breaker | Use a maintained library/platform | Select per dependency; do not apply mechanically to every call |
| Observability pipeline | Adopt components | Operate a standard pipeline with documented ownership and retention |

---

## Related Checklists

- [[checklist/api-checklist/api]] — General API design, resilience patterns, testing
- [[checklist/microservice-checklist/spring-boot/spring-boot-api-gateway]] — Spring Cloud Gateway implementation
- [[checklist/microservice-checklist/spring-boot/spring-boot-eureka]] — Service discovery with Eureka
- [[checklist/microservice-checklist/spring-boot/spring-boot-loadbalance]] — Client-side LB with Spring Cloud
- [[checklist/microservice-checklist/spring-boot/spring-boot-oauth]] — OAuth2 resource server + authorization server
- [[checklist/batch-checklist/batch]] — Batch processing patterns
