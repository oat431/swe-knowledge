---
document_type: Data Catalog
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-catalog, discovery, metadata, dmbok]
standard_ref:
  - DMBOK v2 — Metadata Management
---

# Data Catalog

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Central inventory of all data assets — enabling discovery, understanding, and governance of data.

## 2. Data Asset Inventory

| # | Asset | Type | Location | Owner | Classification | Status |
|---|-------|------|---------|-------|---------------|--------|
| 1 | [customers] | [Table] | [PostgreSQL] | [Customer Steward] | 🔴 L1 | ✅ Active |
| 2 | [requests] | [Table] | [PostgreSQL] | [Operations Steward] | 🟡 L2 | ✅ Active |
| 3 | [transactions] | [Table] | [PostgreSQL] | [Financial Steward] | 🔴 L1 | ✅ Active |
| 4 | [categories] | [Table] | [PostgreSQL] | [Reference Steward] | 🟢 L3 | ✅ Active |
| 5 | [statuses] | [Table] | [PostgreSQL] | [Reference Steward] | 🟢 L3 | ✅ Active |
| 6 | [documents] | [Table] | [PostgreSQL] | [Operations Steward] | 🟡 L2 | ✅ Active |
| 7 | [audit_logs] | [Table] | [PostgreSQL] | [Security Steward] | 🟡 L2 | ✅ Active |
| 8 | [dim_customer] | [Table] | [DW] | [Data Architect] | 🟡 L2 | ✅ Active |
| 9 | [fact_requests] | [Table] | [DW] | [Data Architect] | 🟡 L2 | ✅ Active |

## 3. Catalog Entry Details

### customers

| Field | Value |
|-------|-------|
| [Name] | [customers] |
| [Type] | [Table] |
| [Location] | [PostgreSQL — public schema] |
| [Description] | [Customer master data] |
| [Owner] | [Customer Data Steward] |
| [Classification] | 🔴 L1 Restricted |
| [Records] | [~10,000] |
| [Size] | [~5 MB] |
| [Created] | [YYYY-MM-DD] |
| [Last Updated] | [YYYY-MM-DD] |
| [Columns] | [7] |
| [Quality Score] | [99.1%] |
| [Retention] | [7 years] |

### Column Details

| Column | Type | Nullable | Description | Classification |
|--------|------|---------|-----------|---------------|
| [id] | [UUID] | [No] | [Primary key] | 🟢 L3 |
| [name] | [VARCHAR(255)] | [No] | [Customer name] | 🔴 L1 |
| [email] | [VARCHAR(255)] | [No] | [Email address] | 🔴 L1 |
| [phone] | [VARCHAR(20)] | [Yes] | [Phone number] | 🔴 L1 |
| [type] | [VARCHAR(20)] | [No] | [Customer type] | 🟢 L3 |
| [created_at] | [TIMESTAMPTZ] | [No] | [Creation time] | 🟢 L3 |
| [updated_at] | [TIMESTAMPTZ] | [No] | [Last update] | 🟢 L3 |

## 4. Search & Discovery

| Feature | Implementation | Coverage |
|---------|---------------|---------|
| [Full-text search] | [Elasticsearch] | [All assets] |
| [Tag-based search] | [Tags on each asset] | [All assets] |
| [Classification filter] | [Filter by L1/L2/L3/L4] | [All assets] |
| [Owner filter] | [Filter by data steward] | [All assets] |
| [Quality filter] | [Filter by quality score] | [All assets] |

## 5. Catalog Governance

| Rule | Description |
|------|-----------|
| [Completeness] | [All data assets in catalog] |
| [Accuracy] | [Catalog reflects current state] |
| [Ownership] | [Every asset has an owner] |
| [Classification] | [Every asset classified] |
| [Documentation] | [Every asset has description] |

## 6. Catalog Metrics

| Metric | Target | Current |
|--------|--------|---------|
| [Assets cataloged] | [100%] | [X%] |
| [Assets with owner] | [100%] | [X%] |
| [Assets classified] | [100%] | [X%] |
| [Assets with description] | [100%] | [X%] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Metadata-Repository]] | Metadata storage |
| [[Metadata-Standards]] | Metadata standards |
| [[Data-Classification-Schema]] | Classification |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** If it's not in the catalog, it doesn't exist (officially). Catalog everything. Keep it current.
