---
document_type: Basis of Estimates
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [basis-of-estimates, estimation-rationale, confidence, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Planning (Cost Management, Schedule Management)
  - ISO 21502 — Project Management Guidance
---

# Basis of Estimates

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Project Manager | [Name / Role] |
| Technical Lead | [Name / Role] |

### Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Sponsor | | | |
| Project Manager | | | |

---

## 1. Purpose

> This document provides the supporting detail behind all project estimates — duration, cost, effort, and resource. It explains *how* estimates were derived, *what assumptions* they rest on, and *how confident* we are in them. This enables informed decision-making and re-estimation when assumptions change.

## 2. Estimation Methodology

### 2.1 Methods Used

| Method | When Used | Accuracy Range | Applied To |
|--------|----------|---------------|-----------|
| **Bottom-Up** | [Detailed planning — activity level] | [-10% to +25%] | [Sprint activities, development tasks] |
| **Analogous** | [Early planning — similar projects] | [-25% to +75%] | [Overall project duration, infrastructure] |
| **Parametric** | [Historical data — cost per unit] | [-15% to +50%] | [Cloud costs, licensing, personnel rates] |
| **Three-Point** | [High uncertainty — PERT] | [-10% to +15%] | [Data migration, integration] |
| **Expert Judgment** | [No historical data] | [-30% to +100%] | [Novel components, vendor estimates] |

### 2.2 Three-Point Estimation (PERT)

> Used for activities with high uncertainty.

**Formula:** `Expected = (Optistic + 4×Most Likely + Pessimistic) / 6`
**Standard Deviation:** `σ = (Pessimistic - Optimistic) / 6`

| Activity | Optimistic | Most Likely | Pessimistic | Expected | σ | Confidence |
|----------|-----------|-------------|------------|----------|-----|-----------|
| [Data Migration] | [5 days] | [10 days] | [20 days] | [11.7 days] | [2.5] | ±21% |
| [ERP Integration] | [10 days] | [15 days] | [30 days] | [16.7 days] | [3.3] | ±20% |
| [Performance Tuning] | [3 days] | [5 days] | [10 days] | [5.5 days] | [1.2] | ±21% |
| [UAT — dependent on users] | [7 days] | [10 days] | [20 days] | [11.5 days] | [2.2] | ±19% |

## 3. Duration Estimates — Basis

### 3.1 Requirements Phase

| Activity | Duration | Basis | Assumptions | Confidence |
|----------|----------|-------|-----------|-----------|
| [Requirements Elicitation] | [15 days] | [Analogous — similar project took 12 days; +3 for larger scope] | [Stakeholders available as planned] | ±20% |
| [Requirements Documentation] | [15 days] | [Bottom-up — SRS (7d) + BRD (5d) + RTM (3d)] | [BA experienced with format] | ±15% |
| [Requirements Review] | [3 days] | [Expert judgment — 2 review cycles × 1.5 days] | [Stakeholders attend reviews] | ±25% |

### 3.2 Development Phase

| Activity | Duration | Basis | Assumptions | Confidence |
|----------|----------|-------|-----------|-----------|
| [Sprint 1 — Portal Core] | [10 days (2 weeks)] | [Team velocity = 20 SP/sprint; Sprint 1 = 20 SP] | [Team at full capacity, no blockers] | ±15% |
| [Sprint 2 — Portal + Processing] | [10 days] | [Same velocity; 20 SP allocated] | [Sprint 1 complete, no carryover] | ±15% |
| [Sprint 3 — Processing + Admin] | [10 days] | [Same velocity; 20 SP allocated] | [Integration POC successful] | ±15% |
| [Sprint 4 — Admin + Notifications] | [10 days] | [Same velocity; 20 SP allocated] | [No critical defects from Sprint 3] | ±15% |
| [Sprint 5 — Dashboard + Polish] | [10 days] | [Same velocity; 20 SP allocated] | [All features from Sprint 1-4 working] | ±15% |

### 3.3 Testing Phase

| Activity | Duration | Basis | Assumptions | Confidence |
|----------|----------|-------|-----------|-----------|
| [System Testing] | [10 days] | [Parametric — 0.5 days per feature × 20 features] | [Test cases prepared in advance] | ±20% |
| [Integration Testing] | [5 days] | [Analogous — similar integrations took 4-6 days] | [Test environment stable] | ±25% |
| [Performance Testing] | [5 days] | [Expert judgment — 2 days setup + 3 days execution] | [Load test tools available] | ±30% |
| [UAT] | [10 days] | [Analogous — similar UAT took 8-12 days] | [Users available, test data ready] | ±30% |

## 4. Cost Estimates — Basis

### 4.1 Personnel Costs

| Role | Rate | Basis | Confidence |
|------|------|-------|-----------|
| [Project Manager] | $[X]/day | [Internal loaded rate — salary + benefits + overhead (1.4x)] | 🟢 High |
| [Business Analyst] | $[X]/day | [Internal loaded rate] | 🟢 High |
| [Technical Lead] | $[X]/day | [Internal loaded rate] | 🟢 High |
| [Senior Developer] | $[X]/day | [Internal loaded rate] | 🟢 High |
| [Junior Developer] | $[X]/day | [Internal loaded rate] | 🟢 High |
| [QA Lead] | $[X]/day | [Internal loaded rate] | 🟢 High |
| [Implementation Consultant] | $[X]/day | [Vendor quote — valid 90 days] | 🟢 High |
| [Security Consultant] | $[X]/day | [Vendor quote — valid 90 days] | 🟢 High |

### 4.2 Software & Licensing

| Item | Cost | Basis | Confidence |
|------|------|-------|-----------|
| [CRM Platform] | $[X]/year | [Vendor published pricing + 10% negotiation discount] | 🟢 High |
| [Email Service] | $[X]/year | [Vendor published pricing — usage-based estimate] | 🟡 Medium |
| [Monitoring Tool] | $[X]/year | [Vendor published pricing] | 🟢 High |
| [CI/CD Tool] | $[X]/year | [Vendor published pricing] | 🟢 High |

### 4.3 Infrastructure Costs

| Item | Cost | Basis | Confidence |
|------|------|-------|-----------|
| [Cloud Compute] | $[X]/month | [Parametric — $Y per instance × Z instances × usage hours] | 🟡 Medium |
| [Database] | $[X]/month | [Parametric — $Y per GB × estimated storage] | 🟡 Medium |
| [Storage] | $[X]/month | [Parametric — $Y per GB × estimated growth] | 🟡 Medium |
| [CDN] | $[X]/month | [Analogous — similar project spent $X/month] | 🟡 Medium |

## 5. Effort Estimates — Basis

### 5.1 Story Point Calibration

| Story | Points | Complexity | Uncertainty | Effort | Reference |
|-------|--------|-----------|------------|--------|-----------|
| [Simple CRUD form] | [2] | Low | Low | [1-2 days] | [Team baseline] |
| [Form with validation] | [3] | Medium | Low | [2-3 days] | [Team baseline] |
| [Workflow with business rules] | [5] | Medium | Medium | [3-5 days] | [Team baseline] |
| [Complex workflow with integrations] | [8] | High | Medium | [5-8 days] | [Team baseline] |
| [Dashboard with real-time data] | [8] | High | High | [5-8 days] | [Team baseline] |

### 5.2 Velocity Assumptions

| Assumption | Value | Basis | Risk if Wrong |
|-----------|-------|-------|--------------|
| [Team velocity] | [20 SP/sprint] | [Industry average for 3-developer team] | [Sprints extend or scope reduces] |
| [Sprint length] | [2 weeks] | [Team preference + Agile standard] | [N/A — team decision] |
| [Velocity ramp-up] | [Sprint 1 at 80%, Sprint 2 at 100%] | [New team needs ramp-up] | [Sprint 1 may carry over] |
| [Defect rate] | [5% of story points per sprint] | [Industry average] | [More rework, less new work] |

## 6. Vendor Estimates — Basis

| Vendor | Service | Estimate | Basis | Confidence | Validity |
|--------|---------|---------|-------|-----------|---------|
| [Vendor A] | [Implementation] | $[X] | [SOW — fixed price] | 🟢 High | [90 days] |
| [Vendor A] | [Data Migration] | $[X] | [SOW — fixed price based on 500K records] | 🟡 Medium | [90 days] |
| [Vendor B] | [Penetration Testing] | $[X] | [Vendor quote — 5 days × daily rate] | 🟢 High | [90 days] |
| [Vendor C] | [Training Development] | $[X] | [Vendor quote — fixed price] | 🟢 High | [90 days] |

## 7. Estimation Assumptions

### 7.1 Global Assumptions

| # | Assumption | Impact on Estimates | Confidence | Source |
|---|-----------|-------------------|-----------|--------|
| 1 | [Team available as per resource plan] | [If not — duration extends proportionally] | 🟡 Medium | [PM confirmation] |
| 2 | [No major scope changes after baseline] | [If scope grows — cost and duration grow] | 🟡 Medium | [Change control] |
| 3 | [Vendor prices stable for 90 days] | [If prices increase — cost overrun] | 🟢 High | [Vendor confirmation] |
| 4 | [Cloud costs based on projected usage] | [If usage higher — infrastructure cost increases] | 🟡 Medium | [Technical estimate] |
| 5 | [Team velocity of 20 SP/sprint] | [If lower — more sprints needed] | 🟡 Medium | [Industry benchmark] |
| 6 | [No public holidays during sprints] | [If holidays — sprint effective days reduce] | 🟢 High | [Calendar] |
| 7 | [Exchange rate: 1 USD = X THB] | [If rate changes — cost changes] | 🟡 Medium | [Current rate] |

### 7.2 Risk Contingency

| Risk | Probability | Impact | Contingency | Included In |
|------|-----------|--------|-----------|------------|
| [Vendor delay] | Medium | $[X] | $[Y] | [Contingency reserve] |
| [Data quality issues] | Medium | $[X] | $[Y] | [Contingency reserve] |
| [Scope creep] | High | $[X] | $[Y] | [Contingency reserve] |
| [Resource turnover] | Low | $[X] | $[Y] | [Contingency reserve] |
| **Total Contingency** | | | **$[Sum]** | **[15% of subtotal]** |

## 8. Estimate Confidence Summary

| Category | Estimate | Confidence | Range (Low-High) | Basis Quality |
|----------|---------|-----------|------------------|--------------|
| [Personnel] | $[X] | ±15% | $[Low - High] | 🟢 Good — internal rates known |
| [Software] | $[X] | ±5% | $[Low - High] | 🟢 Good — vendor pricing |
| [Infrastructure] | $[X] | ±20% | $[Low - High] | 🟡 Medium — usage estimates |
| [Vendor Services] | $[X] | ±10% | $[Low - High] | 🟢 Good — fixed-price SOWs |
| [Duration] | [154 days] | ±15% | [131 - 177 days] | 🟡 Medium — velocity assumption |
| [Effort] | [85 SP] | ±15% | [72 - 98 SP] | 🟡 Medium — team new to project |

## 9. Re-Estimation Triggers

| Trigger | When to Re-Estimate | Authority |
|---------|-------------------|----------|
| [Scope change >10%] | [At change approval] | [PM] |
| [Assumption invalidated] | [Immediately] | [PM] |
| [Sprint velocity deviates >20%] | [After Sprint 2] | [PM] |
| [Phase gate review] | [At each gate] | [PM + Sponsor] |
| [Vendor price change] | [Upon notification] | [PM + Procurement] |
| [Resource change] | [Upon change] | [PM] |

## 10. Estimation Lessons (To Be Updated)

| # | Lesson | Source | Impact on Future Estimates |
|---|--------|--------|--------------------------|
| 1 | [To be captured during project] | | |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Cost-Estimates]] | Cost figures supported by this basis |
| [[Cost-Baseline]] | Approved costs with this basis |
| [[Project-Schedule]] | Duration figures supported by this basis |
| [[Functional-Size-Measurement]] | Size measurement feeding estimates |
| [[Risk-Register]] | Risks driving contingency estimates |
| [[Resource-Requirements]] | Resource assumptions underlying estimates |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** This document answers "Why did we estimate X?" for every major estimate. When stakeholders challenge estimates, point them here. When assumptions change, re-estimate using this document as the baseline. Good estimation is transparent estimation.
