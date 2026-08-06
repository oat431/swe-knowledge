---
title: "Data Lineage and Provenance"
note_type: capability-topic
capability_area: data-integration
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - lineage
  - provenance
---

# Data Lineage and Provenance

> Tracking the origin, transformations, and movement of data across systems to enable impact analysis, compliance reporting, and root-cause debugging.

## Why This Is a Senior Skill

A mid-level engineer documents where data comes from in a wiki page. A senior engineer **designs automated lineage capture, builds impact analysis tooling, and ensures compliance-grade provenance** that survives audits and system migrations.

Lineage is not a documentation exercise: it is the foundation for impact analysis (what breaks if I change this source?), root-cause debugging (where did this bad value originate?), and regulatory compliance (can you prove where this number came from?). The senior engineer treats lineage as infrastructure, not documentation.

## Core Frameworks

### Lineage Capture Methods

| Method | How it works | Completeness | Overhead |
|---|---|---|---|
| Instrumentation (code-level) | Pipeline code emits lineage events | High if comprehensive | Requires framework support |
| Log parsing | Parse execution logs to reconstruct flow | Medium, misses implicit transforms | Low runtime cost |
| Query parsing | Parse SQL/query logs for table-level lineage | Table-level only | Storage engine dependent |
| Metadata hooks | Intercept catalog/registry calls | High for registered assets | Requires metadata platform |
| Manual annotation | Engineers document flows | Variable, decays over time | High human cost |

### Lineage Granularity Levels

| Level | Scope | Use case |
|---|---|---|
| System-level | Source system to target system | High-level architecture diagrams |
| Table-level | Source table to target table | Impact analysis for schema changes |
| Column-level | Source column through transforms to target column | Compliance, field-level access control |
| Row-level | Individual record provenance | Audit trail, dispute resolution |

### Compliance Lineage Requirements

| Regulation | Lineage requirement | Retention |
|---|---|---|
| GDPR (EU) | Data origin and processing purpose documented | Duration of processing + retention period |
| BCBS 239 (banking) | Full lineage for risk reports, aggregation documented | 7 years minimum |
| SOX (US public companies) | Audit trail for financial data transformations | 7 years |
| HIPAA (US healthcare) | PHI access and disclosure tracking | 6 years |

## In Practice

**Automate lineage capture.** Manual lineage documentation decays immediately after creation. Use framework-level instrumentation (OpenLineage, Marquez, or cloud-native lineage services) that captures lineage as a side effect of pipeline execution. Every pipeline run should emit lineage events.

**Column-level lineage is essential for compliance.** Table-level lineage tells you that orders flow to the revenue report. Column-level lineage tells you that the customer_phone_number field was transformed, hashed, and then dropped. Regulators increasingly require field-level provenance.

**Impact analysis is the killer use case.** When a source team proposes renaming a column, lineage tells you exactly which downstream reports, dashboards, and models are affected. Without automated lineage, this analysis takes days of manual investigation and is often incomplete.

## Practical Exercise

Build a lineage map for a revenue reporting pipeline:

1. Source: orders table (PostgreSQL), customers table (CRM API), exchange rates (external feed)
2. Transforms: join orders with customers, convert currency, aggregate by month and region
3. Target: revenue_fact table, executive dashboard, regulatory filing
4. Requirements: column-level lineage for compliance, impact analysis capability

Document:
- How you would capture lineage automatically
- The column-level lineage for the revenue_amount field
- What downstream assets break if the orders.total_cents column is renamed
- How you would prove provenance for a specific row in a regulatory audit

## Knowledge Connections

- [[DMBoK v2 - Overview]] : DMBOK metadata management and lineage concepts
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/05_Cross_System_Orchestration]] : lineage across orchestrated DAGs
- [[career-path/09_Data_and_ML_Engineer/05_Data_Security_and_Privacy/00_overview]] : lineage for access control and privacy
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/06_Remediation_and_Root_Cause]] : lineage for root-cause analysis

## Key Takeaways

- Lineage must be captured automatically as a side effect of execution, not documented manually
- Column-level lineage is increasingly required for compliance, not just debugging
- Impact analysis is the primary business value: knowing what breaks before you change something
- Row-level provenance is expensive and only justified for audit and dispute resolution use cases
- Lineage is infrastructure: it must be versioned, stored durably, and queryable
- Compliance requirements (GDPR, BCBS 239, SOX) define minimum lineage retention and granularity
