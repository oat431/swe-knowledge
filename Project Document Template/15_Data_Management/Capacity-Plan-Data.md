---
document_type: Capacity Plan (Data)
version: "1.0"
status: Draft
author: "[DBA]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [capacity-planning, data-growth, dmbok]
standard_ref:
  - DMBOK v2 — Database Operations
---

# Capacity Plan (Data)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Projects data growth and plans storage, compute, and performance capacity — preventing surprises.

## 2. Current Capacity

| Resource | Current | Utilization | Status |
|---------|---------|------------|--------|
| [Database storage] | [X GB] | [X%] | 🟢🟡🔴 |
| [Cache storage] | [X GB] | [X%] | 🟢🟡🔴 |
| [File storage] | [X GB] | [X%] | 🟢🟡🔴 |
| [Connections] | [X / 100] | [X%] | 🟢🟡🔴 |
| [IOPS] | [X] | [X%] | 🟢🟡🔴 |

## 3. Growth Projections

| Month | Requests | Customers | DB Size | Cache Size | File Size |
|-------|---------|----------|---------|-----------|----------|
| [Current] | [X] | [X] | [X GB] | [X GB] | [X GB] |
| [Month 6] | [X] | [X] | [X GB] | [X GB] | [X GB] |
| [Month 12] | [X] | [X] | [X GB] | [X GB] | [X GB] |
| [Month 18] | [X] | [X] | [X GB] | [X GB] | [X GB] |
| [Month 24] | [X] | [X] | [X GB] | [X GB] | [X GB] |

## 4. Growth Rate

| Metric | Monthly Growth | Annual Growth | Trend |
|--------|---------------|--------------|-------|
| [Requests] | [X%] | [X%] | ↑ |
| [Customers] | [X%] | [X%] | ↑ |
| [Database] | [X GB] | [X GB] | ↑ |
| [Files] | [X GB] | [X GB] | ↑ |

## 5. Capacity Alerts

| Resource | Warning | Critical | Action |
|---------|---------|---------|--------|
| [Database storage] | [70%] | [85%] | [Scale up / Archive] |
| [Cache storage] | [70%] | [85%] | [Scale up] |
| [File storage] | [70%] | [85%] | [Archive old files] |
| [Connections] | [70%] | [85%] | [Scale connection pool] |

## 6. Scaling Plan

| Trigger | Action | Timeline | Cost |
|---------|--------|---------|------|
| [DB storage > 70%] | [Increase storage] | [1 day] | [$X/month] |
| [DB storage > 85%] | [Archive old data] | [1 week] | [One-time $X] |
| [Connections > 70%] | [Increase pool size] | [1 hour] | [None] |
| [IOPS > 70%] | [Upgrade instance] | [1 day] | [$X/month] |

## 7. Archival Strategy

| Data Type | Active Period | Archive After | Archive Location | Retention |
|----------|-------------|--------------|-----------------|----------|
| [Requests] | [Current year] | [> 2 years] | [S3 Glacier] | [7 years] |
| [Transactions] | [Current year] | [> 2 years] | [S3 Glacier] | [7 years] |
| [Audit logs] | [Current year] | [> 1 year] | [S3 Standard] | [3 years] |
| [Documents] | [Current year] | [> 3 years] | [S3 Glacier] | [7 years] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Database-Operational-Runbook]] | Operational procedures |
| [[Data-Retention-Archival-Policy]] | Retention rules |
| [[Capacity-Plan]] | Infrastructure capacity |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Capacity planning prevents *surprises*. If you hit 85% storage without a plan, you're in crisis mode.
