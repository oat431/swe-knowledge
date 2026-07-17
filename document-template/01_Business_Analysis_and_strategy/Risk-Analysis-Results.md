---
document_type: Risk Analysis Results
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
ba_owner: "[Business Analyst Name]"
risk_owner: "[Risk Manager / PM]"
classification: "Internal / Confidential"
tags: [risk-analysis, risk-register, babok, iso-31000, strategy]
standard_ref:
  - BABOK v3 — Strategy Analysis
  - PMBOK v8 — Planning (Risk Management)
  - ISO 31000 — Risk Management
  - ISO/IEC/IEEE 15288 — System Life Cycle Processes
---

# Risk Analysis Results

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Archived]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Risk Manager | [Name / Role] |
| Business Analyst | [Name / Role] |
| Sponsor | [Name / Role] |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial draft |
| 1.0 | [YYYY-MM-DD] | [Name] | Approved version |

### Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Sponsor | | | |
| Risk Manager | | | |
| Business Owner | | | |
| BA Lead | | | |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Risk Analysis Method](#2-risk-analysis-method)
3. [Business Risks](#3-business-risks)
4. [Technical Risks](#4-technical-risks)
5. [Financial Risks](#5-financial-risks)
6. [Organizational Risks](#6-organizational-risks)
7. [External Risks](#7-external-risks)
8. [Risk Heat Map](#8-risk-heat-map)
9. [Risk Response Strategies](#9-risk-response-strategies)
10. [Risk Monitoring Plan](#10-risk-monitoring-plan)
11. [Summary](#11-summary)

---

## 1. Executive Summary

| Field | Detail |
|-------|--------|
| Total Risks Identified | [X] |
| 🔴 Critical Risks | [X] — require immediate action |
| 🟠 High Risks | [X] — mitigation plan required |
| 🟡 Medium Risks | [X] — monitor and manage |
| 🟢 Low Risks | [X] — accept and monitor |
| Analysis Date | [YYYY-MM-DD] |
| Next Review Date | [YYYY-MM-DD] |
| Overall Risk Exposure | $[X] (probability-weighted) |

### Top 5 Risks

| # | Risk | Category | Level | Impact if Realized |
|---|------|----------|-------|-------------------|
| 1 | [e.g., User adoption failure] | Organizational | 🔴 | [$X lost benefits, project failure] |
| 2 | [e.g., Data migration quality] | Technical | 🟠 | [Delayed go-live, rework cost] |
| 3 | [e.g., Vendor delivery delays] | External | 🟠 | [Schedule overrun, cost increase] |
| 4 | [e.g., Scope creep] | Business | 🟠 | [Budget overrun, timeline slip] |
| 5 | [e.g., Key resource departure] | Organizational | 🟡 | [Knowledge loss, delays] |

---

## 2. Risk Analysis Method

### 2.1 Approach

> Risks were identified through stakeholder interviews, workshops, historical data review, and expert judgment. Each risk was assessed for probability and impact using the organization's standard risk framework.

### 2.2 Risk Assessment Scale

**Probability Scale**

| Level | Rating | Description | Likelihood |
|-------|--------|-------------|-----------|
| 5 | Almost Certain | Expected to occur | >80% |
| 4 | Likely | Probable occurrence | 60-80% |
| 3 | Possible | Could occur | 40-60% |
| 2 | Unlikely | Improbable | 20-40% |
| 1 | Rare | Highly improbable | <20% |

**Impact Scale**

| Level | Rating | Cost Impact | Schedule Impact | Quality Impact |
|-------|--------|------------|----------------|---------------|
| 5 | Critical | >$500K | >6 months | Project failure |
| 4 | Major | $200K-$500K | 3-6 months | Significant rework |
| 3 | Moderate | $50K-$200K | 1-3 months | Partial rework |
| 2 | Minor | $10K-$50K | <1 month | Minor adjustments |
| 1 | Insignificant | <$10K | <1 week | Negligible |

### 2.3 Risk Categories

| Category | Description | Owner |
|----------|-------------|-------|
| 🏢 **Business** | Scope, requirements, stakeholder, market risks | BA |
| ⚙️ **Technical** | Technology, integration, data, security risks | Tech Lead |
| 💰 **Financial** | Budget, funding, cost overrun risks | Finance |
| 👥 **Organizational** | People, change, culture, capacity risks | Change Manager |
| 🌍 **External** | Vendor, regulatory, market, competitive risks | PM |

---

## 3. Business Risks

| ID | Risk | Description | Probability | Impact | Level | Response | Owner |
|----|------|-------------|------------|--------|-------|----------|-------|
| BR-01 | [Scope creep] | [Uncontrolled expansion of requirements beyond agreed scope] | 4 — Likely | 4 — Major | 🟠 | Mitigate — Strict change control, MoSCoW | PM |
| BR-02 | [Requirements ambiguity] | [Unclear or conflicting requirements leading to rework] | 3 — Possible | 3 — Moderate | 🟡 | Mitigate — Reviews, prototypes, validation | BA |
| BR-03 | [Stakeholder disengagement] | [Key stakeholders withdraw support or become unavailable] | 2 — Unlikely | 4 — Major | 🟡 | Mitigate — Regular engagement, escalation | PM |
| BR-04 | [Market shift] | [Business needs change due to market conditions] | 2 — Unlikely | 3 — Moderate | 🟢 | Accept — Monitor, agile response | Sponsor |
| BR-05 | | | | | | | |

---

## 4. Technical Risks

| ID | Risk | Description | Probability | Impact | Level | Response | Owner |
|----|------|-------------|------------|--------|-------|----------|-------|
| TR-01 | [Legacy integration failure] | [New system unable to integrate with legacy ERP] | 3 — Possible | 5 — Critical | 🔴 | Mitigate — Early POC, phased integration | Tech Lead |
| TR-02 | [Data migration quality] | [Corrupted or incomplete data during migration] | 3 — Possible | 4 — Major | 🟠 | Mitigate — Parallel run, extensive validation | Data Architect |
| TR-03 | [Performance below target] | [System cannot meet performance NFRs under load] | 2 — Unlikely | 4 — Major | 🟡 | Mitigate — Load testing, architecture review | Tech Lead |
| TR-04 | [Security vulnerability] | [Undetected security flaw in new system] | 2 — Unlikely | 5 — Critical | 🟠 | Mitigate — Penetration testing, SAST/DAST | Security Lead |
| TR-05 | [Technology obsolescence] | [Chosen technology becomes unsupported] | 1 — Rare | 3 — Moderate | 🟢 | Accept — Monitor vendor roadmap | Architect |
| TR-06 | | | | | | | |

---

## 5. Financial Risks

| ID | Risk | Description | Probability | Impact | Level | Response | Owner |
|----|------|-------------|------------|--------|-------|----------|-------|
| FR-01 | [Budget overrun] | [Actual costs exceed approved budget by >15%] | 3 — Possible | 4 — Major | 🟠 | Mitigate — Contingency reserve, monthly tracking | PM |
| FR-02 | [Funding withdrawal] | [Organizational funding cut mid-project] | 2 — Unlikely | 5 — Critical | 🟠 | Transfer — Contractual protections, phased funding | Sponsor |
| FR-03 | [Vendor cost escalation] | [Vendor increases prices during implementation] | 2 — Unlikely | 3 — Moderate | 🟡 | Transfer — Fixed-price contracts, penalty clauses | Procurement |
| FR-04 | [Benefits not realized] | [Projected value fails to materialize] | 3 — Possible | 4 — Major | 🟠 | Mitigate — Benefits tracking, early validation | BA |
| FR-05 | | | | | | | |

---

## 6. Organizational Risks

| ID | Risk | Description | Probability | Impact | Level | Response | Owner |
|----|------|-------------|------------|--------|-------|----------|-------|
| OR-01 | [User adoption failure] | [Staff resist new system, continue old processes] | 4 — Likely | 5 — Critical | 🔴 | Mitigate — Change mgmt, training, champions | Change Mgr |
| OR-02 | [Key resource departure] | [Critical team member leaves during project] | 3 — Possible | 4 — Major | 🟠 | Mitigate — Knowledge transfer, documentation | PM |
| OR-03 | [Skills gap] | [Team lacks skills for new technology] | 3 — Possible | 3 — Moderate | 🟡 | Mitigate — Training, vendor support, hiring | PM |
| OR-04 | [Change fatigue] | [Organization exhausted from prior changes] | 3 — Possible | 3 — Moderate | 🟡 | Mitigate — Phased approach, celebrate wins | Change Mgr |
| OR-05 | [Competing priorities] | [Key resources pulled to other initiatives] | 3 — Possible | 3 — Moderate | 🟡 | Mitigate — Resource agreements, escalation | PM |
| OR-06 | | | | | | | |

---

## 7. External Risks

| ID | Risk | Description | Probability | Impact | Level | Response | Owner |
|----|------|-------------|------------|--------|-------|----------|-------|
| ER-01 | [Vendor delivery delays] | [Vendor fails to deliver on committed timeline] | 3 — Possible | 4 — Major | 🟠 | Mitigate — SLAs, penalty clauses, alternatives | PM |
| ER-02 | [Regulatory change] | [New regulation requires scope change] | 2 — Unlikely | 4 — Major | 🟡 | Accept — Monitor, contingency plan | Compliance |
| ER-03 | [Third-party service outage] | [Cloud provider or API experiences extended outage] | 2 — Unlikely | 3 — Moderate | 🟡 | Mitigate — SLA, redundancy, fallback | Tech Lead |
| ER-04 | [Economic downturn] | [Budget cuts due to economic conditions] | 2 — Unlikely | 4 — Major | 🟡 | Accept — Phased delivery, core-first approach | Sponsor |
| ER-05 | | | | | | | |

---

## 8. Risk Heat Map

| Impact \ Probability | 1 — Rare | 2 — Unlikely | 3 — Possible | 4 — Likely | 5 — Almost Certain |
|---------------------|----------|-------------|-------------|-----------|-------------------|
| **5 — Critical** | 🟡 | 🟠 FR-02 | 🟠 TR-02 | 🔴 OR-01 | 🔴 |
| **4 — Major** | 🟢 | 🟡 ER-02, ER-04 | 🟠 FR-01, FR-04, OR-02, ER-01 | 🟠 BR-01 | 🔴 |
| **3 — Moderate** | 🟢 | 🟢 BR-04 | 🟡 BR-02, OR-03, OR-04, OR-05 | 🟡 | 🟠 |
| **2 — Minor** | 🟢 | 🟢 | 🟡 | 🟡 | 🟡 |
| **1 — Insignificant** | 🟢 | 🟢 | 🟢 | 🟢 | 🟡 |

> **Legend:** 🔴 Critical — Immediate action required | 🟠 High — Mitigation plan required | 🟡 Medium — Monitor and manage | 🟢 Low — Accept and monitor

---

## 9. Risk Response Strategies

### 9.1 Response Strategy Summary

| Strategy | Count | Risks |
|----------|-------|-------|
| **Mitigate** (reduce probability or impact) | [X] | TR-01, TR-02, TR-03, TR-04, FR-01, FR-04, OR-01, OR-02, OR-03, ER-01 |
| **Transfer** (shift to third party) | [X] | FR-02, FR-03 |
| **Accept** (acknowledge, monitor) | [X] | BR-04, TR-05, ER-02, ER-03, ER-04 |
| **Avoid** (eliminate the cause) | [X] | — |
| **Escalate** (to higher authority) | [X] | — |

### 9.2 Detailed Response Plans

> **Repeat for each 🟠/🔴 risk.**

#### OR-01: User Adoption Failure

| Field | Detail |
|-------|--------|
| **Risk** | Staff resist new system, continue old processes |
| **Response Strategy** | Mitigate |
| **Response Actions** | 1. Change management program from Day 1<br>2. Identify and train champions in each team<br>3. Involve end users in design workshops<br>4. Phased rollout with early wins<br>5. Executive sponsorship and visible support |
| **Trigger** | [Adoption metrics below X% at any checkpoint] |
| **Contingency** | [If adoption <50% at go-live, extend parallel run + additional training] |
| **Cost of Response** | $[X] |
| **Owner** | Change Manager |
| **Due Date** | [Ongoing — from project start] |

#### TR-01: Legacy Integration Failure

| Field | Detail |
|-------|--------|
| **Risk** | New system unable to integrate with legacy ERP |
| **Response Strategy** | Mitigate |
| **Response Actions** | 1. Integration POC in Phase 1, Week 2<br>2. Engage ERP vendor for API support<br>3. Design fallback — batch file transfer<br>4. Allocate buffer in integration timeline |
| **Trigger** | [POC fails to achieve data exchange] |
| **Contingency** | [Switch to batch file integration — manual sync as last resort] |
| **Cost of Response** | $[X] |
| **Owner** | Technical Lead |
| **Due Date** | [Phase 1, Week 2] |

---

## 10. Risk Monitoring Plan

### 10.1 Monitoring Activities

| Activity | Frequency | Participants | Output |
|----------|-----------|-------------|--------|
| Risk Review Meeting | Bi-weekly | PM, BA, Tech Lead, Change Mgr | Updated risk register |
| Risk Dashboard Update | Weekly | PM | Status report to steering committee |
| Trigger Monitoring | Continuous | Risk Owners | Early warning alerts |
| Risk Retrospective | Monthly | Full team | Lessons learned, new risks |

### 10.2 Risk Dashboard

| Metric | Current | Target | Status |
|--------|---------|--------|--------|
| Total Open Risks | [X] | [<Y] | 🟢/🟡/🔴 |
| 🔴 Critical Risks | [X] | [0] | 🟢/🟡/🔴 |
| 🟠 High Risks | [X] | [<3] | 🟢/🟡/🔴 |
| Risks with Mitigation Plan | [X%] | [100%] | 🟢/🟡/🔴 |
| Overdue Risk Actions | [X] | [0] | 🟢/🟡/🔴 |
| Risk Trend (30 days) | [↑ Increasing / → Stable / ↓ Decreasing] | ↓ | |

### 10.3 Escalation Matrix

| Risk Level | Escalation To | Timeline | Communication |
|-----------|--------------|----------|---------------|
| 🔴 Critical | Steering Committee | Within 24 hours | Emergency meeting |
| 🟠 High | Sponsor | Within 48 hours | Email + meeting |
| 🟡 Medium | PM | Weekly review | Risk register update |
| 🟢 Low | Risk Owner | Monthly review | Dashboard update |

---

## 11. Summary

### Risk Profile

| Category | 🔴 Critical | 🟠 High | 🟡 Medium | 🟢 Low | Total |
|----------|------------|--------|----------|-------|-------|
| Business | 0 | 1 | 2 | 1 | 4 |
| Technical | 1 | 2 | 1 | 1 | 5 |
| Financial | 0 | 3 | 1 | 0 | 4 |
| Organizational | 1 | 1 | 3 | 0 | 5 |
| External | 0 | 1 | 3 | 0 | 4 |
| **Total** | **2** | **8** | **10** | **2** | **22** |

### Key Findings

1. **Organizational risk is highest** — User adoption (OR-01) is the #1 risk requiring dedicated change management
2. **Technical risks are manageable** — Early POC and phased integration mitigate the critical integration risk
3. **Financial risks need monitoring** — Benefits realization (FR-04) must be tracked from Phase 1
4. **External risks are mostly acceptable** — Vendor SLAs and regulatory monitoring provide adequate coverage

### Risk-Based Recommendations

1. **Invest in change management** — OR-01 justifies dedicated change management budget
2. **Integration POC first** — TR-01 must be validated before committing to full integration
3. **Contingency reserve** — 15% budget contingency recommended for FR-01
4. **Phased approach** — Reduces exposure to multiple risks simultaneously

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Business-Case]] | Risks inform the Business Case risk section |
| [[Change-Strategy]] | Risk responses shape the implementation approach |
| [[Solution-Scope]] | Scope decisions mitigate some risks |
| [[Gap-Analysis]] | Unresolved gaps become risks |
| [[Risk-Management-Plan]] | Detailed risk management process and governance |
| [[Risk-Register]] | Operational risk tracking (living document) |

---

> **Template Standard:** Based on BABOK v3 (Strategy Analysis), PMBOK v8 (Risk Management), ISO 31000
> **Usage:** This is a point-in-time analysis. The [[Risk-Register]] is the living document for ongoing risk management. Update this analysis at major milestones or when the risk landscape changes significantly.
