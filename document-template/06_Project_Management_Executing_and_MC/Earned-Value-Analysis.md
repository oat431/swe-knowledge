---
document_type: Earned Value Analysis
version: "1.0"
status: Active
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [earned-value, evm, spi, cpi, pmbok, iso-21508]
standard_ref:
  - PMBOK v8 — Monitoring & Controlling
  - ISO 21508 — Earned Value Management
---

# Earned Value Analysis

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document performs Earned Value Management (EVM) analysis — integrating scope, schedule, and cost to provide objective performance measurement.

## 2. EVM Metrics Reference

| Metric | Formula | Description |
|--------|---------|-------------|
| **PV** (Planned Value) | [Budget × % scheduled] | [Budgeted cost of work scheduled] |
| **EV** (Earned Value) | [Budget × % complete] | [Budgeted cost of work performed] |
| **AC** (Actual Cost) | [Actual expenditure] | [Actual cost of work performed] |
| **BAC** (Budget at Completion) | [Total baseline budget] | [Total planned budget] |
| **SV** (Schedule Variance) | [EV - PV] | [Positive = ahead, Negative = behind] |
| **CV** (Cost Variance) | [EV - AC] | [Positive = under, Negative = over] |
| **SPI** (Schedule Performance Index) | [EV / PV] | [>1 = ahead, <1 = behind] |
| **CPI** (Cost Performance Index) | [EV / AC] | [>1 = under budget, <1 = over] |
| **EAC** (Estimate at Completion) | [BAC / CPI] | [Projected total cost] |
| **ETC** (Estimate to Complete) | [EAC - AC] | [Remaining cost to complete] |
| **VAC** (Variance at Completion) | [BAC - EAC] | [Projected budget variance] |
| **TCPI** (To-Complete Performance Index) | [(BAC - EV) / (BAC - AC)] | [Required CPI for remaining work] |

## 3. EVM Tracking Table

| Period | PV | EV | AC | SV | CV | SPI | CPI | EAC | ETC | VAC |
|--------|-----|-----|-----|-----|-----|------|------|------|------|------|
| [Week 1-2] | $[X] | $[Y] | $[Z] | $[±] | $[±] | [X.XX] | [X.XX] | $[X] | $[Y] | $[Z] |
| [Week 3-4] | $[X] | $[Y] | $[Z] | $[±] | $[±] | [X.XX] | [X.XX] | $[X] | $[Y] | $[Z] |
| [Week 5-6] | $[X] | $[Y] | $[Z] | $[±] | $[±] | [X.XX] | [X.XX] | $[X] | $[Y] | $[Z] |
| [Week 7-8] | $[X] | $[Y] | $[Z] | $[±] | $[±] | [X.XX] | [X.XX] | $[X] | $[Y] | $[Z] |
| [Week 9-10] | $[X] | $[Y] | $[Z] | $[±] | $[±] | [X.XX] | [X.XX] | $[X] | $[Y] | $[Z] |
| **Current** | **$[PV]** | **$[EV]** | **$[AC]** | **$[SV]** | **$[CV]** | **[SPI]** | **[CPI]** | **$[EAC]** | **$[ETC]** | **$[VAC]** |

## 4. Current Period Analysis

### 4.1 Performance Summary

| Metric | Value | Target | Status | Interpretation |
|--------|-------|--------|--------|---------------|
| [SPI] | [0.98] | [≥0.95] | 🟢 OK | [2% behind schedule — acceptable] |
| [CPI] | [0.97] | [≥0.95] | 🟢 OK | [3% over budget — acceptable] |
| [SV] | [-$X] | [≥$0] | 🟡 Slight | [Behind by $X in work value] |
| [CV] | [-$Y] | [≥$0] | 🟡 Slight | [Over budget by $Y] |
| [EAC] | $[Z] | [≤BAC] | 🟡 Slight | [Projected $Y over budget] |
| [VAC] | [-$W] | [≥$0] | 🟡 Slight | [May overrun by $W] |
| [TCPI] | [1.02] | [≤1.10] | 🟢 OK | [Must achieve 1.02 CPI for remaining work] |

### 4.2 Performance Interpretation

| SPI/CPI Combination | Interpretation | Action |
|--------------------|---------------|--------|
| SPI > 1, CPI > 1 | [Ahead of schedule, under budget] | [Maintain — excellent performance] |
| SPI > 1, CPI < 1 | [Ahead of schedule, over budget] | [Investigate cost overruns] |
| SPI < 1, CPI > 1 | [Behind schedule, under budget] | [Consider adding resources] |
| SPI < 1, CPI < 1 | [Behind schedule, over budget] | [Escalate — corrective action needed] |
| **Current: SPI=0.98, CPI=0.97** | **[Slightly behind schedule, slightly over budget]** | **[Monitor — within tolerance]** |

