---
document_type: SE Performance Dashboard
version: "1.0"
status: Active
author: "[Systems Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [se-performance, dashboard, metrics, sebok]
standard_ref:
  - SEBoK v2 — Systems Engineering Management
---

# SE Performance Dashboard

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Centralized view of systems engineering performance — tracking technical health, process effectiveness, and team productivity.

## 2. SE KPIs

| KPI | Definition | Target | Current | Status |
|-----|-----------|--------|---------|--------|
| [Requirements Coverage] | [Requirements with tests / Total] | [100%] | [98%] | ✅ |
| [Requirements Volatility] | [Changes / Total] | [< 10%] | [5%] | ✅ |
| [Defect Density] | [Defects / KLOC] | [< 5] | [3] | ✅ |
| [Test Coverage] | [Code coverage] | [≥ 80%] | [85%] | ✅ |
| [Risk Mitigation Rate] | [Mitigated / Total] | [≥ 80%] | [80%] | ✅ |
| [Review Effectiveness] | [Issues found in review / Total] | [≥ 50%] | [60%] | ✅ |
| [Technical Debt] | [Debt items open] | [< 10] | [5] | ✅ |
| [Decision Velocity] | [Avg days to decide] | [< 7 days] | [3 days] | ✅ |

## 3. Dashboard Layout

```
┌─────────────────────────────────────────────────────────────┐
│  SE PERFORMANCE DASHBOARD                       [Last 30d ▼]│
├──────────┬──────────┬──────────┬──────────────────────────┤
│ Req      │ Defect   │ Test     │ Risk                     │
│ Coverage │ Density  │ Coverage │ Mitigation               │
│ 98%      │ 3/KLOC   │ 85%      │ 80%                      │
├──────────┴──────────┴──────────┴──────────────────────────┤
│  [Requirements Trend — Line]      [Defect Trend — Line]    │
├─────────────────────────────────────────────────────────────┤
│  [Risk Burn-Down — Line]          [TPM Status — Gauge]     │
├──────────────────────────────┬──────────────────────────────┤
│  [Technical Debt — Bar]      │  [Review Stats — Pie]       │
└──────────────────────────────┴──────────────────────────────┘
```

## 4. Requirements Status

| Status | Count | Percentage |
|--------|-------|-----------|
| [Implemented] | [98] | [98%] |
| [In Progress] | [2] | [2%] |
| [Not Started] | [0] | [0%] |
| **Total** | **[100]** | **100%** |

## 5. Defect Trend

| Month | Opened | Closed | Open | Trend |
|-------|--------|--------|------|-------|
| [Month 1] | [15] | [10] | [5] | — |
| [Month 2] | [10] | [12] | [3] | ↓ |
| [Month 3] | [5] | [6] | [2] | ↓ |
| [Current] | [3] | [4] | [1] | ↓ |

## 6. SE Health Score

| Category | Score | Weight | Weighted Score |
|---------|-------|--------|---------------|
| [Requirements] | [98%] | [25%] | [24.5%] |
| [Quality] | [90%] | [25%] | [22.5%] |
| [Risk] | [80%] | [25%] | [20%] |
| [Performance] | [95%] | [25%] | [23.75%] |
| **Overall** | | | **[90.75%]** |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Measurement-Plan]] | Measurement framework |
| [[Technical-Performance-Measures]] | Technical measures |
| [[Quality-Metrics-Dashboard]] | Quality metrics |

---

> **Template Standard:** Based on SEBoK v2
> **Usage:** The dashboard is the *SE health check*. Review weekly. If health score drops below 85%, investigate.
