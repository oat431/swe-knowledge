---
document_type: Data Profiling Report
version: "1.0"
status: Active
author: "[Data Steward]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-profiling, data-quality, dmbok]
standard_ref:
  - DMBOK v2 — Data Quality
---

# Data Profiling Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Analyzes data structure, content, and quality — understanding what data exists and its condition.

## 2. Profiling Summary

| Table | Records | Columns | Nulls (%) | Duplicates (%) | Quality Score |
|-------|---------|---------|----------|---------------|--------------|
| [customers] | [10,000] | [7] | [2%] | [0.1%] | [99.1%] |
| [requests] | [100,000] | [10] | [1%] | [0%] | [99.5%] |
| [transactions] | [200,000] | [7] | [0.5%] | [0%] | [99.8%] |
| [categories] | [15] | [4] | [0%] | [0%] | [100%] |
| [statuses] | [6] | [3] | [0%] | [0%] | [100%] |

## 3. Customer Profiling

| Column | Type | Nulls | Distinct | Min | Max | Avg | Pattern |
|--------|------|-------|---------|-----|-----|-----|---------|
| [id] | [UUID] | [0%] | [10,000] | — | — | — | [UUID v4] |
| [name] | [VARCHAR] | [0%] | [9,800] | [1 char] | [100 chars] | [25 chars] | [Text] |
| [email] | [VARCHAR] | [0%] | [10,000] | [5 chars] | [50 chars] | [20 chars] | [Email] |
| [phone] | [VARCHAR] | [2%] | [9,500] | [10 digits] | [15 digits] | [12 digits] | [E.164] |
| [type] | [VARCHAR] | [0%] | [3] | — | — | — | [Enum] |
| [created_at] | [TIMESTAMP] | [0%] | — | [2025-01-01] | [2026-07-12] | — | [ISO 8601] |
| [updated_at] | [TIMESTAMP] | [0%] | — | [2025-01-01] | [2026-07-12] | — | [ISO 8601] |

## 4. Request Profiling

| Column | Type | Nulls | Distinct | Min | Max | Avg | Pattern |
|--------|------|-------|---------|-----|-----|-----|---------|
| [id] | [UUID] | [0%] | [100,000] | — | — | — | [UUID v4] |
| [customer_id] | [UUID] | [0%] | [10,000] | — | — | — | [FK] |
| [category_id] | [UUID] | [0%] | [15] | — | — | — | [FK] |
| [status_id] | [UUID] | [0%] | [6] | — | — | — | [FK] |
| [assigned_to] | [UUID] | [1%] | [25] | — | — | — | [FK] |
| [description] | [TEXT] | [0%] | — | [10 chars] | [5,000 chars] | [500 chars] | [Text] |
| [amount] | [DECIMAL] | [0%] | — | [1.00] | [50,000.00] | [5,000.00] | [Numeric] |
| [priority] | [VARCHAR] | [0%] | [4] | — | — | — | [Enum] |
| [submitted_at] | [TIMESTAMP] | [0%] | — | [2025-01-01] | [2026-07-12] | — | [ISO 8601] |
| [updated_at] | [TIMESTAMP] | [0%] | — | [2025-01-01] | [2026-07-12] | — | [ISO 8601] |

## 5. Data Quality Issues Found

| # | Issue | Table | Column | Count | Severity | Action |
|---|-------|-------|--------|-------|---------|--------|
| 1 | [Null phone numbers] | [customers] | [phone] | [200] | 🟢 Low | [Accept — optional field] |
| 2 | [Unassigned requests] | [requests] | [assigned_to] | [1,000] | 🟢 Low | [Assign on processing] |
| 3 | [Duplicate names] | [customers] | [name] | [200] | 🟢 Low | [Different people, same name] |

## 6. Profiling Tools

| Tool | Purpose | Configuration |
|------|---------|-------------|
| [Great Expectations] | [Automated profiling] | [expect_column_values_to_not_be_null] |
| [pandas-profiling] | [Statistical profiling] | [Default config] |
| [SQL queries] | [Custom profiling] | [Custom queries] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Quality-Strategy]] | Quality framework |
| [[Data-Quality-Rules]] | Quality rules |
| [[Data-Quality-Scorecard]] | Quality metrics |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Profile before you trust. You can't fix what you don't understand. Profile regularly.