## 5. EVM Trend Analysis

### 5.1 SPI Trend

| Period | SPI | Trend | Forecast |
|--------|-----|-------|----------|
| [Week 1-2] | [0.90] | — | [Behind] |
| [Week 3-4] | [0.95] | ↑ | [Improving] |
| [Week 5-6] | [1.00] | ↑ | [On track] |
| [Week 7-8] | [0.98] | ↓ | [Slight dip] |
| **Current** | **[0.98]** | **→** | **[Stable]** |

### 5.2 CPI Trend

| Period | CPI | Trend | Forecast |
|--------|-----|-------|----------|
| [Week 1-2] | [0.95] | — | [Slightly over] |
| [Week 3-4] | [0.97] | ↑ | [Improving] |
| [Week 5-6] | [0.99] | ↑ | [Nearly on budget] |
| [Week 7-8] | [0.97] | ↓ | [Slight dip] |
| **Current** | **[0.97]** | **→** | **[Stable]** |

### 5.3 EAC Trend

| Period | EAC | VAC | Trend | Confidence |
|--------|-----|-----|-------|-----------|
| [Week 1-2] | $[X] | $[Y] | — | Low |
| [Week 3-4] | $[X] | $[Y] | ↓ | Medium |
| [Week 5-6] | $[X] | $[Y] | ↓ | Medium |
| [Week 7-8] | $[X] | $[Y] | → | Medium |
| **Current** | **$[X]** | **$[Y]** | **→** | **Medium** |

## 6. EVM by WBS Element

| WBS | Element | PV | EV | AC | SV | CV | SPI | CPI | Status |
|-----|---------|-----|-----|-----|-----|-----|------|------|--------|
| 1.1 | [Project Management] | $[X] | $[Y] | $[Z] | $[±] | $[±] | [X.XX] | [X.XX] | 🟢🟡🔴 |
| 1.2 | [Requirements & Design] | $[X] | $[Y] | $[Z] | $[±] | $[±] | [X.XX] | [X.XX] | 🟢🟡🔴 |
| 1.3 | [Development] | $[X] | $[Y] | $[Z] | $[±] | $[±] | [X.XX] | [X.XX] | 🟢🟡🔴 |
| 1.4 | [Testing] | $[X] | $[Y] | $[Z] | $[±] | $[±] | [X.XX] | [X.XX] | 🟢🟡🔴 |
| 1.5 | [Deployment] | $[X] | $[Y] | $[Z] | $[±] | $[±] | [X.XX] | [X.XX] | 🟢🟡🔴 |
| | **Total** | **$[PV]** | **$[EV]** | **$[AC]** | **$[SV]** | **$[CV]** | **[SPI]** | **[CPI]** | **🟢🟡🔴** |

## 7. EVM Alerts

| Alert | Threshold | Current | Status | Action |
|-------|----------|---------|--------|--------|
| [SPI < 0.90] | [0.90] | [0.98] | 🟢 OK | None |
| [CPI < 0.90] | [0.90] | [0.97] | 🟢 OK | None |
| [EAC > BAC + Contingency] | [BAC + 15%] | $[X] | 🟢 OK | None |
| [TCPI > 1.10] | [1.10] | [1.02] | 🟢 OK | None |
| [SV < -$10K] | [-$10K] | [-$X] | 🟢🟡🔴 | Monitor |
| [CV < -$10K] | [-$10K] | [-$X] | 🟢🟡🔴 | Monitor |

## 8. Corrective Actions (If Needed)

| # | Trigger | Action | Owner | Expected Impact |
|---|---------|--------|-------|----------------|
| 1 | [SPI < 0.90 for 2 consecutive periods] | [Schedule recovery plan] | PM | [SPI improvement to ≥0.95] |
| 2 | [CPI < 0.90 for 2 consecutive periods] | [Cost reduction measures] | PM | [CPI improvement to ≥0.95] |
| 3 | [EAC > BAC + Contingency] | [Steering committee review] | Sponsor | [Scope/budget decision] |
| 4 | [TCPI > 1.10] | [Re-baseline discussion] | PM + Sponsor | [Achievable targets] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Cost-Baseline]] | BAC and PV baseline |
| [[Cost-Forecasts]] | EAC and ETC detail |
| [[Schedule-Forecasts]] | SPI-based schedule forecasts |
| [[Work-Performance-Data]] | Raw EV, AC data |
| [[Work-Performance-Information]] | EVM analysis interpretation |
| [[Variance-Analysis-Reports]] | Detailed variance analysis |

---

> **Template Standard:** Based on PMBOK v8, ISO 21508
> **Usage:** EVM provides *objective* performance measurement — not opinions. Update bi-weekly. If SPI or CPI drop below 0.90 for two consecutive periods, corrective action is mandatory. Report EVM metrics in every status report.
