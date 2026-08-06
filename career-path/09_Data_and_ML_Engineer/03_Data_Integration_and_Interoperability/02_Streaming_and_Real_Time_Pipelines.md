---
title: "Streaming and Real-Time Pipelines"
note_type: capability-topic
capability_area: data-integration
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - streaming
  - real-time
---

# Streaming and Real-Time Pipelines

> Designing event-driven data pipelines that process continuous streams with correct ordering, exactly-once guarantees, and graceful handling of late or out-of-order data.

## Why This Is a Senior Skill

A mid-level engineer connects a message broker to a consumer. A senior engineer reasons about **processing guarantees, windowing semantics, and the trade-off between consistency and availability** in the face of partition failures.

Streaming systems fail in subtle ways: messages arrive late, clocks drift, consumers lag behind producers, and exactly-once semantics break under network partitions. The senior engineer designs for these failure modes explicitly and chooses the right consistency model for each use case.

## Core Frameworks

### Delivery Semantics Decision Matrix

| Semantics | Mechanism | Acceptable when | Cost |
|---|---|---|---|
| At-most-once | Fire and forget | Loss is acceptable (metrics, sampling) | Lowest |
| At-least-once | Acknowledge after processing | Idempotent downstream (counts, aggregates) | Medium |
| Exactly-once | Transactional commit between source and sink | Financial, compliance, non-idempotent targets | Highest |

### Windowing Strategies

| Window type | Definition | Use case | Late data handling |
|---|---|---|---|
| Tumbling | Fixed non-overlapping intervals | Periodic aggregation (hourly counts) | Drop or side-output |
| Sliding | Overlapping fixed intervals | Moving averages, trend detection | Partial update |
| Session | Activity-based gaps | User session analytics | Merge on gap close |
| Global | Single window for all data | Running totals, global state | Accumulate indefinitely |

### Exactly-Once Implementation Patterns

| Approach | How it works | Trade-offs |
|---|---|---|
| Two-phase commit | Broker and sink coordinate commit | High latency, distributed consensus |
| Idempotent writes | Deduplicate at sink using message IDs | Requires unique IDs, extra sink logic |
| Transactional outbox | Write to local DB then publish | Consistent within one service boundary |
| Changelog replay | Reprocess from offset on failure | Higher recovery time, simpler runtime |

## In Practice

**Choosing your consistency level:** Most teams over-specify exactly-once. Ask: what is the actual cost of a duplicate vs the cost of added latency and complexity? For clickstream analytics, at-least-once with deduplication at the warehouse is cheaper and simpler. For payment processing, exactly-once is mandatory.

**Late data is inevitable:** No matter how tight your SLA, some data will arrive after its window closes. Design explicit late-data policies: drop, re-emit corrected results, or accumulate in a corrections stream. Communicate this policy to downstream consumers so they know what to expect.

**Backpressure:** When consumers fall behind producers, unbounded buffers cause out-of-memory failures. Design for backpressure: rate-limit producers, shed load explicitly, or scale consumers. Never rely on unbounded in-memory buffers.

## Practical Exercise

Design a streaming pipeline for real-time fraud detection:

1. Source: payment transaction events at 10,000 events/second
2. Processing: enrich with customer profile, apply fraud model, flag suspicious transactions
3. Sink: alert system (exactly-once) and analytics warehouse (at-least-once)

Document:
- Your windowing strategy for fraud pattern detection
- How you handle exactly-once for alerts vs at-least-once for analytics
- Your late-data policy for transactions arriving after the window closes
- Your backpressure strategy when the fraud model service degrades

## Knowledge Connections

- [[06_Data_Integration_and_Interoperability]] : DMBOK foundation for event-driven integration
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/03_Change_Data_Capture]] : CDC as a streaming source
- [[career-path/09_Data_and_ML_Engineer/06_ML_Lifecycle_and_MLOps/00_overview]] : real-time model serving
- [[career-path/07_SRE_and_Platform_Engineer/02_Observability/00_overview]] : monitoring streaming pipelines

## Key Takeaways

- Choose delivery semantics based on downstream idempotency, not on what sounds safest
- Late data is a first-class design concern, not an edge case
- Backpressure must be designed explicitly: unbounded buffers are a ticking time bomb
- Windowing strategy should match the business question, not the technology default
- Exactly-once has real latency and complexity costs: justify it with business impact
- Streaming pipelines need the same observability as batch: SLIs for lag, throughput, and error rate
