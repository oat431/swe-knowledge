---
title: "Batch ETL Design"
note_type: capability-topic
capability_area: data-integration
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - etl
  - batch-processing
---

# Batch ETL Design

> Designing batch data pipelines that extract, transform, and load data reliably with idempotency, error handling, and backfill capabilities.

## Why This Is a Senior Skill

A mid-level engineer writes a script that moves data from A to B. A senior engineer designs the **failure semantics**: what happens when the source is slow, the transform fails midway, or the target is unavailable? They choose between ETL and ELT based on transformation complexity and target capabilities, design for idempotency so re-runs are safe, and build backfill strategies for historical corrections.

The senior skill is **predicting failure modes** and designing systems that recover gracefully without manual intervention or data corruption.

## Core Frameworks

### ETL vs ELT Decision Matrix

| Factor | ETL preferred | ELT preferred |
|---|---|---|
| Transformation complexity | Complex multi-step transforms | Simple SQL-pushdown transforms |
| Target compute | Limited or expensive | Scalable cloud warehouse |
| Data volume | Moderate, pre-filtered | Large, raw ingestion |
| Latency tolerance | Can batch-transform in-flight | Load raw, transform later |
| Schema flexibility | Fixed schemas | Schema-on-read |

### Idempotency Patterns

| Pattern | Mechanism | Use case |
|---|---|---|
| Upsert by key | INSERT ON CONFLICT UPDATE | Dimension tables with natural keys |
| Partition overwrite | Replace entire date partition | Fact tables partitioned by time |
| Temporal staging | Load to staging, merge to target | Complex multi-source joins |
| Watermark tracking | Track last-processed offset | Incremental loads with resume |

### Error Handling Strategy

| Error type | Response | Recovery |
|---|---|---|
| Transient (timeout, connection) | Exponential backoff retry | Automatic resume from watermark |
| Data validation failure | Dead-letter queue + alert | Manual review and replay |
| Schema mismatch | Halt pipeline + notify | Schema registry intervention |
| Target unavailable | Buffer in staging layer | Flush when target recovers |

## In Practice

**Designing for idempotency:** Every pipeline run should produce the same result whether run once or ten times. Use partition overwrites for time-series data: if the pipeline for 2024-01-15 runs three times, the partition contains the same final state. Avoid append-only patterns unless you have deduplication logic.

**Backfill strategy:** When business logic changes retroactively, you need to reprocess historical data. Design pipelines with explicit date-range parameters so backfills are first-class operations, not hacks. Maintain a separate backfill orchestration path that runs in parallel with incremental loads.

**Error budgets for batch:** Define acceptable failure rates. A pipeline that fails 1% of the time but recovers automatically may be acceptable. A pipeline that silently produces wrong data is never acceptable. Instrument for both failure detection and correctness verification.

## Practical Exercise

Design a batch ETL pipeline for a customer analytics use case:

1. Source: transactional database with 50M orders, updated continuously
2. Target: analytics warehouse with daily partitioned fact table
3. Requirements: idempotent daily loads, backfill capability, error handling for source timeouts

Document:
- Your ETL vs ELT choice with reasoning
- The idempotency mechanism you would use
- How backfill would work for correcting a month of data
- Your error handling and alerting strategy

## Knowledge Connections

- [[06_Data_Integration_and_Interoperability]] : DMBOK foundation for integration patterns
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/03_Quality_Rules_and_Validation]] : validation within pipelines
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/05_Cross_System_Orchestration]] : orchestration of batch jobs
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/06_Data_Lineage_and_Provenance]] : tracking what the pipeline produced

## Key Takeaways

- ETL vs ELT is a trade-off between transformation complexity and target compute capability
- Idempotency is non-negotiable: every pipeline must be safe to re-run without corruption
- Backfill is a first-class design requirement, not an afterthought
- Error handling must distinguish transient failures (retry) from data errors (dead-letter)
- Partition overwrite is the simplest idempotency pattern for time-series data
- Instrument for correctness, not just completion: a pipeline that succeeds but produces wrong data is worse than one that fails loudly
