---
document_type: Data Model Scorecard
version: "1.0"
status: Active
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-model, scorecard, quality, dmbok]
standard_ref:
  - DMBOK v2 — Data Modeling
---

# Data Model Scorecard

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Evaluates data model quality — completeness, correctness, standards compliance, and documentation.

## 2. Model Scorecard

| Model | Completeness | Correctness | Standards | Documentation | Overall |
|-------|-------------|------------|----------|--------------|--------|
| [CDM] | [95%] | [100%] | [100%] | [90%] | [96%] |
| [LDM] | [100%] | [100%] | [100%] | [95%] | [99%] |
| [PDM] | [100%] | [100%] | [100%] | [100%] | [100%] |
| [Star Schema] | [95%] | [100%] | [100%] | [90%] | [96%] |

## 3. Quality Dimensions

| Dimension | Definition | Weight | Scoring |
|----------|-----------|--------|---------|
| [Completeness] | [All entities/attributes defined] | [25%] | [% of required elements present] |
| [Correctness] | [Model is logically correct] | [25%] | [% of correct relationships, constraints] |
| [Standards Compliance] | [Follows naming/typing standards] | [25%] | [% compliance with [[Data-Modeling-Standards]]] |
| [Documentation] | [All elements documented] | [25%] | [% of elements with descriptions] |

## 4. Detailed Scoring

### CDM Scorecard

| Criterion | Score | Evidence |
|----------|-------|---------|
| [All business entities defined] | [100%] | [9 entities] |
| [All relationships defined] | [100%] | [8 relationships] |
| [Business rules documented] | [90%] | [5 rules, 1 pending] |
| [Naming conventions followed] | [100%] | [All entities follow convention] |
| **Overall** | **[96%]** | |

### LDM Scorecard

| Criterion | Score | Evidence |
|----------|-------|---------|
| [All attributes defined] | [100%] | [All attributes with type/nullable] |
| [All constraints defined] | [100%] | [PK, FK, UNIQUE, CHECK] |
| [Normalization verified] | [100%] | [3NF confirmed] |
| [Documentation complete] | [95%] | [Most attributes documented] |
| **Overall** | **[99%]** | |

### PDM Scorecard

| Criterion | Score | Evidence |
|----------|-------|---------|
| [All tables implemented] | [100%] | [All tables in PostgreSQL] |
| [All indexes created] | [100%] | [All FK indexed] |
| [All constraints enforced] | [100%] | [DB constraints active] |
| [DDL documented] | [100%] | [All DDL in version control] |
| **Overall** | **[100%]** | |

## 5. Issues Found

| # | Model | Issue | Severity | Status |
|---|-------|-------|---------|--------|
| 1 | [CDM] | [Business rule for payment pending] | 🟢 Low | ⬜ Open |
| 2 | [LDM] | [Description for sort_order missing] | 🟢 Low | ⬜ Open |
| 3 | [Star Schema] | [dim_staff segment attribute pending] | 🟢 Low | ⬜ Open |

## 6. Improvement Actions

| # | Action | Owner | Due Date | Status |
|---|--------|-------|---------|--------|
| 1 | [Document CDM business rule for payment] | [BA] | [YYYY-MM-DD] | ⬜ |
| 2 | [Add LDM description for sort_order] | [Data Architect] | [YYYY-MM-DD] | ⬜ |
| 3 | [Add Star Schema segment attribute] | [Data Architect] | [YYYY-MM-DD] | ⬜ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Model-Review-Records]] | Review evidence |
| [[Data-Modeling-Standards]] | Standards being scored |
| [[Conceptual-Data-Model-CDM]] | Models being scored |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Score every model. Track trends. If scores drop, investigate why.
