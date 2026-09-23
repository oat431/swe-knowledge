---
document_type: Potential Value
schema_version: 2
canonical_name: Potential Value
doc_form: heavy
applicability: universal
min_project_tier: 4  # Small-Prod
minimum_form: Lightweight markdown doc (frontmatter + required sections only)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
ba_owner: "[Business Analyst Name]"
sponsor: "[Sponsor Name]"
classification: "Internal / Confidential"
tags: [potential-value, benefits, roi, value-proposition, babok]
standard_ref:
  - BABOK v3 — Strategy Analysis
  - PMBOK v8 — Initiating (Benefits Management)
  - ISO 21505 — Governance of Projects, Programmes and Portfolios
---

# Potential Value

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Archived]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Sponsor | [Name / Role] |
| Business Analyst | [Name / Role] |
| Finance Analyst | [Name / Role] |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial draft |
| 1.0 | [YYYY-MM-DD] | [Name] | Approved version |

### Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Sponsor | | | |
| Finance Director | | | |
| Business Owner | | | |
| BA Lead | | | |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Value Framework](#2-value-framework)
3. [Financial Value](#3-financial-value)
4. [Operational Value](#4-operational-value)
5. [Strategic Value](#5-strategic-value)
6. [Risk Reduction Value](#6-risk-reduction-value)
7. [Customer Value](#7-customer-value)
8. [Value Summary](#8-value-summary)
9. [Value Realization Tracking](#9-value-realization-tracking)
10. [Sensitivity & Scenarios](#10-sensitivity--scenarios)

---

## 1. Executive Summary

| Field | Detail |
|-------|--------|
| Total Estimated Annual Value | $[X] |
| Financial Value | $[X] — cost savings + revenue |
| Operational Value | [X]% efficiency improvement |
| Strategic Value | [Qualitative — competitive position, market access] |
| Risk Reduction Value | $[X] — avoided costs |
| Customer Value | [NPS +X, retention +Y%] |
| Investment Required | $[X] |
| Net Value (Year 1) | $[X] |
| ROI | [X%] |
| Payback Period | [X months] |

---

## 2. Value Framework

### 2.1 Value Categories

> Value is measured across five dimensions — not just financial return.

```mermaid
flowchart TB
    R(("Total Value"))
    n1["Financial"]
    R --> n1
    n2["Cost Savings"]
    n1 --> n2
    n3["Revenue Increase"]
    n1 --> n3
    n4["Cost Avoidance"]
    n1 --> n4
    n5["Operational"]
    R --> n5
    n6["Efficiency"]
    n5 --> n6
    n7["Speed"]
    n5 --> n7
    n8["Quality"]
    n5 --> n8
    n9["Strategic"]
    R --> n9
    n10["Market Position"]
    n9 --> n10
    n11["Innovation"]
    n9 --> n11
    n12["Scalability"]
    n9 --> n12
    n13["Risk Reduction"]
    R --> n13
    n14["Compliance"]
    n13 --> n14
    n15["Security"]
    n13 --> n15
    n16["Business Continuity"]
    n13 --> n16
    n17["Customer"]
    R --> n17
    n18["Satisfaction"]
    n17 --> n18
    n19["Retention"]
    n17 --> n19
    n20["Acquisition"]
    n17 --> n20
```

### 2.2 Value Measurement Approach

| Category | Measurement Type | Method | Confidence |
|----------|-----------------|--------|-----------|
| Financial | Quantitative — $ | Cost analysis, revenue modeling | 🟢 High |
| Operational | Quantitative — % / time | Process measurement, time studies | 🟢 High |
| Strategic | Qualitative + Quantitative | Market analysis, competitive benchmarking | 🟡 Medium |
| Risk Reduction | Quantitative — $ | Risk assessment, probability × impact | 🟡 Medium |
| Customer | Quantitative — score / % | Surveys, retention analysis | 🟡 Medium |

---

## 3. Financial Value

### 3.1 Cost Savings

| ID | Savings Area | Current Cost | Future Cost | Annual Savings | Confidence | Realization Timeline |
|----|-------------|-------------|------------|---------------|-----------|---------------------|
| FS-[XX] | [e.g., Manual labor — data entry] | $[X]/year | $[Y]/year | $[X-Y] | 🟢 High | [Phase [N] + [N] months] |
| FS-[XX] | [e.g., Error rework costs] | $[X]/year | $[Y]/year | $[X-Y] | 🟢 High | [Phase [N] + [N] months] |
| FS-[XX] | [e.g., Legacy system maintenance] | $[X]/year | $[Y]/year | $[X-Y] | 🟡 Medium | [Phase [N] complete] |
| FS-[XX] | [e.g., Paper/printing costs] | $[X]/year | $[Y]/year | $[X-Y] | 🟢 High | [Phase [N] + [N] months] |
| FS-[XX] | [e.g., Support call reduction] | $[X]/year | $[Y]/year | $[X-Y] | 🟡 Medium | [Phase [N] + [N] months] |
| **Total Cost Savings** | | | | **$[Sum]** | | |

### 3.2 Revenue Increase

| ID | Revenue Source | Current Revenue | Projected Revenue | Annual Increase | Confidence | Realization Timeline |
|----|---------------|----------------|------------------|----------------|-----------|---------------------|
| FR-[XX] | [e.g., Faster onboarding → more customers] | $[X]/year | $[Y]/year | $[Y-X] | 🟡 Medium | [Phase [N] + [N] months] |
| FR-[XX] | [e.g., Self-service → higher conversion] | $[X]/year | $[Y]/year | $[Y-X] | 🟡 Medium | [Phase [N] + [N] months] |
| FR-[XX] | [e.g., New digital channel] | $0 | $[Y]/year | $[Y] | 🔴 Low | [Phase [N] + [N] months] |
| **Total Revenue Increase** | | | | **$[Sum]** | | |

### 3.3 Cost Avoidance

| ID | Avoided Cost | Description | Amount | Confidence | Trigger |
|----|-------------|-------------|--------|-----------|---------|
| FA-[XX] | [e.g., Regulatory fine avoidance] | [Non-compliance penalty if audit fails] | $[X]/year | 🟡 Medium | [Regulatory deadline] |
| FA-[XX] | [e.g., Legacy system end-of-life] | [Emergency replacement cost if delayed] | $[X] one-time | 🟢 High | [Vendor EOL date] |
| FA-[XX] | [e.g., Security breach avoidance] | [Average breach cost in industry] | $[X] probability-weighted | 🔴 Low | [Ongoing] |
| **Total Cost Avoidance** | | | **$[Sum]** | | |

### 3.4 Financial Summary

| Category | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 | Total |
|----------|--------|--------|--------|--------|--------|-------|
| Cost Savings | $ | $ | $ | $ | $ | $ |
| Revenue Increase | $ | $ | $ | $ | $ | $ |
| Cost Avoidance | $ | $ | $ | $ | $ | $ |
| **Total Financial Value** | **$** | **$** | **$** | **$** | **$** | **$** |

---

## 4. Operational Value

### 4.1 Efficiency Improvements

| ID | Process | Current | Target | Improvement | Value |
|----|---------|---------|--------|-------------|-------|
| OE-[XX] | [e.g., Customer Onboarding] | [[N] days] | [[N] days] | [[X]% faster] | [X more customers/month] |
| OE-[XX] | [e.g., Order Processing] | [[X] hours] | [[X] min] | [[X]% faster] | [X more orders/day] |
| OE-[XX] | [e.g., Report Generation] | [Weekly, manual] | [Real-time, auto] | [Instant] | [X hours/week saved] |
| OE-[XX] | [e.g., Error Resolution] | [[X]% error rate] | [<[X]%] | [[X]% reduction] | [X hours/week saved] |

### 4.2 Productivity Gains

| ID | Area | Current Productivity | Target Productivity | Gain | Annual Value |
|----|------|---------------------|--------------------|-------|----|
| OP-[XX] | [e.g., Operations team] | [X transactions/FTE/day] | [Y transactions/FTE/day] | [+Z%] | $[FTE savings or capacity increase] |
| OP-[XX] | [e.g., Management decision time] | [Week-old data] | [Real-time data] | [Faster decisions] | [Qualitative] |
| OP-[XX] | [e.g., IT maintenance] | [X hours/week firefighting] | [Y hours/week] | [-Z%] | $[Redirected to innovation] |

### 4.3 Quality Improvements

| ID | Quality Metric | Current | Target | Improvement | Value |
|----|---------------|---------|--------|-------------|-------|
| OQ-[XX] | [e.g., Data accuracy] | [[X]%] | [[X]%] | [+[X]%] | [Fewer errors, better decisions] |
| OQ-[XX] | [e.g., Process compliance] | [[X]%] | [[X]%] | [+[X]%] | [Audit readiness] |
| OQ-[XX] | [e.g., First-call resolution] | [[X]%] | [[X]%] | [+[X]%] | [Customer satisfaction] |

---

## 5. Strategic Value

### 5.1 Competitive Advantage

| ID | Strategic Benefit | Description | Impact | Measurement |
|----|------------------|-------------|--------|-------------|
| SV-[XX] | [e.g., Digital-first experience] | [Self-service portal matches competitors] | 🟡 High | [Competitive feature parity] |
| SV-[XX] | [e.g., Speed to market] | [New products launched in [N] days, not months] | 🟡 High | [Time-to-market metric] |
| SV-[XX] | [e.g., Data-driven decisions] | [Real-time analytics enable proactive strategy] | 🟡 High | [Decision cycle time] |
| SV-[XX] | [e.g., Scalability] | [Platform handles [N]x volume without re-architecture] | 🟢 Medium | [Capacity testing] |

### 5.2 Market Position

| Factor | Current Position | Target Position | Impact |
|--------|-----------------|----------------|--------|
| [e.g., Digital capability ranking] | [#X in market] | [#Y in market] | [Competitive differentiation] |
| [e.g., Customer experience score] | [Below average] | [Above average] | [Customer acquisition] |
| [e.g., Time-to-onboard vs competitors] | [Slowest] | [Among fastest] | [Market share protection] |

### 5.3 Innovation Enablement

| Capability Unlocked | Description | Future Value |
|--------------------|-------------|-------------|
| [e.g., API-first architecture] | [Enables partner ecosystem, marketplace] | [New revenue channels] |
| [e.g., Data platform] | [Enables AI/ML, predictive analytics] | [Operational intelligence] |
| [e.g., Cloud infrastructure] | [Enables rapid scaling, global expansion] | [Market expansion] |

---

## 6. Risk Reduction Value

### 6.1 Risk Avoidance

| ID | Risk | Current Exposure | Probability | Impact | Reduced Exposure | Annual Value |
|----|------|-----------------|------------|--------|-----------------|-------------|
| RV-[XX] | [e.g., Regulatory non-compliance] | [No audit trail] | [[Rating] — [X]%] | $[X] | [Full compliance] | $[X × 0.7] |
| RV-[XX] | [e.g., Data breach] | [Legacy security] | [[Rating] — [X]%] | $[Y] | [Modern security] | $[Y × 0.3] |
| RV-[XX] | [e.g., System failure] | [End-of-life systems] | [[Rating] — [X]%] | $[Z] | [Supported platform] | $[Z × 0.6] |
| RV-[XX] | [e.g., Key person dependency] | [Knowledge in [N] heads] | [[Rating] — [X]%] | $[W] | [Documented, automated] | $[W × 0.4] |
| **Total Risk Reduction** | | | | | | **$[Sum]** |

### 6.2 Compliance Value

| Regulation | Current Risk | Mitigation | Value |
|-----------|-------------|-----------|-------|
| [e.g., GDPR] | [No automated deletion] | [Data management platform] | [Fine avoidance — $X] |
| [e.g., Industry standard] | [No audit trail] | [Full logging] | [Certification readiness] |
| [e.g., Data retention law] | [Manual, inconsistent] | [Automated retention policy] | [Compliance assurance] |

---

## 7. Customer Value

### 7.1 Customer Experience Improvement

| ID | Metric | Current | Target | Impact |
|----|--------|---------|--------|--------|
| CV-[XX] | [e.g., Net Promoter Score (NPS)] | [[X]] | [[X]] | [Customer loyalty] |
| CV-[XX] | [e.g., Customer Satisfaction (CSAT)] | [[X]/5] | [[X]/5] | [Retention] |
| CV-[XX] | [e.g., Customer Effort Score (CES)] | [High effort] | [Low effort] | [Reduced churn] |
| CV-[XX] | [e.g., Onboarding Experience] | [Frustrating, slow] | [Fast, self-service] | [First impression] |

### 7.2 Customer Retention & Acquisition

| ID | Impact | Current | Projected | Value |
|----|--------|---------|-----------|-------|
| CA-[XX] | [e.g., Customer retention rate] | [X%] | [Y%] | [Retained customers × avg revenue] |
| CA-[XX] | [e.g., New customer acquisition] | [X/month] | [Y/month] | [Additional customers × avg revenue] |
| CA-[XX] | [e.g., Customer lifetime value] | $[X] | $[Y] | [Increased CLV × customer base] |

### 7.3 Customer Feedback Supporting Value

| Source | Feedback | Date | Implication |
|--------|---------|------|------------|
| [Customer Survey] | ["[Customer verbatim quote]"] | [YYYY-MM-DD] | [Implication] |
| [Support Tickets] | ["[Customer verbatim quote]"] | [YYYY-MM-DD] | [Implication] |
| [NPS Verbatim] | ["[Customer verbatim quote]"] | [YYYY-MM-DD] | [Implication] |

---

## 8. Value Summary

### 8.1 Total Value Summary

| Value Category | Annual Value | Confidence | % of Total |
|---------------|-------------|-----------|-----------|
| 💰 Financial (Cost Savings) | $[X] | 🟢 High | [X%] |
| 💰 Financial (Revenue Increase) | $[X] | 🟡 Medium | [X%] |
| 💰 Financial (Cost Avoidance) | $[X] | 🟡 Medium | [X%] |
| ⚙️ Operational (Efficiency) | $[X] | 🟢 High | [X%] |
| 🎯 Strategic | [Qualitative] | 🟡 Medium | — |
| 🛡️ Risk Reduction | $[X] | 🟡 Medium | [X%] |
| 👥 Customer | $[X] | 🟡 Medium | [X%] |
| **Total Annual Value** | **$[Sum]** | | **100%** |

### 8.2 Value vs Investment

| Metric | Value |
|--------|-------|
| Total Investment | $[X] |
| Total Annual Value | $[Y] |
| Simple ROI | [((Y - X) / X) × 100]% |
| Payback Period | [X / (Y/12)] months |
| NPV (5-year, X% discount) | $[Z] |
| Value-to-Investment Ratio | [Y:X] |

### 8.3 Value Composition

```mermaid
pie title Annual Value Distribution
    "Cost Savings" : [N]
    "Revenue Increase" : [N]
    "Risk Reduction" : [N]
    "Operational Efficiency" : [N]
    "Customer Value" : [N]
```

---

## 9. Value Realization Tracking

### 9.1 Benefits Realization Plan

| ID | Benefit | Expected Value | Measurement Method | Baseline | Target | Realization Date | Owner |
|----|---------|---------------|-------------------|----------|--------|-----------------|-------|
| BR-[XX] | [Cost savings — labor] | $[X]/year | [Hours saved × rate] | [Current hours] | [Target hours] | [Date] | [Ops Manager] |
| BR-[XX] | [Revenue — faster onboarding] | $[X]/year | [Additional customers × revenue] | [Current rate] | [Target rate] | [Date] | [Sales Lead] |
| BR-[XX] | [Risk — compliance] | $[X]/year | [Avoided fines] | [Current risk] | [Target risk] | [Date] | [Compliance] |
| BR-[XX] | [Customer — NPS] | +[X] points | [Measurement method] | [NPS [X]] | [NPS [X]] | [Date] | [Product Owner] |

### 9.2 Value Realization Timeline

```mermaid
flowchart LR
    subgraph P1["Phase 1 Value<br>Month 4+"]
        V1["[Value driver]<br>$[X]/year"]
        V2["[Value driver]<br>$[X]/year"]
        V3["[Value driver]<br>$[X]/year"]
    end

    subgraph P2["Phase 2 Value<br>Month 8+"]
        V4["[Value driver]<br>$[X]/year"]
        V5["[Value driver]<br>$[X]/year"]
        V6["[Value driver]<br>[Metric +X]"]
    end

    subgraph P3["Phase 3 Value<br>Month 12+"]
        V7["[Value driver]<br>$[X]/year"]
        V8["[Value driver]<br>[Metric +X]"]
        V9["[Value driver]"]
    end

    P1 --> P2 --> P3

    style P1 fill:#4CAF50,color:#fff
    style P2 fill:#2196F3,color:#fff
    style P3 fill:#9C27B0,color:#fff
```

### 9.3 Value Review Cadence

| Review | Timing | Participants | Focus |
|--------|--------|-------------|-------|
| Value Check | Monthly | BA, Finance | [Leading indicators trending?] |
| Benefits Review | Quarterly | Sponsor, BA, Finance | [Actual vs expected value] |
| Full Value Assessment | Semi-annually | Steering Committee | [ROI recalculation, strategy alignment] |
| Post-Implementation | 12 months post-go-live | All stakeholders | [Full value realization report] |

---

## 10. Sensitivity & Scenarios

### 10.1 Sensitivity Analysis

| Scenario | Change | Impact on Annual Value | Impact on ROI | Likelihood |
|----------|--------|----------------------|--------------|-----------|
| **Base Case** | As estimated | $[X] | [Y]% | Most likely |
| **Pessimistic** | Benefits -[X]%, Costs +[X]% | $[X × 0.7] | [Lower]% | Possible |
| **Optimistic** | Benefits +[X]%, Costs -[X]% | $[X × 1.2] | [Higher]% | Possible |
| **Delayed (6 months)** | Benefits start [N] months late | $[X × 0.75] | [Lower]% | Risk |
| **Partial Adoption** | Only [X]% user adoption | $[X × 0.6] | [Lower]% | Risk |

### 10.2 Scenario Outcomes

| Scenario | Annual Value | ROI | Payback | NPV (5-yr) |
|----------|-------------|-----|---------|-----------|
| 🟢 Optimistic | $[X] | [Y]% | [Z] months | $[W] |
| 🟡 Base Case | $[X] | [Y]% | [Z] months | $[W] |
| 🟠 Conservative | $[X] | [Y]% | [Z] months | $[W] |
| 🔴 Pessimistic | $[X] | [Y]% | [Z] months | $[W] |

### 10.3 Break-Even Analysis

| Variable | Break-Even Point | Current Estimate | Margin |
|----------|-----------------|-----------------|--------|
| [e.g., User adoption rate] | [X%] | [Y%] | [+Z% buffer] |
| [e.g., Cost savings realization] | [X%] of projected | [Y%] expected | [+Z% buffer] |
| [e.g., Implementation cost] | [≤ $X] | [$Y estimated] | [$Z buffer] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Business-Case]] | Potential value justifies the investment in the Business Case |
| [[Business-Objectives]] | Value measures objective achievement |
| [[Business-Requirements]] | Requirements deliver the value described here |
| [[Current-State-Description]] | Current costs and performance establish the baseline |
| [[Future-State-Description]] | Future state delivers the value described here |
| [[Change-Strategy]] | Strategy realization plan aligns with value timeline |
| [[Benefits-Management-Plan]] | Detailed benefits tracking and ownership |

---

> **Template Standard:** Based on BABOK v3 (Strategy Analysis), PMBOK v8 (Benefits Management), ISO 21505
> **Usage:** This document quantifies the *why* — the value that justifies the investment. All figures should be traceable to source data (interviews, benchmarks, financial reports). Use confidence levels honestly — inflated projections erode trust.
