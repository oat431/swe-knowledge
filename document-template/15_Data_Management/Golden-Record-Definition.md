---
document_type: Golden Record Definition
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [golden-record, mdm, data-quality, dmbok]
standard_ref:
  - DMBOK v2 — Master Data Management
---

# Golden Record Definition

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines the canonical, authoritative record for each master data entity — the "golden record" that all systems trust.

## 2. Golden Record Principles

| # | Principle | Description |
|---|----------|-------------|
| 1 | [One source of truth] | [Each entity has one authoritative source] |
| 2 | [Complete] | [All required fields populated] |
| 3 | [Accurate] | [Data verified against source] |
| 4 | [Current] | [Updated within SLA] |
| 5 | [Unique] | [No duplicate records] |

## 3. Golden Record Definitions

### Customer Golden Record

| Field | Source | Rule | Quality Check |
|-------|--------|------|-------------|
| [id] | [Application DB] | [UUID, system-generated] | [Unique] |
| [name] | [Application DB] | [Latest value] | [Not null, 1-100 chars] |
| [email] | [Application DB] | [Verified email] | [Valid format, unique] |
| [phone] | [Application DB] | [E.164 format] | [Valid E.164] |
| [type] | [Application DB] | [Current type] | [Valid enum] |
| [address] | [Application DB] | [Latest address] | [Not null for L1] |
| [created_at] | [Application DB] | [Original creation] | [Not null] |
| [updated_at] | [Application DB] | [Latest update] | [Not null] |

### Request Golden Record

| Field | Source | Rule | Quality Check |
|-------|--------|------|-------------|
| [id] | [Application DB] | [UUID, system-generated] | [Unique] |
| [customer_id] | [Application DB] | [FK to customer] | [Referential integrity] |
| [category_id] | [Application DB] | [FK to category] | [Referential integrity] |
| [status_id] | [Application DB] | [FK to status] | [Referential integrity] |
| [description] | [Application DB] | [Original submission] | [Not empty] |
| [amount] | [Application DB] | [Original amount] | [> 0] |
| [submitted_at] | [Application DB] | [Submission time] | [Not null] |

## 4. Match & Merge Rules

### Customer Match Rules

| # | Rule | Confidence | Action |
|---|------|-----------|--------|
| 1 | [Email exact match] | [100%] | [Merge automatically] |
| 2 | [Name + phone match] | [95%] | [Merge with review] |
| 3 | [Name + address match] | [80%] | [Flag for review] |
| 4 | [Fuzzy name match] | [60%] | [Flag for review] |

### Customer Merge Rules

| Scenario | Winner | Rule |
|---------|--------|------|
| [Same customer, different names] | [Most recent] | [Latest update wins] |
| [Same customer, different phones] | [Both] | [Keep all phones] |
| [Same customer, different addresses] | [Most recent] | [Latest address wins] |
| [Same customer, different types] | [Higher type] | [CORPORATE > VIP > STANDARD] |

## 5. Survivorship Rules

| Field | Rule | Rationale |
|-------|------|----------|
| [name] | [Most recent non-null] | [Names can change] |
| [email] | [Most recent verified] | [Emails can change] |
| [phone] | [Most recent E.164] | [Phones can change] |
| [type] | [Highest value] | [Upgrade path] |
| [created_at] | [Earliest] | [Original creation] |

## 6. Golden Record Quality

| Metric | Definition | Target | Current |
|--------|-----------|--------|---------|
| [Completeness] | [Required fields populated] | [100%] | [X%] |
| [Accuracy] | [Verified against source] | [99%] | [X%] |
| [Uniqueness] | [No duplicates] | [100%] | [X%] |
| [Freshness] | [Updated within SLA] | [< 24h] | [X hours] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[MDM-Strategy]] | MDM strategy |
| [[Reference-Data-Catalog]] | Reference data |
| [[Data-Quality-Rules]] | Quality rules |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** The golden record is *the truth*. Every system syncs to it. If the golden record is wrong, everything is wrong.
