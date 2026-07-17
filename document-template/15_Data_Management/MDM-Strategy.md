---
document_type: MDM Strategy
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [mdm, master-data-management, dmbok]
standard_ref:
  - DMBOK v2 — Master Data Management
---

# MDM Strategy (Master Data Management)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines strategy for managing master data — ensuring consistent, accurate reference data across all systems.

## 2. Master Data Domains

| Domain | Description | Source of Truth | Systems Using |
|--------|-----------|----------------|--------------|
| [Customer] | [Customer master data] | [Application DB] | [Application, ERP, DW] |
| [Product/Service] | [Product catalog] | [Application DB] | [Application, ERP] |
| [Employee] | [Staff master data] | [HR System] | [Application, ERP] |
| [Organization] | [Company structure] | [ERP] | [All systems] |
| [Reference Data] | [Codes, categories] | [Application DB] | [All systems] |

## 3. MDM Architecture

```mermaid
flowchart TD
    subgraph Sources["Master Data Sources"]
        APP[Application DB]
        ERP[ERP System]
        HR[HR System]
    end

    subgraph MDM["MDM Hub"]
        MASTER[Master<br>Records]
        MATCH[Match &<merge class=""></merge>]
        QUALITY[Quality<br>Rules]
    end

    subgraph Consumers["Consumers"]
        APP2[Application]
        DW[Data Warehouse]
        REPORTS[Reports]
    end

    APP --> MASTER
    ERP --> MASTER
    HR --> MASTER
    MASTER --> MATCH
    MATCH --> QUALITY
    QUALITY --> APP2
    QUALITY --> DW
    QUALITY --> REPORTS

    style Sources fill:#e3f2fd,color:#000
    style MDM fill:#fff3e0,color:#000
    style Consumers fill:#e8f5e9,color:#000
```

## 4. Master Data Rules

| Rule | Description | Implementation |
|------|-----------|---------------|
| [Single source of truth] | [One system owns each master data domain] | [Source system designation] |
| [Golden record] | [One canonical record per entity] | [Match & merge rules] |
| [Data quality] | [Master data meets quality standards] | [Quality rules enforcement] |
| [Change control] | [Master data changes are controlled] | [Change request process] |
| [Distribution] | [Master data is distributed to consumers] | [Sync mechanisms] |

## 5. Match & Merge Rules

| Domain | Match Criteria | Merge Strategy | Conflict Resolution |
|--------|---------------|---------------|-------------------|
| [Customer] | [Email (exact), Name (fuzzy)] | [Survivorship rules] | [Source of truth wins] |
| [Product] | [SKU (exact)] | [Latest wins] | [Source system wins] |
| [Employee] | [Employee ID (exact)] | [HR system wins] | [HR system wins] |

## 6. Data Stewardship for Master Data

| Domain | Steward | Responsibilities |
|--------|---------|-----------------|
| [Customer] | [Customer Data Steward] | [Quality, dedup, merge] |
| [Product] | [Product Data Steward] | [Catalog management] |
| [Employee] | [HR Data Steward] | [Employee records] |
| [Reference] | [Reference Data Steward] | [Codes, categories] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Reference-Data-Catalog]] | Reference data |
| [[Golden-Record-Definition]] | Golden record rules |
| [[Data-Governance-Charter]] | Governance framework |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** MDM ensures *one version of the truth*. Without it, every system has its own version of "customer."
