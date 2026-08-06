---
title: "Cross-System Orchestration"
note_type: capability-topic
capability_area: data-integration
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - orchestration
  - dag
---

# Cross-System Orchestration

> Designing directed acyclic graphs (DAGs) that coordinate data pipelines across multiple systems with proper dependency management, SLA cascading, and failure recovery.

## Why This Is a Senior Skill

A mid-level engineer schedules a single job in cron. A senior engineer **designs the dependency graph, defines SLA cascading rules, and architects failure recovery** so that one failed upstream task does not silently corrupt downstream results.

Orchestration is where individual pipelines become an enterprise data platform. The senior engineer is accountable for the end-to-end data availability SLA, which means understanding how delays and failures propagate through the dependency graph.

## Core Frameworks

### Orchestration Tool Selection

| Tool | Strengths | Limitations | Best for |
|---|---|---|---|
| Apache Airflow | Mature, Python-native, large ecosystem | Scheduler latency, not event-native | Batch DAGs, complex dependencies |
| Dagster | Asset-oriented, first-class data concepts | Younger ecosystem | Data asset pipelines, lineage-aware |
| Prefect | Modern Python, hybrid execution | Smaller community | Hybrid cloud/on-prem, dynamic workflows |
| Temporal/Cadence | Durable execution, retries built-in | Workflow programming model | Long-running, stateful orchestration |
| Event-driven (Kafka + Flink) | True event-driven, low latency | Complex debugging | Streaming orchestration, reactive |

### Dependency Management Patterns

| Pattern | Description | Risk |
|---|---|---|
| Sensor-based | Poll for upstream completion | Wasted resources, polling lag |
| Event-triggered | Upstream publishes completion event | Requires event infrastructure |
| Time-based | Schedule assuming upstream is done | Silent failure if upstream is late |
| Data-aware | Check data asset freshness before running | Requires data catalog integration |

### SLA Cascading Model

| Upstream SLA | Downstream buffer | Total downstream SLA |
|---|---|---|
| 06:00 completion | 30 min processing | 06:30 availability |
| 06:00 completion | 2 hr processing + 30 min validation | 08:30 availability |
| Late by 1 hr | Cascade delay or run partial | Communicate revised SLA |

## In Practice

**Avoid time-based dependencies.** Scheduling a downstream job at 07:00 assuming upstream finishes by 06:30 works until it does not. Use sensors or event triggers so downstream jobs start only when upstream data is actually available.

**Define explicit SLA cascades.** If your dashboard SLA is 08:00 and the pipeline takes 2 hours, your raw data SLA must be 06:00. Document these cascades and alert when upstream delays threaten downstream SLAs. Communicate proactively to consumers before they notice.

**Design for partial failure.** When one branch of a DAG fails, decide explicitly: do dependent branches wait, skip, or run with stale data? Each choice has different correctness and availability trade-offs. Default to fail-fast with explicit overrides for known-safe scenarios.

## Practical Exercise

Design an orchestration DAG for a daily business intelligence pipeline:

1. Upstream: 3 source system extracts (finance, sales, CRM), each with different SLAs
2. Processing: join sources, apply business rules, build aggregates
3. Downstream: executive dashboard (SLA 08:00), ML feature store (SLA 10:00)
4. Requirements: handle upstream delays, partial failures, and weekend catch-up

Document:
- Your dependency strategy (sensor vs event vs time-based)
- SLA cascade from source extracts to dashboard
- Failure recovery: what happens when one source is 3 hours late
- How you handle backfill for a logic error discovered on day 5

## Knowledge Connections

- [[06_Data_Integration_and_Interoperability]] : DMBOK orchestration and process controls
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/01_Batch_ETL_Design]] : individual pipeline design
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/06_Data_Lineage_and_Provenance]] : lineage across orchestrated pipelines
- [[career-path/07_SRE_and_Platform_Engineer/01_Service_Objectives/00_overview]] : SLO design for data availability

## Key Takeaways

- Time-based dependencies are the most common source of silent data staleness
- SLA cascading must be explicit and documented: every downstream consumer inherits upstream delay risk
- Sensor-based or event-triggered dependencies are more reliable than time-based scheduling
- Partial failure handling must be a deliberate design choice, not a framework default
- Asset-oriented orchestration (Dagster-style) makes data contracts first-class
- Orchestration is the coordination layer: individual pipeline correctness is assumed, not enforced here
