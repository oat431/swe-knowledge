---
document_type: Reference Data Catalog
version: "1.0"
status: Active
author: "[Data Steward]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [reference-data, catalog, dmbok]
standard_ref:
  - DMBOK v2 — Master Data Management
---

# Reference Data Catalog

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Catalogs all reference data — codes, categories, lookups, and enumerations used across the system.

## 2. Reference Data Inventory

| # | Reference Data | Table | Records | Owner | Last Updated |
|---|---------------|-------|---------|-------|------------|
| 1 | [Customer Types] | [customer_types] | [3] | [Customer Steward] | [YYYY-MM-DD] |
| 2 | [Request Categories] | [categories] | [15] | [Operations Steward] | [YYYY-MM-DD] |
| 3 | [Request Statuses] | [statuses] | [6] | [Operations Steward] | [YYYY-MM-DD] |
| 4 | [Priority Levels] | [priorities] | [4] | [Operations Steward] | [YYYY-MM-DD] |
| 5 | [Payment Methods] | [payment_methods] | [5] | [Financial Steward] | [YYYY-MM-DD] |
| 6 | [Currencies] | [currencies] | [3] | [Financial Steward] | [YYYY-MM-DD] |
| 7 | [Document Types] | [document_types] | [8] | [Operations Steward] | [YYYY-MM-DD] |
| 8 | [Countries] | [countries] | [195] | [Reference Steward] | [YYYY-MM-DD] |

## 3. Reference Data Details

### Customer Types

| Code | Name | Description | Sort Order | Active |
|------|------|-----------|----------|--------|
| [STANDARD] | [Standard] | [Regular customer] | [1] | ✅ |
| [VIP] | [VIP] | [High-value customer] | [2] | ✅ |
| [CORPORATE] | [Corporate] | [Business customer] | [3] | ✅ |

### Request Statuses

| Code | Name | Description | Sort Order | Active |
|------|------|-----------|----------|--------|
| [PENDING] | [Pending] | [Awaiting processing] | [1] | ✅ |
| [IN_PROGRESS] | [In Progress] | [Being processed] | [2] | ✅ |
| [APPROVED] | [Approved] | [Request approved] | [3] | ✅ |
| [REJECTED] | [Rejected] | [Request rejected] | [4] | ✅ |
| [COMPLETED] | [Completed] | [Request completed] | [5] | ✅ |
| [CANCELLED] | [Cancelled] | [Request cancelled] | [6] | ✅ |

### Priority Levels

| Code | Name | Description | SLA (Hours) | Sort Order |
|------|------|-----------|-----------|----------|
| [LOW] | [Low] | [Low priority] | [72] | [1] |
| [NORMAL] | [Normal] | [Standard priority] | [48] | [2] |
| [HIGH] | [High] | [High priority] | [24] | [3] |
| [URGENT] | [Urgent] | [Urgent priority] | [4] | [4] |

## 4. Reference Data Governance

| Rule | Description |
|------|-----------|
| [Change control] | [All changes via change request] |
| [Approval] | [Data Steward approves changes] |
| [Versioning] | [All changes logged with timestamp] |
| [No deletion] | [Deactivate instead of delete] |
| [Documentation] | [Every code has name + description] |

## 5. Reference Data Change Log

| Date | Reference Data | Change | Changed By | Approved By |
|------|---------------|--------|-----------|------------|
| [YYYY-MM-DD] | [Request Categories] | [Added CORPORATE category] | [Data Steward] | [DGO] |
| [YYYY-MM-DD] | [Priority Levels] | [Added URGENT level] | [Data Steward] | [DGO] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[MDM-Strategy]] | Master data strategy |
| [[Golden-Record-Definition]] | Golden record rules |
| [[Data-Standards]] | Data standards |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Reference data is the *vocabulary*. If codes are wrong, everything downstream is wrong. Govern carefully.
