---
document_type: Maintenance Metrics Dashboard
version: "1.0"
status: Active
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [maintenance-metrics, dashboard, swebok]
standard_ref:
  - SWEBOK v4 — Maintenance
---

# Maintenance Metrics Dashboard

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Tracks maintenance health — defect trends, resolution times, backlog, and team productivity.

## 2. Maintenance KPIs

| KPI | Definition | Target | Current | Status |
|-----|-----------|--------|---------|--------|
| [MTTR] | [Mean time to resolution] | [< 1 hour] | [X min] | 🟢🟡🔴 |
| [Defect Density] | [Defects per feature] | [< 2] | [X] | 🟢🟡🔴 |
| [Backlog Size] | [Open MRs] | [< 20] | [X] | 🟢🟡🔴 |
| [Resolution Rate] | [MRs closed per week] | [≥ 5] | [X] | 🟢🟡🔴 |
| [Reopen Rate] | [Reopened MRs / Total] | [< 10%] | [X%] | 🟢🟡🔴 |
| [SLA Compliance] | [Within SLA / Total] | [≥ 95%] | [X%] | 🟢🟡🔴 |
| [Technical Debt] | [Open debt items] | [< 10] | [X] | 🟢🟡🔴 |

## 3. Defect Trends

| Month | Found | Fixed | Remaining | Trend |
|-------|-------|-------|----------|-------|
| [Month 1] | [12] | [10] | [2] | — |
| [Month 2] | [8] | [9] | [1] | ↓ |
| [Month 3] | [5] | [6] | [0] | ↓ |
| [Month 4] | [6] | [5] | [1] | → |
| **Current** | **[X]** | **[X]** | **[X]** | **↑↓→** |

## 4. Resolution Time Distribution

| Severity | < 1h | 1-4h | 4-8h | 8-24h | > 24h |
|---------|------|------|------|-------|-------|
| 🔴 Critical | [X] | [X] | [X] | [X] | [X] |
| 🟡 High | [X] | [X] | [X] | [X] | [X] |
| 🟢 Medium | [X] | [X] | [X] | [X] | [X] |
| ⚪ Low | [X] | [X] | [X] | [X] | [X] |

## 5. Maintenance Velocity

| Sprint | Planned | Completed | Carried Over | Velocity |
|--------|---------|----------|-------------|---------|
| [Sprint 1] | [10] | [8] | [2] | [8] |
| [Sprint 2] | [10] | [9] | [1] | [9] |
| [Sprint 3] | [8] | [8] | [0] | [8] |
| [Sprint 4] | [10] | [10] | [0] | [10] |

## 6. Dashboard Layout

```
┌─────────────────────────────────────────────────────────────┐
│  MAINTENANCE DASHBOARD                          [Last 30d ▼]│
├──────────┬──────────┬──────────┬──────────────────────────┤
│ MTTR     │ Backlog  │ Resolution│ SLA Compliance           │
│ 45 min   │ 12       │ 8/week   │ 98%                      │
├──────────┴──────────┴──────────┴──────────────────────────┤
│  [Defect Trends — Line Chart]    [Resolution Time — Bar]   │
├─────────────────────────────────────────────────────────────┤
│  [Backlog Trend — Area Chart]    [Velocity — Bar Chart]    │
└─────────────────────────────────────────────────────────────┘
```

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Maintenance-Plan]] | Maintenance strategy |
| [[SLA-Compliance-Report]] | SLA tracking |
| [[Technical-Debt-Register]] | Technical debt |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Review maintenance metrics weekly. If MTTR is rising or backlog is growing, investigate immediately.
