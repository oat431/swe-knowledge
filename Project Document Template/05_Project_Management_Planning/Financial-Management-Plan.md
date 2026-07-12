---
document_type: Financial Management Plan
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
finance_owner: "[Finance Analyst]"
classification: "Internal / Confidential"
tags: [financial-management, budget, cost, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Planning (Cost Management)
  - ISO 21502 — Project Management Guidance
---

# Financial Management Plan

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Project Manager | [Name / Role] |
| Finance Analyst | [Name / Role] |

### Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Sponsor | | | |
| Finance Director | | | |
| Project Manager | | | |

---

## 1. Purpose

> This plan defines how project costs will be estimated, budgeted, monitored, controlled, and reported.

## 2. Cost Management Approach

### 2.1 Estimation Method

| Method | When to Use | Accuracy |
|--------|------------|----------|
| [Analogous Estimation] | [Early planning — based on similar projects] | [-25% to +75%] |
| [Parametric Estimation] | [When historical data exists — cost per unit] | [-15% to +50%] |
| [Bottom-Up Estimation] | [Detailed planning — activity-level estimates] | [-10% to +25%] |
| [Three-Point Estimation] | [When uncertainty is high — optimistic + pessimistic + most likely] | [-10% to +15%] |

### 2.2 Budget Structure

| Category | Description | Estimation Method |
|----------|-------------|------------------|
| [Personnel] | [Internal team costs — salaries, benefits] | [Bottom-up × loaded rate] |
| [Software/Licensing] | [SaaS subscriptions, tools] | [Vendor quotes] |
| [Infrastructure] | [Cloud hosting, networking] | [Parametric — usage-based] |
| [Services] | [Vendor implementation, consulting] | [Vendor quotes] |
| [Training] | [Training materials, delivery] | [Analogous] |
| [Travel] | [If applicable] | [Analogous] |
| [Contingency] | [Reserve for known risks] | [15% of subtotal] |
| [Management Reserve] | [Reserve for unknown risks] | [5% of total] |

## 3. Budget Summary

### 3.1 Cost Breakdown

| Category | One-Time | Annual | Total (Project Period) |
|----------|---------|--------|----------------------|
| [Personnel — Internal] | — | $[X] | $[X] |
| [Software / Licensing] | $[X] | $[Y] | $[X + Y] |
| [Infrastructure] | $[X] | $[Y] | $[X + Y] |
| [Vendor Services] | $[X] | — | $[X] |
| [Training] | $[X] | — | $[X] |
| [Travel] | $[X] | — | $[X] |
| **Subtotal** | **$[X]** | **$[Y]** | **$[Sum]** |
| [Contingency (15%)] | | | $[X] |
| [Management Reserve (5%)] | | | $[X] |
| **Total Budget** | | | **$[Grand Total]** |

### 3.2 Cost by Phase

| Phase | Duration | Personnel | Other | Total |
|-------|----------|----------|-------|-------|
| [Initiation] | [1 week] | $[X] | $[Y] | $[Sum] |
| [Planning] | [7 weeks] | $[X] | $[Y] | $[Sum] |
| [Execution] | [10 weeks] | $[X] | $[Y] | $[Sum] |
| [Testing] | [5 weeks] | $[X] | $[Y] | $[Sum] |
| [Deployment] | [5 weeks] | $[X] | $[Y] | $[Sum] |
| [Closure] | [2 weeks] | $[X] | $[Y] | $[Sum] |
| **Total** | **[30 weeks]** | **$[X]** | **$[Y]** | **$[Sum]** |

## 4. Cost Monitoring

### 4.1 Cost Metrics

| Metric | Formula | Target | Frequency |
|--------|---------|--------|-----------|
| [Cost Performance Index (CPI)] | [Earned Value / Actual Cost] | [≥0.95] | [Weekly] |
| [Cost Variance (CV)] | [Earned Value - Actual Cost] | [≥0] | [Weekly] |
| [Estimate at Completion (EAC)] | [BAC / CPI] | [Within budget] | [Monthly] |
| [Variance at Completion (VAC)] | [BAC - EAC] | [≥0] | [Monthly] |
| [Budget Utilization %] | [Actual Cost / Total Budget × 100] | [≤% complete] | [Monthly] |

### 4.2 Cost Control Thresholds

| Scenario | Threshold | Action | Authority |
|----------|----------|--------|----------|
| [Monthly spend > 10% over plan] | [10%] | [Investigate, corrective action] | [PM] |
| [CPI < 0.90] | [0.90] | [Escalate to sponsor] | [PM + Sponsor] |
| [EAC > BAC + Contingency] | [BAC + Contingency] | [Steering committee review] | [Steering Committee] |
| [Management reserve access] | [Any] | [Sponsor approval required] | [Sponsor] |

## 5. Financial Reporting

| Report | Audience | Frequency | Content |
|--------|----------|-----------|---------|
| [Weekly Cost Report] | [PM] | Weekly | [Actual vs planned, variances] |
| [Monthly Financial Report] | [Sponsor, Finance] | Monthly | [EAC, CPI, CV, forecast] |
| [Phase Gate Financial Review] | [Steering Committee] | Per gate | [Budget status, forecast] |
| [Final Financial Report] | [Sponsor, Finance] | Project closure | [Final actuals, lessons learned] |

## 6. Earned Value Management

### 6.1 EVM Metrics Definition

| Metric | Definition | Formula |
|--------|-----------|---------|
| **Planned Value (PV)** | [Budgeted cost of work scheduled] | [Baseline cost × % scheduled] |
| **Earned Value (EV)** | [Budgeted cost of work performed] | [Baseline cost × % complete] |
| **Actual Cost (AC)** | [Actual cost of work performed] | [Actual expenditure] |
| **Schedule Variance (SV)** | [EV - PV] | [Positive = ahead, Negative = behind] |
| **Cost Variance (CV)** | [EV - AC] | [Positive = under, Negative = over] |
| **Schedule Performance Index (SPI)** | [EV / PV] | [>1 = ahead, <1 = behind] |
| **Cost Performance Index (CPI)** | [EV / AC] | [>1 = under budget, <1 = over] |
| **Budget at Completion (BAC)** | [Total planned budget] | [Sum of all baselined costs] |
| **Estimate at Completion (EAC)** | [Projected total cost] | [BAC / CPI] |
| **Variance at Completion (VAC)** | [BAC - EAC] | [Positive = under, Negative = over] |

### 6.2 EVM Tracking Template

| Period | PV | EV | AC | SV | CV | SPI | CPI | EAC | VAC |
|--------|-----|-----|-----|-----|-----|------|------|------|------|
| [Week 1] | $ | $ | $ | $ | $ | | | $ | $ |
| [Week 2] | $ | $ | $ | $ | $ | | | $ | $ |
| [Week 3] | $ | $ | $ | $ | $ | | | $ | $ |
| [Week 4] | $ | $ | $ | $ | $ | | | $ | $ |

## 7. Invoice & Payment Process

| Step | Activity | Owner | Timing |
|------|----------|-------|--------|
| 1 | [Vendor submits invoice] | [Vendor] | [Monthly] |
| 2 | [PM verifies deliverables] | [PM] | [Within 5 days] |
| 3 | [Finance processes payment] | [Finance] | [Net 30] |
| 4 | [PM records in cost tracking] | [PM] | [Upon payment] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Cost-Estimates]] | Detailed cost estimates |
| [[Cost-Baseline]] | Approved budget baseline |
| [[Project-Funding-Requirements]] | Funding schedule |
| [[Project-Management-Plan]] | Parent plan |
| [[Procurement-Management-Plan]] | Vendor cost management |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** This plan defines *how* costs are managed. Track EVM metrics weekly. Report to sponsor monthly. Escalate immediately if CPI drops below 0.90.
