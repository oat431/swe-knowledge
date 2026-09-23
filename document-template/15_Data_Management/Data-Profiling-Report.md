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
| [table] | [N] | [N] | [X]% | [X]% | [X]% |

## 3. [Table] Profiling

| Column | Type | Nulls | Distinct | Min | Max | Avg | Pattern |
|--------|------|-------|---------|-----|-----|-----|---------|
| [column] | [TYPE] | [X]% | [N] | [min] | [max] | [avg] | [Pattern] |

## 4. [Table] Profiling

| Column | Type | Nulls | Distinct | Min | Max | Avg | Pattern |
|--------|------|-------|---------|-----|-----|-----|---------|
| [column] | [TYPE] | [X]% | [N] | [min] | [max] | [avg] | [Pattern] |

> **Repeat sections 3–4 for each profiled table.**

## 5. Data Quality Issues Found

| # | Issue | Table | Column | Count | Severity | Action |
|---|-------|-------|--------|-------|---------|--------|
| 1 | [Issue description] | [table] | [column] | [N] | [🟢 Low / 🟡 Medium / 🔴 High] | [Action] |

## 6. Profiling Tools

| Tool | Purpose | Configuration |
|------|---------|-------------|
| [Great Expectations] | [Automated profiling] | [Configuration] |
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