---
document_type: Data Stewardship Assignment
version: "1.0"
status: Draft
author: "[Data Governance Officer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-stewardship, roles, dmbok]
standard_ref:
  - DMBOK v2 — Data Governance
---

# Data Stewardship Assignment

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Assigns data stewards and owners to data domains — ensuring accountability for data quality, security, and lifecycle.

## 2. Stewardship Model

```mermaid
flowchart TD
    DGO[Data Governance<br>Officer] --> CS[Customer Data<br>Steward]
    DGO --> OS[Operations Data<br>Steward]
    DGO --> FS[Financial Data<br>Steward]
    DGO --> RS[Reference Data<br>Steward]

    CS --> C1[Customer]
    CS --> C2[Account]
    CS --> C3[Contact]
    OS --> O1[Request]
    OS --> O2[Transaction]
    OS --> O3[Workflow]
    FS --> F1[Payment]
    FS --> F2[Invoice]
    FS --> F3[Budget]
    RS --> R1[Lookup Tables]
    RS --> R2[Codes]
    RS --> R3[Categories]

    style DGO fill:#1a237e,color:#fff
```

## 3. Stewardship Assignments

| Data Domain | Data Owner | Data Steward | Backup Steward |
|------------|-----------|-------------|---------------|
| [Customer] | [VP Sales] | [Customer Data Steward] | [Operations Steward] |
| [Operations] | [VP Operations] | [Operations Data Steward] | [Customer Steward] |
| [Financial] | [CFO] | [Financial Data Steward] | [Operations Steward] |
| [Reference] | [Data Governance Officer] | [Reference Data Steward] | [Any Steward] |
| [System] | [CTO] | [System Data Steward] | [Operations Steward] |

## 4. Steward Responsibilities

| Responsibility | Frequency | Description |
|---------------|----------|-------------|
| [Data quality monitoring] | [Daily] | [Monitor DQ dashboards, investigate issues] |
| [Data issue resolution] | [As needed] | [Triage and resolve data quality issues] |
| [Metadata maintenance] | [Weekly] | [Update data catalog, glossary, definitions] |
| [Access request review] | [As needed] | [Review and approve data access requests] |
| [Policy compliance] | [Monthly] | [Ensure domain compliance with data policies] |
| [Stewardship reporting] | [Monthly] | [Report on domain data quality metrics] |

## 5. RACI Matrix

| Activity | Owner | Steward | IT | DGO |
|---------|-------|---------|-----|-----|
| [Data quality rules] | [A] | [R] | [C] | [I] |
| [Access approvals] | [A] | [R] | [R] | [I] |
| [Metadata updates] | [I] | [R] | [C] | [A] |
| [Issue resolution] | [I] | [R] | [R] | [A] |
| [Policy compliance] | [A] | [R] | [C] | [R] |

## 6. Stewardship Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| [Domain data quality score] | [Quality rules pass rate] | [≥ 95%] |
| [Issue resolution time] | [Avg days to resolve DQ issues] | [< 3 days] |
| [Metadata completeness] | [Fields with definitions / Total] | [100%] |
| [Access review completion] | [Reviews completed on time] | [100%] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Governance-Operating-Framework]] | Operating model |
| [[Data-Policy]] | Policies to enforce |
| [[Data-Quality-Scorecard]] | Quality metrics |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Without stewardship, data governance is just policy on paper. Stewards are the *hands and feet* of governance.
