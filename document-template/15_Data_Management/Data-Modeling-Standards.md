---
document_type: Data Modeling Standards
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-modeling, standards, dmbok]
standard_ref:
  - DMBOK v2 — Data Modeling
---

# Data Modeling Standards

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines conventions and standards for data modeling — ensuring consistency across all models.

## 2. Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| [Tables] | [snake_case, plural] | [customers, requests] |
| [Columns] | [snake_case, singular] | [customer_id, created_at] |
| [Primary keys] | [id] | [id] |
| [Foreign keys] | [entity_id] | [customer_id] |
| [Indexes] | [idx_table_column] | [idx_requests_status] |
| [Unique constraints] | [uq_table_column] | [uq_customers_email] |
| [Check constraints] | [ck_table_condition] | [ck_requests_amount_positive] |
| [Foreign keys] | [fk_table_referenced] | [fk_requests_customers] |

## 3. Modeling Rules

| # | Rule | Description |
|---|------|-----------|
| 1 | [Every table has UUID primary key] | [Use gen_random_uuid()] |
| 2 | [Every table has created_at and updated_at] | [TIMESTAMPTZ with defaults] |
| 3 | [Use ENUMs for fixed value sets] | [CREATE TYPE or VARCHAR + CHECK] |
| 4 | [Normalize to 3NF minimum] | [No redundancy in OLTP] |
| 5 | [Denormalize for analytics] | [Star schema for DW] |
| 6 | [Index foreign keys] | [For JOIN performance] |
| 7 | [Use soft deletes] | [deleted_at column, not DROP] |
| 8 | [Document all columns] | [COMMENT ON COLUMN] |

## 4. Data Type Standards

| Use Case | Data Type | Example |
|---------|----------|---------|
| [Identifiers] | [UUID] | [customer_id UUID] |
| [Names] | [VARCHAR(255)] | [name VARCHAR(255)] |
| [Emails] | [VARCHAR(255)] | [email VARCHAR(255)] |
| [Phones] | [VARCHAR(20)] | [phone VARCHAR(20)] |
| [Descriptions] | [TEXT] | [description TEXT] |
| [Amounts] | [DECIMAL(12,2)] | [amount DECIMAL(12,2)] |
| [Booleans] | [BOOLEAN] | [is_active BOOLEAN] |
| [Timestamps] | [TIMESTAMPTZ] | [created_at TIMESTAMPTZ] |
| [Dates] | [DATE] | [submitted_at DATE] |
| [Integers] | [INT] or [BIGINT] | [sort_order INT] |

## 5. Model Review Checklist

| # | Check | Status |
|---|-------|--------|
| 1 | [Naming conventions followed] | ☐ |
| 2 | [All tables have UUID PK] | ☐ |
| 3 | [All tables have timestamps] | ☐ |
| 4 | [Foreign keys indexed] | ☐ |
| 5 | [Constraints defined] | ☐ |
| 6 | [Normalization correct] | ☐ |
| 7 | [Data types appropriate] | ☐ |
| 8 | [Comments added] | ☐ |

## 6. Modeling Tools

| Tool | Purpose | License |
|------|---------|--------|
| [dbdiagram.io] | [ER diagram design] | [Free] |
| [pgModeler] | [PostgreSQL modeling] | [Open source] |
| [dbt] | [Transformation models] | [Open source] |
| [SchemaSpy] | [Documentation generation] | [Open source] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Standards]] | Overall data standards |
| [[Data-Model-Review-Records]] | Review evidence |
| [[Physical-Data-Model-PDM]] | Model implementation |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Standards ensure *consistency*. Without them, every model looks different. Review every model against these standards.
