---
tags:
- architecture
- microservices
- overview
- programming
---

# Microservice Architecture — Overview

A practical guide to designing, building, and operating microservice systems. From decomposition to deployment — with the patterns, tools, and trade-offs you need in production.

> For the architectural philosophy behind service boundaries, see **[[Boundaries]]** and **[[Main Component and Services]]** in the Clean Architecture vault.

---

## Topics

### 01 Decomposition

> [[011 Decomposition Patterns]] — How to split a monolith: by business capability, subdomain, transaction boundary

### 02 Integration

> [[021 API Gateway]] — Single entry point, routing, auth, rate limiting
> [[022 API Contracts & Versioning]] — Contract-first, OpenAPI/Protobuf, backward compatibility, Pact
> [[023 Event-Driven Architecture]] — Async communication, event bus, pub/sub
> [[024 Messaging Patterns]] — Kafka, RabbitMQ, message ordering, idempotency
> [[025 Outbox Pattern]] — Transactional outbox, CDC, solving the dual-write problem

### 03 Data

> [[031 Database per Service]] — Isolation, data ownership, shared-nothing
> [[032 Eventual Consistency Patterns]] — Living with eventual consistency, conflict resolution, CRDTs
> [[033 Saga Pattern]] — Distributed transactions across services
> [[034 CQRS & Event Sourcing]] — Separate read/write models, audit trails

### 04 Resilience

> [[041 Circuit Breaker]] — Stop cascading failures before they spread
> [[042 Retry & Timeout]] — When to retry, when to give up, how to degrade
> [[043 Bulkhead Pattern]] — Isolate failures to contained compartments
> [[044 Chaos Engineering]] — Prove resilience works by breaking things on purpose

### 05 Observability

> [[051 Logging & Monitoring]] — Centralized logs, metrics, dashboards
> [[052 Distributed Tracing]] — Follow a request across services
> [[053 Health Checks]] — Liveness, readiness, dependency checks
> [[054 SLOs & Error Budgets]] — Measure reliability, burn rate alerting, error budget policy

### 06 Infrastructure

> [[061 Service Discovery]] — How services find each other at runtime
> [[062 Load Balancing]] — Edge vs internal, L4/L7, algorithms, outlier detection
> [[063 Configuration Management]] — Externalized config, feature flags
> [[064 Security Patterns]] — mTLS, OAuth2, API keys, JWT
> [[065 Zero Trust & Network Security]] — Network policies, pod security, supply chain

### 07 Deployment

> [[071 Containers & Orchestration]] — Docker, Kubernetes, pod architecture
> [[072 Deployment Strategies]] — Blue-green, canary, rolling updates
> [[073 Service Mesh]] — Istio, Linkerd, sidecar proxies
> [[074 GitOps & CI-CD Pipelines]] — ArgoCD, Flux, independent pipelines, git as source of truth

---

## My Experiments

| Component | Repository |
|-----------|-----------|
| API Gateway | [try-api-gateway](https://github.com/oat431/try-api-gateway) |
| Service Discovery | [spb3-service-discovery](https://github.com/oat431/spb3-service-discovery) |
| REST Service | [spb3-demo-rest-api](https://github.com/oat431/spb3-demo-rest-api) |
| GraphQL Service | [spb3-demo-graphql-api](https://github.com/oat431/spb3-demo-graphql-api) |
| DB Service | [spb3-dynamic-allowed-cors](https://github.com/oat431/spb3-dynamic-allowed-cors) |

> Home server: Ubuntu 24.04 at [server.panomete.com](https://server.panomete.com)

---

## How to Read

| Goal | Path |
|------|------|
| **Designing a new system** | 01 → 02 → 03 (decompose first, then integrate, then handle data) |
| **System keeps breaking** | 04 (resilience) → 05 (observability — you can't fix what you can't see) |
| **Deploying to production** | 07 → 06 (deployment + infrastructure concerns) |
| **Quick reference** | Each file is self-contained — pick by number |

---

## Sources

- Newman, Sam. *Building Microservices*, 2nd ed., O'Reilly, 2021.
- Richardson, Chris. *Microservices Patterns*, Manning, 2018.
