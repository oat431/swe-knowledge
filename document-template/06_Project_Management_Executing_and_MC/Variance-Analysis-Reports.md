---
document_type: Variance Analysis Reports
version: "1.0"
status: Active
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [variance-analysis, baseline-comparison, corrective-action, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Monitoring & Controlling
  - ISO 21502 — Project Management Guidance
---

# Variance Analysis Reports

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document analyzes variances between baseline and actual performance — identifying causes, impacts, and corrective actions.

## 2. Variance Summary

### 2.1 Overall Variance Dashboard

| Dimension | Baseline | Actual | Variance | % Variance | Threshold | Status |
|-----------|---------|--------|---------|-----------|----------|--------|
| [Schedule — Go-Live] | [YYYY-MM-DD] | [Forecast: YYYY-MM-DD] | [+2 days] | [+1.3%] | [±5%] | 🟢 OK |
| [Cost — Total Budget] | $[BAC] | $[Actual + ETC] | $[+X] | [+Y%] | [±10%] | 🟢🟡🔴 |
| [Scope — Features] | [20 features] | [19 complete, 1 in progress] | [-1] | [-5%] | [±10%] | 🟢 OK |
| [Quality — Defect Density] | [<2/feature] | [1.5/feature] | [-0.5] | [-25%] | [≤2] | 🟢 OK |
| [Velocity — Sprint SP] | [20 SP/sprint] | [20 SP average] | [0] | [0%] | [±20%] | 🟢 OK |

### 2.2 Variance Trend

| Period | Schedule Variance | Cost Variance | Scope Variance | Quality Variance |
|--------|------------------|--------------|---------------|-----------------|
| [Month 1] | [+5 days] | [+$X] | [0 features] | [3.0/feature] |
| [Month 2] | [+3 days] | [+$X] | [0 features] | [2.0/feature] |
| [Month 3] | [+2 days] | [+$X] | [0 features] | [1.5/feature] |
| [Month 4] | [+2 days] | [+$X] | [-1 feature] | [1.2/feature] |
| **Current** | **[+2 days]** | **[+$X]** | **[-1 feature]** | **[1.5/feature]** |
| **Trend** | **↓ Improving** | **→ Stable** | **→ Stable** | **↓ Improving** |

## 3. Schedule Variance Analysis

### 3.1 Milestone Variance

| Milestone | Baseline | Actual/Forecast | Variance (days) | Variance (%) | Cause |
|----------|---------|----------------|----------------|-------------|-------|
| [Requirements Baselined] | [YYYY-MM-DD] | [YYYY-MM-DD] | [0] | [0%] | — |
| [Design Approved] | [YYYY-MM-DD] | [YYYY-MM-DD] | [+2] | [+4%] | [Architecture review took longer] |
| [Sprint 1-3] | [Various] | [Various] | [-1 to 0] | [0%] | [Sprint 3 early] |
| [Go-Live] | [YYYY-MM-DD] | [YYYY-MM-DD] | [+2] | [+1.3%] | [Design delay cascaded] |

### 3.2 Schedule Variance Root Causes

| # | Variance | Root Cause | Category | Impact | Corrective Action |
|---|---------|-----------|----------|--------|------------------|
| 1 | [+2 days — Design] | [Architecture review needed extra cycle] | Process | [+2 days to project] | [Add buffer for future reviews] |
| 2 | [+1 day — Integration POC] | [ERP API more complex than expected] | Technical | [Partially absorbed] | [Better POC scoping] |
| 3 | [-1 day — Sprint 3] | [Team velocity exceeded plan] | Positive | [-1 day recovery] | [Maintain team momentum] |

### 3.3 Schedule Variance Corrective Actions

| # | Action | Owner | Deadline | Expected Impact | Status |
|---|--------|-------|----------|----------------|--------|
| 1 | [Add 1-day buffer to future review milestones] | PM | [Next phase gate] | [Prevent similar delays] | ⬜ Open |
| 2 | [Better POC scoping for integrations] | TL | [Before Sprint 4] | [Reduce integration surprises] | ⬜ Open |
| 3 | [Accept +2 day variance — within tolerance] | PM | [N/A] | [No recovery needed] | ✅ Accepted |

## 4. Cost Variance Analysis

### 4.1 Category Variance

| Category | Baseline | Actual | Variance ($) | Variance (%) | Cause |
|----------|---------|--------|-------------|-------------|-------|
| [Personnel] | $[X] | $[Y] | $[+Z] | [+W%] | [Overtime during integration] |
| [Software] | $[X] | $[Y] | $[-Z] | [-W%] | [Lower usage than projected] |
| [Infrastructure] | $[X] | $[Y] | $[-Z] | [-W%] | [Right-sized instances] |
| [Vendor] | $[X] | $[Y] | $[0] | [0%] | [Fixed-price contracts] |
| **Total** | **$[BAC]** | **$[Actual]** | **$[+Z]** | **[+W%]** | |

### 4.2 Cost Variance Root Causes

| # | Variance | Root Cause | Category | Impact | Corrective Action |
|---|---------|-----------|----------|--------|------------------|
| 1 | [+$X — Personnel] | [Overtime during Sprint 3 integration] | Resource | [+$X to budget] | [Better integration planning] |
| 2 | [-$Y — Software] | [Lower API calls than projected] | Positive | [-$Y savings] | [Update forecast] |
| 3 | [-$Z — Infrastructure] | [Right-sized after load testing] | Positive | [-$Z savings] | [Maintain current config] |

### 4.3 Cost Variance Corrective Actions

| # | Action | Owner | Deadline | Expected Impact | Status |
|---|--------|-------|----------|----------------|--------|
| 1 | [Better integration task estimation] | TL | [Next sprint] | [Reduce overtime] | ⬜ Open |
| 2 | [Update infrastructure forecast] | PM | [Monthly report] | [Accurate EAC] | ✅ Done |
| 3 | [Reallocate savings to contingency] | PM | [Monthly report] | [Buffer maintained] | ✅ Done |

## 5. Scope Variance Analysis

### 5.1 Scope Changes

| Change | Type | Impact | Reason | Decision |
|--------|------|--------|--------|----------|
| [Added save draft feature] | [Added] | [+3 days, +$2K] | [User feedback] | ✅ Approved |
| [Deferred ML anomaly detection] | [Removed] | [-3 months, -$50K] | [Budget constraint] | ✅ Approved |
| [Added bulk upload] | [Added] | [+5 days, +$3K] | [Missed requirement] | ✅ Approved |
| **Net Scope Change** | | **[+8 days, -$45K]** | | |

### 5.2 Requirements Stability

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Total Requirements] | [X] | — | — |
| [Requirements Changed] | [X] | [<15%] | 🟢 OK |
| [Requirements Added] | [X] | — | — |
| [Requirements Removed] | [X] | — | — |
| [Requirements Stability Index] | [X%] | [≥85%] | 🟢 OK |

