---
document_type: Metadata Standards
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [metadata, standards, dmbok]
standard_ref:
  - DMBOK v2 — Metadata Management
---

# Metadata Standards

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines standards for metadata — what metadata to collect, how to format it, and how to maintain it.

## 2. Required Metadata

| Metadata Element | Required | Format | Example |
|-----------------|---------|--------|---------|
| [Business Name] | ✅ | [Title Case] | [Customer Email Address] |
| [Technical Name] | ✅ | [snake_case] | [customer_email] |
| [Description] | ✅ | [Sentence] | [The email address for customer communications] |
| [Data Type] | ✅ | [Standard type] | [VARCHAR(255)] |
| [Classification] | ✅ | [L1/L2/L3/L4] | [L1 Restricted] |
| [Owner] | ✅ | [Role name] | [Customer Data Steward] |
| [Source] | ✅ | [System name] | [Application DB] |
| [Retention] | ✅ | [Duration] | [7 years] |
| [Quality Rules] | ✅ | [Rule IDs] | [DQ-C01, DQ-C02] |
| [Valid Values] | 🟡 | [List or range] | [STANDARD, VIP, CORPORATE] |
| [Default Value] | 🟡 | [Value] | [STANDARD] |
| [Constraints] | 🟡 | [Constraint type] | [NOT NULL, UNIQUE] |

## 3. Metadata Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| [Tables] | [snake_case, plural] | [customers, requests] |
| [Columns] | [snake_case, singular] | [customer_id, created_at] |
| [Indexes] | [idx_table_column] | [idx_requests_status] |
| [Constraints] | [type_table_column] | [fk_requests_customers] |
| [Business names] | [Title Case, spaces] | [Customer Email Address] |

## 4. Metadata Quality

| Quality Check | Rule | Threshold |
|--------------|------|----------|
| [Completeness] | [All required fields populated] | [100%] |
| [Accuracy] | [Metadata matches actual data] | [99%] |
| [Timeliness] | [Updated within SLA] | [< 24 hours] |
| [Consistency] | [Same metadata across systems] | [99%] |

## 5. Metadata Maintenance

| Activity | Frequency | Owner | Method |
|---------|----------|-------|--------|
| [Schema sync] | [On change] | [Automated] | [Database introspection] |
| [Business metadata review] | [Quarterly] | [Data Steward] | [Manual review] |
| [Quality check] | [Monthly] | [Data Architect] | [Automated checks] |
| [Full audit] | [Annually] | [DGO] | [Manual audit] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Metadata-Repository]] | Metadata storage |
| [[Data-Catalog]] | Catalog using metadata |
| [[Data-Standards]] | Data standards |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Metadata standards ensure *consistency*. Without them, every team documents data differently.
