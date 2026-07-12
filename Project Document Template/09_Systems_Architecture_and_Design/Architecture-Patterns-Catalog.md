---
document_type: Architecture Patterns & Styles Catalog
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
architect: "[Solution Architect]"
classification: "Internal / Confidential"
tags: [architecture-patterns, styles, catalog, swebok]
standard_ref:
  - SWEBOK v4 — Architecture
---

# Architecture Patterns & Styles Catalog

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This catalog documents the architectural patterns and styles used in the project, providing a shared vocabulary for design decisions.

## 2. Pattern Catalog

### 2.1 Architectural Styles Applied

| Style | Description | Applied To | Rationale |
|-------|-----------|-----------|----------|
| [Layered] | [Separation into presentation, service, data layers] | [Overall architecture] | [Separation of concerns, team organization] |
| [Microservices] | [Independent, loosely coupled services] | [Service layer] | [Independent scaling, deployment, team autonomy] |
| [Event-Driven] | [Async communication via events] | [Inter-service communication] | [Decoupling, resilience] |
| [API-First] | [All functionality exposed via APIs] | [External interfaces] | [Extensibility, integration] |

### 2.2 Design Patterns Applied

| Pattern | Category | Applied To | Problem Solved | Trade-off |
|---------|----------|-----------|---------------|----------|
| [Repository] | Data Access | [All services] | [Abstracts data access from business logic] | [Extra layer of abstraction] |
| [Strategy] | Behavioral | [Processing Service] | [Different validation/approval strategies] | [More classes/interfaces] |
| [Observer] | Behavioral | [Event system] | [Decoupled notification on state change] | [Debugging complexity] |
| [Circuit Breaker] | Resilience | [Integration Service] | [Prevents cascading failures] | [May reject valid requests] |
| [Retry with Backoff] | Resilience | [External calls] | [Handles transient failures] | [Increased latency] |
| [Cache-Aside] | Performance | [API Gateway, Auth] | [Reduces DB load, faster reads] | [Stale data risk] |
| [CQRS] | Structural | [Reporting Service] | [Separate read/write models for performance] | [Complexity, eventual consistency] |
| [Saga] | Distributed | [Multi-step workflows] | [Distributed transaction management] | [Complexity, compensation logic] |

### 2.3 Integration Patterns Applied

| Pattern | Applied To | Purpose | Trade-off |
|---------|-----------|---------|----------|
| [Anti-Corruption Layer] | [ERP Integration] | [Isolate domain model from external system] | [Extra translation layer] |
| [API Gateway] | [All external APIs] | [Single entry point, auth, rate limiting] | [Single point of concern] |
| [Event Sourcing] | [Audit Log] | [Complete history of all changes] | [Storage, query complexity] |
| [Dead Letter Queue] | [Message processing] | [Handle failed messages] | [Monitoring needed] |

## 3. Pattern Decision Guide

| When You Need... | Use Pattern | Example |
|-----------------|-----------|---------|
| [Separate concerns] | [Layered] | [Presentation / Service / Data] |
| [Independent scaling] | [Microservices] | [Scale processing independently from notifications] |
| [Loose coupling] | [Event-Driven] | [Processing notifies without knowing who listens] |
| [Resilient external calls] | [Circuit Breaker + Retry] | [ERP integration with fallback] |
| [Fast reads] | [Cache-Aside] | [User session, reference data] |
| [Complex workflows] | [Saga] | [Multi-step approval process] |
| [Audit trail] | [Event Sourcing] | [All state changes recorded] |
| [Different algorithms] | [Strategy] | [Different validation rules per request type] |

## 4. Anti-Patterns to Avoid

| Anti-Pattern | Why Bad | Prevention |
|-------------|---------|-----------|
| [God Class] | [Single class does everything] | [SRP — single responsibility] |
| [Spaghetti Code] | [No structure, tangled dependencies] | [Layered architecture, dependency injection] |
| [Golden Hammer] | [Same solution for every problem] | [Evaluate patterns per problem] |
| [Premature Optimization] | [Optimizing before measuring] | [Profile first, optimize second] |
| [Distributed Monolith] | [Microservices with tight coupling] | [Event-driven, API contracts] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Software-Architecture-Document]] | Architecture using these patterns |
| [[Architecture-Decision-Records]] | Decisions to use specific patterns |
| [[Design-Pattern-Catalog]] | GoF patterns at code level |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** This catalog provides a shared vocabulary. When discussing architecture, reference patterns by name. When introducing a new pattern, add it here with rationale and trade-offs.
