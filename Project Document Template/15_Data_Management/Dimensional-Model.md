---
document_type: Dimensional Model (Star/Snowflake)
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [dimensional-model, star-schema, data-warehouse, dmbok]
standard_ref:
  - DMBOK v2 — Data Modeling
---

# Dimensional Model (Star/Snowflake)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines the dimensional model for analytics and reporting — fact tables and dimension tables optimized for queries.

## 2. Star Schema

```mermaid
erDiagram
    FACT_REQUESTS ||--|| DIM_DATE : submitted_date
    FACT_REQUESTS ||--|| DIM_CUSTOMER : customer
    FACT_REQUESTS ||--|| DIM_CATEGORY : category
    FACT_REQUESTS ||--|| DIM_STATUS : status
    FACT_REQUESTS ||--|| DIM_STAFF : assigned_staff

    FACT_REQUESTS {
        uuid request_id PK
        uuid date_key FK
        uuid customer_key FK
        uuid category_key FK
        uuid status_key FK
        uuid staff_key FK
        decimal amount
        int processing_days
        int sla_breach
    }

    DIM_DATE {
        uuid date_key PK
        date full_date
        int year
        int quarter
        int month
        int week
        int day_of_week
        string month_name
        string day_name
        boolean is_weekend
    }

    DIM_CUSTOMER {
        uuid customer_key PK
        string customer_id
        string name
        string type
        string segment
    }

    DIM_CATEGORY {
        uuid category_key PK
        string category_id
        string name
        string group
    }

    DIM_STATUS {
        uuid status_key PK
        string status_id
        string name
        string stage
    }

    DIM_STAFF {
        uuid staff_key PK
        string staff_id
        string name
        string role
        string department
    }
```

## 3. Fact Table

### fact_requests

| Column | Type | Description | Aggregation |
|--------|------|-----------|------------|
| [request_id] | [UUID] | [Degenerate dimension] | — |
| [date_key] | [UUID] | [FK → dim_date] | — |
| [customer_key] | [UUID] | [FK → dim_customer] | — |
| [category_key] | [UUID] | [FK → dim_category] | — |
| [status_key] | [UUID] | [FK → dim_status] | — |
| [staff_key] | [UUID] | [FK → dim_staff] | — |
| [amount] | [DECIMAL] | [Request amount] | [SUM, AVG] |
| [processing_days] | [INT] | [Days to complete] | [AVG] |
| [sla_breach] | [INT] | [1 if SLA breached] | [SUM, COUNT] |

**Grain:** One row per request.

## 4. Dimension Tables

### dim_date

| Column | Type | Description |
|--------|------|-----------|
| [date_key] | [UUID] | [Primary key] |
| [full_date] | [DATE] | [Calendar date] |
| [year] | [INT] | [Year (2026)] |
| [quarter] | [INT] | [Quarter (1-4)] |
| [month] | [INT] | [Month (1-12)] |
| [week] | [INT] | [ISO week number] |
| [day_of_week] | [INT] | [Day (1=Mon, 7=Sun)] |
| [month_name] | [VARCHAR] | [January, February, ...] |
| [day_name] | [VARCHAR] | [Monday, Tuesday, ...] |
| [is_weekend] | [BOOLEAN] | [True if Sat/Sun] |

### dim_customer

| Column | Type | Description |
|--------|------|-----------|
| [customer_key] | [UUID] | [Surrogate key] |
| [customer_id] | [VARCHAR] | [Business key] |
| [name] | [VARCHAR] | [Customer name] |
| [type] | [VARCHAR] | [Customer type] |
| [segment] | [VARCHAR] | [Customer segment] |

## 5. Sample Queries

```sql
-- Requests by month and category
SELECT
    d.month_name,
    c.name AS category,
    COUNT(*) AS request_count,
    SUM(f.amount) AS total_amount,
    AVG(f.processing_days) AS avg_processing_days
FROM fact_requests f
JOIN dim_date d ON f.date_key = d.date_key
JOIN dim_category c ON f.category_key = c.category_key
WHERE d.year = 2026
GROUP BY d.month_name, c.name
ORDER BY d.month, total_amount DESC;

-- SLA breach rate by staff
SELECT
    s.name AS staff,
    COUNT(*) AS total_requests,
    SUM(f.sla_breach) AS breaches,
    ROUND(SUM(f.sla_breach)::NUMERIC / COUNT(*) * 100, 2) AS breach_rate
FROM fact_requests f
JOIN dim_staff s ON f.staff_key = s.staff_key
GROUP BY s.name
ORDER BY breach_rate DESC;
```

## 6. ETL Process

| Step | Source | Target | Transformation |
|------|--------|--------|---------------|
| 1 | [requests] | [dim_date] | [Generate date records] |
| 2 | [customers] | [dim_customer] | [SCD Type 2] |
| 3 | [categories] | [dim_category] | [Direct mapping] |
| 4 | [requests] | [fact_requests] | [Calculate measures] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Physical-Data-Model-PDM]] | Source model |
| [[Data-Warehouse-Architecture]] | DW architecture |
| [[ETL-ELT-Specification]] | ETL process |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Dimensional models are *query-optimized*. Star schema for simplicity, snowflake for storage efficiency.
