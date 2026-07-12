---
document_type: Quality Metrics Dashboard
version: "1.0"
status: Active
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [quality-metrics, dashboard, swebok]
standard_ref:
  - SWEBOK v4 — Quality Assurance
---

# Quality Metrics Dashboard

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Centralized view of quality metrics — defects, testing, code quality, and process compliance.

## 2. Quality KPIs

| KPI | Definition | Target | Current | Status |
|-----|-----------|--------|---------|--------|
| [Defect Density] | [Defects / Feature] | [< 2] | [X] | 🟢🟡🔴 |
| [Test Coverage] | [Code coverage %] | [≥ 80%] | [X%] | 🟢🟡🔴 |
| [Test Pass Rate] | [Tests passed / Total] | [≥ 95%] | [X%] | 🟢🟡🔴 |
| [Defect Leakage] | [Prod defects / Total] | [< 5%] | [X%] | 🟢🟡🔴 |
| [MTTR] | [Mean time to resolution] | [< 1 day] | [X hours] | 🟢🟡🔴 |
| [Code Review Coverage] | [PRs reviewed / Total] | [100%] | [X%] | 🟢🟡🔴 |
| [Static Analysis] | [Linting errors] | [0] | [X] | 🟢🟡🔴 |
| [Requirements Coverage] | [Reqs tested / Total] | [100%] | [X%] | 🟢🟡🔴 |

## 3. Quality Dashboard Layout

```
┌─────────────────────────────────────────────────────────────┐
│  QUALITY DASHBOARD                              [Last 30d ▼]│
├──────────┬──────────┬──────────┬──────────────────────────┤
│ Defect   │ Test     │ Test     │ Defect                   │
│ Density  │ Coverage │ Pass Rate│ Leakage                  │
│ 1.5      │ 85%      │ 96%      │ 3%                       │
├──────────┴──────────┴──────────┴──────────────────────────┤
│  [Defect Trends — Line Chart]    [Test Coverage — Bar]    │
├─────────────────────────────────────────────────────────────┤
│  [Defects by Module — Pie]       [Quality Trend — Line]   │
└─────────────────────────────────────────────────────────────┘
```

## 4. Quality Trends

| Month | Defect Density | Coverage | Pass Rate | Leakage | Overall |
|-------|---------------|---------|----------|---------|---------|
| [Month 1] | [2.5] | [72%] | [90%] | [5%] | 🟡 |
| [Month 2] | [2.0] | [78%] | [93%] | [3%] | 🟡 |
| [Month 3] | [1.5] | [83%] | [95%] | [2%] | 🟢 |
| [Month 4] | [1.2] | [85%] | [96%] | [1%] | 🟢 |
| **Current** | **[X]** | **[X%]** | **[X%]** | **[X%]** | **🟢🟡🔴** |

## 5. Quality Alerts

| Alert | Condition | Status | Action |
|-------|----------|--------|--------|
| [Defect density > 2] | [Defects/feature > 2] | 🟢🟡🔴 | [Investigate module] |
| [Coverage < 80%] | [Code coverage < 80%] | 🟢🟡🔴 | [Add tests] |
| [Leakage > 5%] | [Prod defects > 5%] | 🟢🟡🔴 | [Shift-left testing] |
| [MTTR > 1 day] | [Resolution time > 1 day] | 🟢🟡🔴 | [Escalate] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Defect-Log-Metrics]] | Defect details |
| [[SQAP]] | Quality assurance plan |
| [[Test-Report]] | Test results |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Quality metrics answer "Is our quality improving?" Trends matter more than snapshots.
