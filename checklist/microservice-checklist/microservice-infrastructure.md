# Microservice Infrastructure Checklist

> Practical, no-fluff checklist for production microservice infrastructure.
> Service discovery, load balancing, service mesh, API gateway, authentication, observability, and deployment.
> Framework-agnostic. Last updated: 2026-07-17

---

## 1. Service Boundaries & Design

- [ ] **Domain-Driven Design** — Each service owns a bounded context. One business capability per service. If a service can't handle a request end-to-end without synchronous calls to another service, the boundary is wrong.
- [ ] **Database per service** — No shared databases. Each service owns its data store. Schema changes in Service A never break Service B. Cross-service data: events, API calls, or CQRS — not direct DB access.
- [ ] **Loose coupling, high cohesion** — Changing one service doesn't require changing others. Everything inside a service relates to the same domain. If services deploy in lockstep, you have a distributed monolith.
- [ ] **Right-sized services** — Not nano-services (five calls to do one thing) and not mini-monoliths. A service should be ownable by one team (2-pizza rule) and deployable independently.
- [ ] **API contracts first** — Define service interfaces (OpenAPI, protobuf, AsyncAPI) before implementation. Contracts are versioned. Breaking changes go through deprecation cycle.
- [ ] **Async over sync where possible** — Synchronous chains (A→B→C→D) compound latency and create cascading failures. Use async messaging for non-critical paths. Keep sync chains ≤ 2 hops.
- [ ] **Event-driven for cross-service workflows** — Service A publishes `order.created`. Service B consumes it. No direct coupling. Eventual consistency is the default model. Saga pattern for distributed transactions.

## 2. Service Discovery

- [ ] **No hardcoded service locations** — IPs change on redeploy. Ports are dynamic. A service registry is the single source of truth for "what's running and where."
- [ ] **Registration on startup** — Every service announces itself: name, address, port, health endpoint. Automatic, not manual.
- [ ] **Deregistration on shutdown** — Graceful shutdown removes the instance. Crash → heartbeat timeout → registry evicts (within 60-90s).
- [ ] **Client-side vs server-side discovery** — Client-side (service queries registry, picks instance): no extra hop, common for internal traffic. Server-side (load balancer queries registry): simpler caller, one more hop, common for external. Hybrid is the standard: gateway for external, client-side for internal.
- [ ] **Registry is HA** — Single registry = SPOF. Production: ≥ 2 nodes. Know your registry's HA model (Eureka: peer replication, Consul: Raft consensus, K8s: etcd-backed DNS).
- [ ] **Health checking** — Active (registry pings services) or heartbeat (services report to registry). Configure interval (10-30s), timeout, and eviction threshold.
- [ ] **Self-preservation** — When registry can't reach many services (network partition), should it evict all (risky) or preserve state (stale but safe)? Know your registry's default behavior.
- [ ] **Logical names, not IPs** — Services call `order-service`, not `10.0.1.5:8080`. DNS or registry resolves the logical name to current instances.
- [ ] **Registry secured** — Management endpoints behind auth. No public exposure. Read access may be open internally; write access restricted.

**Technology options:**

| Tool | Model | CAP | Best for |
|------|-------|-----|----------|
| Kubernetes DNS + Service | Server-side, built-in | CP | Already on K8s (most common 2026) |
| HashiCorp Consul | Both | CP | Multi-platform, service mesh, KV store |
| Netflix Eureka | Client-side | AP | Spring Boot shops, simpler setups |
| AWS Cloud Map | Server-side, managed | Managed | AWS-only deployments |

## 3. Load Balancing

### Edge (External → Internal)