## 6. Quality Variance Analysis

### 6.1 Quality Metrics Variance

| Metric | Target | Actual | Variance | Status | Trend |
|--------|--------|--------|---------|--------|-------|
| [Defect Density] | [<2/feature] | [1.5/feature] | [-0.5] | 🟢 Better | ↓ Improving |
| [Code Coverage] | [≥80%] | [82%] | [+2%] | 🟢 Better | → Stable |
| [Test Pass Rate] | [≥95%] | [97%] | [+2%] | 🟢 Better | → Stable |
| [Critical Defects] | [0] | [0] | [0] | 🟢 OK | → Stable |

### 6.2 Quality Variance Root Causes

| # | Variance | Root Cause | Category | Impact | Corrective Action |
|---|---------|-----------|----------|--------|------------------|
| 1 | [Defect density improving] | [Code reviews catching more defects] | Positive | [Higher quality] | [Maintain code reviews] |
| 2 | [Code coverage above target] | [Team committed to testing] | Positive | [Better coverage] | [Maintain testing discipline] |

## 7. Variance Corrective Action Summary

| # | Dimension | Variance | Root Cause | Action | Owner | Deadline | Status |
|---|----------|---------|-----------|--------|-------|----------|--------|
| 1 | Schedule | [+2 days] | [Review buffer insufficient] | [Add buffer] | PM | [Next gate] | ⬜ Open |
| 2 | Cost | [+$X personnel] | [Integration overtime] | [Better estimation] | TL | [Next sprint] | ⬜ Open |
| 3 | Scope | [+1 feature] | [Missed requirement] | [Better elicitation] | BA | [Next project] | ⬜ Open |
| 4 | Quality | [Improving] | [Code reviews] | [Maintain] | TL | [Ongoing] | ✅ Ongoing |

## 8. Variance Thresholds

| Dimension | Green | Yellow | Red |
|-----------|-------|--------|-----|
| [Schedule] | [±5% of baseline] | [5-10%] | [>10%] |
| [Cost] | [±10% of BAC] | [10-15%] | [>15%] |
| [Scope] | [±10% of features] | [10-20%] | [>20%] |
| [Quality] | [Within target] | [10% over target] | [>10% over target] |
| [Velocity] | [±20% of planned] | [20-30%] | [>30%] |

## 9. Variance Reporting

| Report | Audience | Frequency | Content |
|--------|----------|-----------|---------|
| [Weekly Variance Summary] | [PM] | Weekly | [Key variances, status] |
| [Monthly Variance Report] | [Sponsor, Steering] | Monthly | [Full analysis, corrective actions] |
| [Phase Gate Variance Review] | [Steering Committee] | Per phase | [Cumulative variance, lessons] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Work-Performance-Data]] | Raw variance data |
| [[Work-Performance-Information]] | Variance analysis interpretation |
| [[Earned-Value-Analysis]] | EVM-based variance |
| [[Cost-Forecasts]] | Cost variance forecasting |
| [[Schedule-Forecasts]] | Schedule variance forecasting |
| [[Change-Log]] | Scope variance tracking |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** Variance analysis answers "Why are we off baseline?" Not all variances are bad — positive variances (ahead of schedule, under budget) should be understood too. Focus on root causes, not symptoms. Every significant variance needs a corrective action or an acceptance decision.
