---
title: "Data Observability"
note_type: capability-topic
capability_area: data-quality
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - observability
  - monitoring
---

# Data Observability

> Continuously monitoring data pipelines and assets for freshness, volume, schema drift, and lineage-based impact so that quality degradation is detected before it affects consumers.

## Why This Is a Senior Skill

A mid-level engineer sets up alerts on pipeline success or failure. A senior engineer **designs observability that detects silent failures: data that is stale, sparse, duplicated, or structurally changed without the pipeline failing.**

Pipeline success does not mean data quality. A pipeline can complete successfully and produce empty output, duplicate rows, or stale data. The senior engineer designs observability that measures the data itself, not just the process that produced it.

## Core Frameworks

### Observability Pillars for Data

| Pillar | What it measures | Alert trigger | Example |
|---|---|---|---|
| Freshness | Time since last update | Data older than SLA | Table not updated in 6 hours |
| Volume | Row count, file size | Outside expected range | 80% fewer rows than yesterday |
| Schema | Column count, types, names | Unexpected change | New column added, type changed |
| Distribution | Value distributions, null rates | Statistical drift | Null rate jumped from 2% to 15% |
| Lineage | Upstream dependency health | Upstream asset stale | Source table not refreshed |

### Freshness Monitoring Patterns

| Pattern | How it works | Use case |
|---|---|---|
| Max timestamp check | MAX(updated_at) compared to SLA | Partitioned tables with known update patterns |
| Pipeline completion event | Orchestration emits success with timestamp | DAG-based pipelines |
| Consumer-side check | Consumer queries data and checks age | End-to-end freshness verification |
| Heartbeat row | Dedicated row updated on every pipeline run | Simple liveness check |

### Schema Drift Detection

| Change type | Detection method | Response |
|---|---|---|
| New column added | Column count or name comparison | Alert, update expectations if intentional |
| Column dropped | Column presence check | Halt downstream, investigate |
| Type changed | Schema registry comparison | Halt, assess impact |
| Nullable changed | Nullability metadata check | Update rules if needed |
| Primary key changed | Constraint metadata | Critical alert, halt all dependents |

## In Practice

**Monitor the data, not just the pipeline.** A pipeline that succeeds with zero output rows is a silent failure. Monitor row counts, value distributions, and freshness of the actual data assets, not just the exit code of the process that produced them.

**Freshness is the most common silent failure.** A pipeline scheduled for 06:00 that silently fails to run means consumers see yesterday's data at 08:00. Freshness monitoring catches this immediately. Define freshness SLAs per consumer and alert when they are at risk.

**Schema drift detection must be automated.** When an upstream team adds a column to a table your pipeline reads, your pipeline may silently ignore it or break in subtle ways. Schema drift detection catches these changes before they propagate. Integrate with schema registries or compare DDL snapshots.

## Practical Exercise

Design a data observability system for a data warehouse with 200 tables:

1. Critical tables (10): executive dashboards, regulatory reports
2. Standard tables (100): department analytics, operational reports
3. Development tables (90): experimentation, ad-hoc analysis
4. Requirements: detect stale data, volume anomalies, schema drift, with minimal alert fatigue

Document:
- Which pillars you monitor for each table tier
- Your freshness SLA definition for critical vs standard tables
- How you handle expected volume variation (weekends, month-end)
- Your alerting strategy to minimize fatigue while catching real issues

## Knowledge Connections

- [[career-path/07_SRE_and_Platform_Engineer/02_Observability/00_overview]] : SRE observability principles applied to data
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/02_Profiling_and_Anomaly_Detection]] : profiling feeds observability baselines
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/03_Quality_Rules_and_Validation]] : rules as observability checks
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/06_Data_Lineage_and_Provenance]] : lineage-based impact analysis

## Key Takeaways

- Data observability measures the data itself, not just the pipeline that produced it
- Freshness is the most common silent failure and must be monitored end-to-end
- Schema drift detection catches upstream changes before they break downstream consumers
- Volume monitoring must account for expected variation to avoid alert fatigue
- Observability tiers (critical, standard, development) allow differentiated monitoring investment
- Lineage-based observability propagates alerts through dependency chains to affected consumers
