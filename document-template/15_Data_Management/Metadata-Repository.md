---
document_type: Metadata Repository
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [metadata, repository, data-catalog, dmbok]
standard_ref:
  - DMBOK v2 — Metadata Management
---

# Metadata Repository

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Central repository for metadata — technical, business, and operational metadata about all data assets.

## 2. Metadata Types

| Type | Description | Examples | Source |
|------|-----------|---------|--------|
| [Technical Metadata] | [Structure, format, storage] | [Table names, column types, indexes] | [Database catalog] |
| [Business Metadata] | [Meaning, context, usage] | [Business names, definitions, owners] | [Business glossary] |
| [Operational Metadata] | [Usage, quality, lineage] | [Query logs, quality scores, lineage] | [Monitoring tools] |

## 3. Metadata Architecture

```mermaid
flowchart TD
    subgraph Sources["Metadata Sources"]
        DB[Database<br>Catalog]
        APP[Application<br>Config]
        ETL[ETL<br>Pipelines]
        BI[BI<br>Tools]
    end

    subgraph Repository["Metadata Repository"]
        TECHNICAL[Technical<br>Metadata]
        BUSINESS[Business<br>Metadata]
        OPERATIONAL[Operational<br>Metadata]
    end

    subgraph Consumers["Metadata Consumers"]
        DATA[Data<br>Catalog]
        LINEAGE[Data<br>Lineage]
        QUALITY[Data<br>Quality]
        SEARCH[Search &<br>Discovery]
    end

    DB --> TECHNICAL
    APP --> BUSINESS
    ETL --> OPERATIONAL
    BI --> OPERATIONAL

    TECHNICAL --> DATA
    BUSINESS --> DATA
    OPERATIONAL --> LINEAGE
    OPERATIONAL --> QUALITY
    DATA --> SEARCH

    style Sources fill:#e3f2fd,color:#000
    style Repository fill:#fff3e0,color:#000
    style Consumers fill:#e8f5e9,color:#000
```

## 4. Metadata Standards

| Standard | Description | Implementation |
|---------|-----------|---------------|
| [Naming conventions] | [Consistent naming] | [[Data-Standards]] |
| [Data classification] | [Classification tags] | [[Data-Classification-Schema]] |
| [Business glossary] | [Term definitions] | [[Business-Glossary]] |
| [Lineage tracking] | [Data origin and flow] | [[Data-Lineage-Documentation]] |

## 5. Metadata Collection

| Metadata | Collection Method | Frequency | Owner |
|---------|------------------|----------|-------|
| [Table/column names] | [Database introspection] | [On change] | [Automated] |
| [Business definitions] | [Manual entry] | [On change] | [Data Steward] |
| [Data lineage] | [ETL logging] | [On run] | [Automated] |
| [Quality scores] | [Quality monitoring] | [Daily] | [Automated] |
| [Usage statistics] | [Query logging] | [Real-time] | [Automated] |

## 6. Metadata Governance

| Rule | Description |
|------|-----------|
| [Completeness] | [All data assets have metadata] |
| [Accuracy] | [Metadata reflects current state] |
| [Timeliness] | [Metadata updated within SLA] |
| [Ownership] | [Every metadata element has an owner] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Catalog]] | Catalog using metadata |
| [[Metadata-Standards]] | Metadata standards |
| [[Data-Lineage-Documentation]] | Lineage metadata |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Metadata is *data about data*. Without it, you can't find, understand, or trust your data.