- [ ] **Edge LB is the ONLY internet entry point** — No direct service exposure. All external traffic goes through edge LB → gateway → service.
- [ ] **TLS termination at edge** — Auto-renewed certificates (Let's Encrypt, ACM, cert-manager). TLS 1.2 minimum, prefer 1.3. HSTS enabled.
- [ ] **Edge LB is not a SPOF** — Multi-instance or cloud-managed (ALB, Cloud Load Balancer). Auto-scaling under load.
- [ ] **Layer 7 at edge** — Content-based routing (path, headers, host). Auth, rate limiting, WAF rules applied here.
- [ ] **Connection pooling** — HTTP keep-alive to backends. Reuse connections. Configured timeouts: connect < read < client.

### Internal (Service → Service)

- [ ] **Internal LB for service-to-service** — Client-side (Spring Cloud LoadBalancer, gRPC built-in, Envoy sidecar) or centralized (internal LB, K8s Service). Client-side eliminates an extra hop.
- [ ] **Layer 4 is usually sufficient internally** — Route by IP:port. No need for content-based routing between internal services.
- [ ] **Algorithm chosen with reason** — Round-robin (default, equal instances), least-connections (variable request duration), weighted (canary/mixed-size instances), latency-based (geo-distributed).

### Health & Failure

- [ ] **Active health checks** — LB pings `/health` every 5-30s. Failed → removed from pool automatically. No manual intervention needed.
- [ ] **Passive health checks / outlier detection** — Observe actual request outcomes. Too many 5xx from one instance → eject even if health endpoint passes.
- [ ] **Sticky sessions avoided** — Makes instances stateful. Breaks on scale-down/crash. Use shared state (Redis/DB) instead. Only if absolutely required (legacy WebSocket without session replication).
- [ ] **Request retry on connection failure** — Only for idempotent methods (GET, PUT, DELETE). Never auto-retry POST without idempotency key.

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
- [ ] **Authentication at the gateway** — Validate JWT/API key at the edge. Forward verified identity (claims, headers) to downstream. Services don't re-implement login flows.
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

- [ ] **Pattern choice** — (1) Gateway validates JWT, forwards claims as headers (simple, but internal traffic must be trusted). (2) Every service validates JWT independently via JWKS cache (defense in depth, recommended for production). (3) Token introspection (instant revocation, but auth server dependency per request).
- [ ] **Recommended: every service validates** — Gateway handles login flow AND validates. Downstream services ALSO validate independently. No single point of trust.

### OAuth2 / OpenID Connect

- [ ] **Dedicated auth server** — Keycloak (self-hosted), Auth0/Okta (SaaS), Cognito (AWS). Never build your own OAuth2 server.
- [ ] **Authorization Code + PKCE** — For all user-facing login flows (SPA, mobile, server-rendered). Never Implicit or Password grants.
- [ ] **Client Credentials** — For service-to-service calls. Machine-to-machine, no user involved. Short-lived tokens.
- [ ] **Access tokens short-lived** — ≤ 15 minutes. Signed with RS256 or ES256 (asymmetric — services validate with public key, never need the private key).
- [ ] **Refresh tokens long-lived + rotated** — 7-30 days. Each use issues new refresh token, invalidates old. Stolen token detected on next legitimate use.
- [ ] **JWKS endpoint** — Auth server publishes public keys at `/.well-known/jwks.json`. Services cache keys. Key rotation: new key published, old kept until existing tokens expire.
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

> **When to adopt:** ≥ 10 services, polyglot stack, zero-trust requirement, need traffic splitting, or mTLS without code changes.
> **When to skip:** < 10 services, single language, team can manage cross-cutting concerns in code, operational overhead not justified.

- [ ] **What it provides** — mTLS (encryption + identity between services), traffic management (retries, timeouts, circuit breakers), observability (metrics, traces without code), and traffic splitting (canary, A/B) — all without application code changes.
- [ ] **Sidecar proxy model** — Each pod gets an Envoy sidecar. All traffic flows through the proxy. Application is unaware of mesh. Language-agnostic.
- [ ] **mTLS everywhere** — All service-to-service traffic encrypted and authenticated. Zero-trust by default. No plaintext internal traffic.
- [ ] **Traffic policies** — Retry budgets, timeouts, circuit breakers configured in mesh policy (not application code). Per-route, per-service tuning.
- [ ] **Traffic splitting** — Canary releases (5% → 25% → 100%), A/B testing, header-based routing to specific versions. Mesh handles this at the infrastructure layer.
- [ ] **Observability built-in** — Request metrics (rate, error, latency), distributed traces, and access logs generated by sidecars. No instrumentation code needed for golden signals.
- [ ] **Authorization policies** — Define which services can talk to which. `allow order-service → payment-service`. Deny by default. Least privilege at the network layer.
- [ ] **Resource overhead** — Each sidecar consumes CPU + memory. Budget for it. Typically 50-100MB RAM, 0.1 CPU per sidecar. At 100 services = significant. Measure before production.
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

- [ ] **REST for external + simple internal** — JSON over HTTP. Universal. Every language supports it. Use for CRUD, simple queries between services.
- [ ] **gRPC for internal high-performance** — Protobuf serialization (10x smaller than JSON), HTTP/2 multiplexing, bidirectional streaming. Use for service-to-service where latency matters. Not for browser clients (without grpc-web).
- [ ] **Timeout on every outbound call** — Connection timeout + read timeout. Shorter than caller's own timeout. No call waits forever.
- [ ] **Circuit breaker on every outbound call** — Stop calling failing downstream after N failures. Fail fast. Return fallback. See [API Checklist — Circuit Breaker](api.md).
- [ ] **Retry only idempotent operations** — GET, PUT, DELETE: safe to retry. POST: only with idempotency key. Exponential backoff + jitter. Max 3 retries.
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
- [ ] **Health checks** — `/health` (liveness: is the process alive?) and `/ready` (readiness: can it serve? DB connected? Dependencies reachable?). K8s uses both. Gateway uses readiness.
- [ ] **SLOs defined and measured** — "99.9% of requests < 200ms" or "error rate < 0.1%." Track error budget. Alert when burning budget too fast. More actionable than "alert on 5xx spike."
- [ ] **Dashboards as code** — Grafana JSON provisioned from git. Not hand-tweaked in production. Dashboard changes go through PR review.
- [ ] **Alerting on symptoms, not causes** — Alert: "users experiencing errors" (symptom). Not: "CPU at 80%" (cause that may not affect users). Error budget burn rate > threshold → page. Circuit breaker open → page. DLQ growing → warn.
- [ ] **Correlation across services** — Trace ID in every log, metric label, and error report. Click from alert → dashboard → traces → logs. No manual correlation.
- [ ] **Infrastructure-level dashboard** — Per-service: health status, circuit breaker state, error rate, latency p50/p95/p99. Cross-service: dependency map, traffic flow, failure propagation.

## 9. Security (Infrastructure-Level)

- [ ] **Zero trust** — No service trusts another just because they share a network. Every request authenticated and authorized regardless of origin. Network is not a security boundary.
- [ ] **mTLS for service-to-service** — All internal traffic encrypted and mutually authenticated. Service mesh handles this transparently. Or manual cert management (harder, not recommended without mesh).
- [ ] **Network policies** — K8s NetworkPolicies define which services can talk to which. Default deny, explicit allow. Least privilege at the network layer.
- [ ] **Secrets management** — HashiCorp Vault, AWS Secrets Manager, K8s External Secrets Operator. Secrets injected at runtime, not baked into images or committed to git. Auto-rotation.
- [ ] **TLS everywhere** — Edge to internal: TLS 1.2+ minimum, TLS 1.3 preferred. Internal: mTLS via mesh or manual. No plaintext HTTP in production (even internally).
- [ ] **Certificate auto-renewal** — cert-manager (K8s), Let's Encrypt, ACM. No manual certificate rotation. Expiry alerts as backup.
- [ ] **Container security** — Non-root containers. Read-only filesystem where possible. No privileged pods. Vulnerability scanning in CI (Trivy, Snyk). Minimal base images (distroless, alpine, scratch).
- [ ] **Supply chain security** — Image signing (cosign, Notary). Admission controller rejects unsigned images. SBOM generation. Dependency scanning.
- [ ] **Gateway admin isolated** — Admin API on separate port/network. Not internet-facing. Credentials rotated. Access logged.

## 10. Deployment & Orchestration

### Containerization

- [ ] **One container per service** — Multi-stage builds. Minimal final image. Non-root user. Health check instruction or K8s probe.
- [ ] **Image registry** — Private registry (ECR, GCR, Harbor). Images tagged with git SHA, not `:latest`. Immutable tags in production.
- [ ] **Resource requests & limits** — Every pod has CPU/memory requests (scheduling) and limits (OOM protection). Tune based on actual usage, not guesses.

### Kubernetes (or equivalent orchestrator)

- [ ] **Namespace per environment or team** — Isolate dev/staging/prod. Or isolate by team/domain. RBAC per namespace.
- [ ] **Horizontal Pod Autoscaler** — Scale on CPU, memory, or custom metrics (queue depth, request rate). Min/max replicas defined.
- [ ] **Pod Disruption Budget** — `minAvailable` or `maxUnavailable`. Prevents draining all pods during node maintenance.
- [ ] **Rolling deployment** — Zero-downtime by default. `maxSurge: 1`, `maxUnavailable: 0`. Health check must pass before old pod is killed.
- [ ] **Canary / Blue-Green** — Shift traffic gradually (5% → 25% → 100%). Auto-rollback on error rate spike. Mesh or Argo Rollouts handles traffic splitting.
- [ ] **Startup order handled gracefully** — Services retry connecting to dependencies on startup. No fragile "start A before B" sequencing. Circuit breakers handle temporary unavailability.

### CI/CD

- [ ] **Independent pipelines per service** — Service A deploys without Service B. Shared library changes trigger affected services only.
- [ ] **Pipeline stages** — Lint → test → build → push image → deploy staging → integration test → deploy production. Fail fast at each stage.
- [ ] **GitOps** — Desired state in git. PR merge triggers deployment. ArgoCD or Flux reconciles. No manual `kubectl` in production.
- [ ] **Feature flags** — Deploy code dark. Enable gradually. Kill broken features without redeploy. LaunchDarkly, Unleash, or DB-backed flags.
- [ ] **Contract tests in CI** — Consumer-driven contracts (Pact). Catches breaking API changes before deploy. Producer verifies consumer expectations.
- [ ] **Build once, deploy many** — Same image promoted through environments. Config differs via env vars, not rebuilds.

## 11. Resilience Patterns

- [ ] **Circuit breaker per downstream** — Different thresholds per dependency. Payment gateway: strict (fail fast on 30% errors). Email service: loose (tolerate 80% errors). See [API Checklist — Circuit Breaker](api.md).
- [ ] **Timeout at every boundary** — HTTP client, DB query, message consumer, external API. No call waits forever. Shorter than caller's own timeout.
- [ ] **Retry with backoff + jitter** — Exponential backoff prevents thundering herd. Jitter prevents synchronized retries. Only for transient failures. Max 3 attempts.
- [ ] **Bulkhead** — Separate thread pools / connection pools per downstream. One slow dependency doesn't exhaust resources for all outbound calls.
- [ ] **Rate limiting** — At gateway (per-client) and between services (per-caller). Protect downstream from burst. Graceful degradation over hard failure.
- [ ] **Graceful degradation** — If non-critical dependency fails: return partial data, cached stale data, or degraded response. Not 500 for the whole request.
- [ ] **Idempotency** — Every write operation should be safe to retry. Idempotency keys for mutations. Upserts in data stores. Dedup in consumers.
- [ ] **Chaos engineering** — Kill a random pod. Does the system recover? Circuit breakers open? Retries work? DLQ catches failed messages? Start small, expand gradually.

## 12. Data Management

- [ ] **Database per service (enforced)** — No shared tables, no shared schemas. If two services need the same data: one owns it, the other gets a copy via events or API.
- [ ] **Event sourcing (where appropriate)** — Store events, not state. Rebuild state from events. Audit trail for free. Not needed everywhere — use for domains where history matters (finance, orders).
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
- [ ] Registry is HA (≥ 2 nodes)

### Load Balancing & Gateway
- [ ] Edge LB is the only internet entry point
- [ ] TLS terminated at edge with auto-renewed certs
- [ ] Unhealthy instances removed from pool automatically
- [ ] Gateway validates auth on every request
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
- [ ] Rolling/canary deploys with auto-rollback
- [ ] GitOps: desired state in git, reconciled automatically
- [ ] Feature flags for gradual rollout

### Resilience
- [ ] Circuit breaker on every outbound call
- [ ] Timeout on every outbound call
- [ ] Graceful degradation when non-critical dependency fails
- [ ] Chaos tested: kill a pod, system recovers

---

## Build vs Adopt

| Concern | Build? | Use instead |
|---------|--------|-------------|
| Service registry | ❌ Never | K8s DNS, Consul, Eureka |
| Edge load balancer | ❌ Never | Traefik, NGINX, HAProxy, Cloud LB |
| API Gateway | ❌ Never | Kong, Traefik, Spring Cloud Gateway, Cloud API GW |
| OAuth2/OIDC server | ❌ NEVER | Keycloak, Auth0, Okta, Cognito |
| Service mesh | ❌ Don't | Istio, Linkerd, Consul Connect, Cilium |
| JWT validation | ✅ Use library | Framework libraries (jose, jsonwebtoken, Spring Security) |
| Circuit breaker | ✅ Use library | Resilience4j, opossum, gobreaker |
| Observability pipeline | ❌ Don't | OpenTelemetry + Prometheus + Grafana / Datadog |

---

## Related Checklists

- [API Checklist](api.md) — General API design, resilience patterns, testing
- [Spring Boot API Gateway](spring-boot-api-gateway.md) — Spring Cloud Gateway implementation
- [Spring Boot Eureka](spring-boot-eureka.md) — Service discovery with Eureka
- [Spring Boot Load Balancing](spring-boot-loadbalance.md) — Client-side LB with Spring Cloud
- [Spring Boot OAuth](spring-boot-oauth.md) — OAuth2 resource server + authorization server
- [Batch Checklist](batch.md) — Batch processing patterns
