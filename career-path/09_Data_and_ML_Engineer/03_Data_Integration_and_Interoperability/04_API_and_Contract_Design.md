---
title: "API and Contract Design"
note_type: capability-topic
capability_area: data-integration
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - api-design
  - data-contracts
---

# API and Contract Design

> Defining explicit data contracts between producers and consumers with versioning, schema validation, and contract testing to prevent silent breakage.

## Why This Is a Senior Skill

A mid-level engineer builds an API that returns data. A senior engineer **defines the contract, negotiates breaking-change policies, and ensures that producers and consumers can evolve independently** without silent data corruption.

Data contracts are the interface between teams. When they are implicit (undocumented, unversioned, unvalidated), every schema change is a game of roulette. The senior engineer makes contracts explicit, machine-readable, and enforced.

## Core Frameworks

### API Style Selection for Data Services

| Style | Best for | Trade-offs |
|---|---|---|
| REST (resource-oriented) | CRUD on entities, public APIs | Simple, cacheable; over-fetching, N+1 queries |
| GraphQL | Flexible queries, frontend-driven | Complex caching, harder to version |
| gRPC | High-throughput internal services | Binary protocol, requires code generation |
| Async (events/webhooks) | Event-driven, decoupled | No synchronous response, harder to debug |

### Contract Versioning Strategies

| Strategy | Mechanism | Breaking change handling |
|---|---|---|
| URL versioning (/v1/, /v2/) | Separate endpoints per version | Run both versions during migration |
| Header versioning | Version in Accept header | Same endpoint, content negotiation |
| Schema evolution (Avro/Protobuf) | Forward/backward compatibility flags | Schema registry enforces compatibility |
| Consumer-driven contracts | Consumers define expected shape | Producer tests against all consumer contracts |

### Data Contract Components

| Component | Purpose | Enforcement |
|---|---|---|
| Schema definition | Field names, types, nullability | Schema registry validation |
| Semantic rules | Business meaning, valid ranges | Contract tests |
| SLA commitments | Latency, availability, freshness | Monitoring and alerting |
| Deprecation policy | How long old versions are supported | Governance process |
| Ownership | Who is accountable for the contract | Documentation and on-call |

## In Practice

**Consumer-driven contract testing** is the most effective pattern for preventing breakage. Each consumer writes a test that describes the shape of data it expects. The producer runs all consumer contracts in CI before deploying. This catches breaking changes before they reach production.

**GraphQL is not always the answer.** It excels when consumers need flexible queries and the data model is well-understood. It struggles with caching, versioning, and operational complexity. For data platform APIs where consumers are known and stable, REST with explicit versioning is often simpler and more predictable.

**Contracts must include freshness SLAs.** A schema-perfect response that is 3 days stale is useless for real-time use cases. Define and monitor freshness as part of the contract, not as an afterthought.

## Practical Exercise

Design a data contract for a customer profile API:

1. Producer: customer service that owns customer master data
2. Consumers: billing system (needs address, payment method), analytics (needs full profile), mobile app (needs display name and preferences)
3. Requirements: schema can evolve, breaking changes require 6-month deprecation window

Document:
- Your API style choice with reasoning for each consumer
- Your versioning strategy
- How you handle a breaking change (e.g., renaming a field)
- Your contract testing approach

## Knowledge Connections

- [[software-engineering-note/06_Software_Engineering_Operations/08_Service_Operations_and_Support]] : service interface design
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/03_Change_Data_Capture]] : CDC as a contract surface
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/03_Quality_Rules_and_Validation]] : validation as contract enforcement
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/06_Data_Lineage_and_Provenance]] : lineage through API boundaries

## Key Takeaways

- Data contracts must be explicit, versioned, and machine-enforced
- Consumer-driven contract testing catches breaking changes before deployment
- API style is a trade-off: REST for stability, GraphQL for flexibility, gRPC for throughput
- Freshness is part of the contract, not just schema correctness
- Deprecation policies must be defined before the first breaking change occurs
- Schema registries with compatibility checks prevent silent evolution breakage
