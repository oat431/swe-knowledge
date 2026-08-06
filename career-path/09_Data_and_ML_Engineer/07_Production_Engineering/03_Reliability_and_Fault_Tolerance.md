---
title: "Reliability and Fault Tolerance"
note_type: capability-topic
capability_area: production-engineering
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - reliability
  - fault-tolerance
  - idempotency
---

# Reliability and Fault Tolerance

> Designing data pipelines and systems that survive component failures without data loss, duplication, or corruption.

## Why This Is a Senior Skill

Mid-level engineers handle errors when they occur. Senior engineers design systems where failures are expected and handled gracefully: idempotent pipelines that can be retried safely, dead letter queues that capture failures for investigation, and exactly-once processing where the use case demands it.

The senior challenge is that infrastructure fails regularly. A pipeline that works when everything is healthy but corrupts data during a failure is not production-ready.

## Core Frameworks

### Idempotency Patterns

| Pattern | How It Works | Use Case |
|---------|-------------|----------|
| Upsert by key | INSERT ON CONFLICT UPDATE | Data loads where duplicates should merge |
| Overwrite by partition | REPLACE partition on each run | Daily aggregations, snapshots |
| Deduplication window | Track processed IDs, skip duplicates | Event processing with retry |
| Deterministic output | Same input always produces same output | Functional transformations |
| Write-ahead log | Log intent before execution, replay on failure | Transactional systems |

### Processing Guarantees

| Guarantee | Meaning | Cost | Use Case |
|-----------|---------|------|----------|
| At-most-once | Messages may be lost, never duplicated | Lowest | Logging, metrics where loss is acceptable |
| At-least-once | Messages are never lost, may be duplicated | Medium | Event processing with idempotent downstream |
| Exactly-once | Messages processed exactly once | Highest | Financial transactions, inventory updates |

### Failure Handling Strategies

| Strategy | When to Use | Implementation |
|----------|------------|----------------|
| Retry with backoff | Transient failures: network timeout, temporary unavailability | Exponential backoff, jitter, max retries |
| Circuit breaker | Dependency is failing, stop hammering it | Open circuit after N failures, half-open after timeout |
| Dead letter queue | Messages that cannot be processed after retries | Capture failed messages for investigation |
| Fallback | Primary path fails, use degraded path | Return cached result, default value, or skip |
| Compensation | Undo a partial transaction | Saga pattern: compensating actions for each step |

## In Practice

**Make every pipeline idempotent.** If a pipeline fails halfway and you restart it, the result should be identical to a successful run. Upserts, partition overwrites, and deduplication windows all achieve idempotency. Test by running the same pipeline twice and verifying no duplication.

**Use at-least-once with idempotent downstream for most cases.** Exactly-once is expensive and complex. At-least-once with an idempotent sink achieves the same correctness at lower cost. Only use exactly-once when the downstream cannot be made idempotent.

**Implement dead letter queues for every pipeline.** When a message fails after retries, capture it with its error context. A dead letter queue lets you investigate failures without losing data. Review the DLQ daily and fix root causes.

**Design circuit breakers for external dependencies.** If a downstream API is failing, stop sending requests. A circuit breaker opens after N consecutive failures, returns fallback responses, and periodically tests if the dependency has recovered. This prevents cascading failures.

**Test failure scenarios.** Inject failures in staging: kill a node, partition the network, fill a disk. Verify that the system degrades gracefully and recovers when the failure is resolved. Untested fault tolerance is theoretical fault tolerance.

## Practical Exercise

Design fault tolerance for a data pipeline you are building:
1. Identify every step that can fail: source reads, transformations, sink writes
2. Make each step idempotent: how does it behave if run twice?
3. Choose a processing guarantee: at-most-once, at-least-once, or exactly-once
4. Design a dead letter queue: what information do you capture for failed records?
5. Implement a circuit breaker for the most critical external dependency
6. Write a failure test: inject a failure and verify the system recovers correctly

## Knowledge Connections

- [[software-engineering-note/06_Software_Engineering_Operations/Software Engineering Operations Overview]]: reliability engineering
- [[01_Distributed_Systems_for_Data]]: distributed systems have more failure modes
- [[06_Operational_Runbooks_and_On_Call]]: runbooks for failure recovery
- [[04_Data_Quality/00_overview|Data Quality]]: data quality checks catch corruption
- [[05_CI_CD_for_Data_and_ML]]: testing failure scenarios in CI

## Common Pitfalls

- Non-idempotent pipelines: retries duplicate data, corrupting downstream systems
- Exactly-once everywhere: 10x cost for use cases where at-least-once with idempotent sink suffices
- No dead letter queue: failed records are logged and forgotten, data loss accumulates
- Circuit breakers without fallback: circuit opens, system fails hard instead of degrading gracefully
- Untested fault tolerance: failure handling code that has never run in a real failure scenario

## Key Takeaways

- Idempotency is the foundation: pipelines that can be safely retried survive infrastructure failures
- At-least-once with idempotent downstream is usually sufficient and cheaper than exactly-once
- Dead letter queues capture failures for investigation: never silently drop failed records
- Circuit breakers prevent cascading failures: stop hammering a failing dependency
- Test failure scenarios: untested fault tolerance is theoretical
- Senior engineers design for failure, not hope it does not happen
