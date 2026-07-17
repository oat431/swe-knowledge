---
document_type: Cost Forecasts
version: "1.0"
status: Active
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
finance_owner: "[Finance Analyst]"
classification: "Internal / Confidential"
tags: [cost-forecasts, eac, budget-forecast, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Monitoring & Controlling (Cost Management)
  - ISO 21502 — Project Management Guidance
---

# Cost Forecasts

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document provides cost forecasts — predicted future costs based on current performance trends. It answers: "At current rates, what will the project actually cost?"

## 2. Forecast Summary

| Field | Detail |
|-------|--------|
| [Budget at Completion (BAC)] | $[X] |
| [Estimate at Completion (EAC)] | $[Y] |
| [Variance at Completion (VAC)] | $[BAC - EAC] |
| [Estimate to Complete (ETC)] | $[EAC - AC] |
| [To-Complete Performance Index (TCPI)] | [X.XX] |
| [Forecast Confidence] | [High / Medium / Low] |
| [Forecast Date] | [YYYY-MM-DD] |

## 3. Forecast Methods

| Method | Formula | When to Use | Current EAC |
|--------|---------|------------|------------|
| **EAC = BAC / CPI** | [BAC ÷ CPI] | [Typical variances — most common] | $[X] |
| **EAC = AC + (BAC - EV)** | [Actual + Remaining Budget] | [Atypical variance — one-time] | $[X] |
| **EAC = AC + (BAC - EV) / (CPI × SPI)** | [Actual + Remaining / (CPI × SPI)] | [Both schedule and cost impact] | $[X] |
| **EAC = AC + Bottom-Up ETC** | [Actual + New estimate for remaining] | [Original estimates flawed] | $[X] |

### 3.1 Recommended EAC

| Method | EAC | VAC | Rationale |
|--------|-----|-----|----------|
| [BAC / CPI] | $[X] | $[Y] | [Most likely — typical variance pattern] |
| [AC + Bottom-Up ETC] | $[X] | $[Y] | [Most accurate — re-estimated remaining] |
| **Recommended** | **$[X]** | **$[Y]** | [Used for reporting] |

## 4. Cost Forecast by Category

| Category | BAC | AC (Actual) | EAC (Forecast) | VAC | ETC | Status |
|----------|-----|------------|----------------|-----|-----|--------|
| [Personnel] | $[X] | $[Y] | $[Z] | $[W] | $[V] | 🟢🟡🔴 |
| [Software/Licensing] | $[X] | $[Y] | $[Z] | $[W] | $[V] | 🟢🟡🔴 |
| [Infrastructure] | $[X] | $[Y] | $[Z] | $[W] | $[V] | 🟢🟡🔴 |
| [Vendor Services] | $[X] | $[Y] | $[Z] | $[W] | $[V] | 🟢🟡🔴 |
| [Training] | $[X] | $[Y] | $[Z] | $[W] | $[V] | 🟢🟡🔴 |
| [Contingency] | $[X] | $[Y] | $[Z] | $[W] | $[V] | 🟢🟡🔴 |
| **Total** | **$[BAC]** | **$[AC]** | **$[EAC]** | **$[VAC]** | **$[ETC]** | **🟢🟡🔴** |

## 5. Monthly Cost Forecast

| Month | Planned Spend | Actual Spend | Forecast Spend | Cumulative Actual | Cumulative Forecast | Variance |
|-------|-------------|-------------|---------------|------------------|-------------------|---------|
| [Month 1] | $[X] | $[Y] | $[X] | $[Y] | $[Z] | $[W] |
| [Month 2] | $[X] | $[Y] | $[X] | $[Y] | $[Z] | $[W] |
| [Month 3] | $[X] | $[Y] | $[X] | $[Y] | $[Z] | $[W] |
| [Month 4] | $[X] | $[Y] | $[X] | $[Y] | $[Z] | $[W] |
| [Month 5] | $[X] | | $[X] | | $[Z] | $[W] |
| [Month 6] | $[X] | | $[X] | | $[Z] | $[W] |
| [Month 7] | $[X] | | $[X] | | $[Z] | $[W] |
| **Total** | **$[BAC]** | **$[AC]** | **$[EAC]** | | | **$[VAC]** |

## 6. Forecast Trend

| Period | EAC | VAC | TCPI | Trend | Confidence |
|--------|-----|-----|------|-------|-----------|
| [Month 1] | $[X] | $[Y] | [Z] | — | Low |
| [Month 2] | $[X] | $[Y] | [Z] | → | Medium |
| [Month 3] | $[X] | $[Y] | [Z] | ↓ Improving | Medium |
| [Month 4] | $[X] | $[Y] | [Z] | → Stable | Medium |
| **Current** | **$[X]** | **$[Y]** | **[Z]** | **→ Stable** | **Medium** |

## 7. To-Complete Performance Index (TCPI)

| Metric | Formula | Value | Interpretation |
|--------|---------|-------|---------------|
| **TCPI (BAC)** | [(BAC - EV) / (BAC - AC)] | [X.XX] | [>1 = must perform better, <1 = can relax] |
| **TCPI (EAC)** | [(BAC - EV) / (EAC - AC)] | [X.XX] | [Required CPI for remaining work] |

### TCPI Interpretation

| TCPI Value | Meaning | Action |
|-----------|---------|--------|
| [≤ 0.95] | [Under budget — comfortable margin] | [Maintain current approach] |
| [0.95 - 1.05] | [On budget — tight but achievable] | [Monitor closely] |
| [1.05 - 1.10] | [Over budget — needs improvement] | [Cost reduction measures] |
| [> 1.10] | [Significantly over — unrealistic] | [Re-baseline or scope reduction] |

## 8. Forecast Scenarios

| Scenario | Assumption | EAC | VAC | Probability |
|----------|-----------|-----|-----|-----------|
| **Optimistic** | [CPI improves to 1.0, no additional costs] | $[X] | $[Y] | [20%] |
| **Base Case** | [CPI stays at current level] | $[X] | $[Y] | [60%] |
| **Pessimistic** | [CPI drops 10%, contingency partially used] | $[X] | $[Y] | [20%] |
| **Expected Value** | [Weighted average] | $[X] | $[Y] | — |

## 9. Forecast Alerts

| Alert | Threshold | Current | Status | Action |
|-------|----------|---------|--------|--------|
| [EAC > BAC + 10%] | [110% of BAC] | [X%] | 🟢🟡🔴 | [Investigate if triggered] |
| [VAC < 0] | [Negative variance] | $[X] | 🟢🟡🔴 | [Cost reduction needed] |
| [TCPI > 1.10] | [Unrealistic performance needed] | [X.XX] | 🟢🟡🔴 | [Re-baseline discussion] |
| [Contingency > 50% used] | [50% of contingency] | [X%] | 🟢🟡🔴 | [Sponsor notification] |

## 10. Forecast Assumptions

| # | Assumption | Impact if Invalid |
|---|-----------|-------------------|
| 1 | [No additional scope changes] | [EAC increases] |
| 2 | [Vendor costs remain fixed] | [EAC increases if vendor raises prices] |
| 3 | [Team size remains stable] | [EAC increases if additional resources needed] |
| 4 | [CPI trend continues] | [EAC changes if CPI improves/declines] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Cost-Baseline]] | Baseline being forecasted against |
| [[Work-Performance-Data]] | Actual cost data |
| [[Work-Performance-Information]] | Cost analysis |
| [[Financial-Management-Plan]] | How costs are managed |
| [[Earned-Value-Analysis]] | EVM calculations |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** Update cost forecasts monthly. Use multiple methods and compare. Report the recommended EAC to stakeholders. If EAC exceeds BAC + contingency, escalate immediately.
