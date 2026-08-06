---
title: "Change Data Capture"
note_type: capability-topic
capability_area: data-integration
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - cdc
  - data-integration
---

# Change Data Capture

> Capturing and propagating row-level changes from source databases to downstream systems with minimal latency, handling schema evolution and ensuring consistent state across consumers.

## Why This Is a Senior Skill

A mid-level engineer sets up a CDC connector and watches it run. A senior engineer **chooses between log-based and query-based capture based on impact tolerance, designs schema propagation policies, and handles the failure modes** that inevitably arise when source schemas evolve without warning.

CDC is deceptively simple in concept but operationally demanding: transaction logs rotate, schemas change mid-stream, consumers fall behind, and initial snapshots conflict with ongoing changes. The senior engineer designs for all of these scenarios and owns the operational runbook.

## Core Frameworks

### CDC Method Comparison

| Method | How it works | Latency | Source impact | Schema awareness |
|---|---|---|---|---|
| Log-based (WAL/binlog) | Reads database transaction log directly | Sub-second | Near-zero | Requires log parsing per DB engine |
| Query-based (timestamp/watermark) | Polls for rows changed since last check | Seconds to minutes | Query load on source | Implicit via schema query |
| Trigger-based | Database triggers write changes to audit table | Sub-second | Write amplification | Must maintain trigger definitions |

### Schema Evolution Handling

| Change type | Risk level | Strategy |
|---|---|---|
| Add nullable column | Low | Auto-propagate with default null |
| Add required column | Medium | Halt pipeline, notify team, backfill default |
| Drop column | High | Keep emitting with null until consumers migrate |
| Rename column | High | Emit both old and new names during transition |
| Type change | Critical | Halt, assess impact, coordinate migration |

### Initial Snapshot vs Incremental Sync

| Scenario | Approach | Considerations |
|---|---|---|
| New consumer, small table | Full snapshot then incremental | Simple, lock table briefly during snapshot |
| New consumer, large table | Chunked snapshot with concurrent CDC | Complex ordering, requires snapshot-aware offsets |
| Consumer recovery from offset | Resume from last committed position | Must handle gap between snapshot and CDC start |
| Schema changed during snapshot | Re-snapshot or merge with CDC stream | Coordinate with schema registry |

## In Practice

**Log-based is almost always preferable** when available. It captures every change including deletes, has near-zero impact on the source, and provides transaction boundaries. Query-based CDC is acceptable only when log access is unavailable or when change volume is low and polling overhead is negligible.

**Schema propagation is the hardest part of CDC.** When a source team adds a column, your pipeline either needs to propagate it automatically or halt and notify. Neither is universally right: auto-propagation risks downstream breakage, halting risks staleness. Define a policy per table based on downstream sensitivity.

**Initial snapshots are operationally dangerous.** A full-table scan on a 100GB table locks resources for hours. Design chunked snapshots with resumable checkpoints. Never start a snapshot without monitoring source database load.

## Practical Exercise

Design a CDC pipeline for a product catalog:

1. Source: PostgreSQL with 10M product rows, schema changes monthly
2. Consumers: search index (needs sub-second latency), analytics warehouse (eventual consistency OK)
3. Requirements: handle schema changes without breaking search index, recover from 4-hour outage

Document:
- Your CDC method choice with reasoning
- Schema evolution policy for each consumer
- How you handle the initial snapshot for the search index
- Your recovery strategy after a 4-hour consumer outage

## Knowledge Connections

- [[06_Data_Integration_and_Interoperability]] : DMBOK CDC concepts and latency spectrum
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/02_Streaming_and_Real_Time_Pipelines]] : CDC as a streaming source
- [[career-path/09_Data_and_ML_Engineer/02_Data_Modeling_and_Design/00_overview]] : schema design that enables clean CDC
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/06_Data_Lineage_and_Provenance]] : tracking CDC provenance

## Key Takeaways

- Log-based CDC is preferred for low impact and completeness; query-based is a fallback
- Schema evolution policy must be defined per table and per consumer, not globally
- Initial snapshots require chunking, checkpointing, and source-load monitoring
- Deletes are invisible to query-based CDC: use log-based when delete propagation matters
- Consumer lag monitoring is essential: silent lag is worse than loud failure
- CDC is not a silver bullet: it captures changes, not business events, so semantic enrichment is still needed
