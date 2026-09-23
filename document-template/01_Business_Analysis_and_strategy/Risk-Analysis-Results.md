---
document_type: Risk Analysis Results
schema_version: 2
canonical_name: Risk Analysis Results
doc_form: record
applicability: evidence
min_project_tier: 4  # Small-Prod
tier_trigger: Produced by the activity it records
minimum_form: Tracker entries with links to decisions
overlap_group: risk
source_of_truth: 05_Project_Management_Planning/Risk-Management-Plan.md
overlap_note: BABOK analysis view
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
| BR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| BR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| BR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| BR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| BR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |

---

## 4. Technical Risks

| ID | Risk | Description | Probability | Impact | Level | Response | Owner |
|----|------|-------------|------------|--------|-------|----------|-------|
| TR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| TR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| TR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| TR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| TR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| TR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |

---

## 5. Financial Risks

| ID | Risk | Description | Probability | Impact | Level | Response | Owner |
|----|------|-------------|------------|--------|-------|----------|-------|
| FR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| FR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| FR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| FR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| FR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |

---

## 6. Organizational Risks

| ID | Risk | Description | Probability | Impact | Level | Response | Owner |
|----|------|-------------|------------|--------|-------|----------|-------|
| OR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| OR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| OR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| OR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| OR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| OR-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |

---

## 7. External Risks

| ID | Risk | Description | Probability | Impact | Level | Response | Owner |
|----|------|-------------|------------|--------|-------|----------|-------|
| ER-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| ER-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| ER-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| ER-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |
| ER-[XX] | [Risk name] | [Risk description] | [1-5 — Rating] | [1-5 — Rating] | [🔴/🟠/🟡/🟢] | [Response — actions] | [Role] |

---

## 8. Risk Heat Map

| Impact \ Probability | 1 — Rare | 2 — Unlikely | 3 — Possible | 4 — Likely | 5 — Almost Certain |
|---------------------|----------|-------------|-------------|-----------|-------------------|
| **5 — Critical** | 🟡 | 🟠 FR-[XX] | 🟠 TR-[XX] | 🔴 OR-[XX] | 🔴 |
| **4 — Major** | 🟢 | 🟡 ER-[XX], ER-[XX] | 🟠 FR-[XX], FR-[XX], OR-[XX], ER-[XX] | 🟠 BR-[XX] | 🔴 |
| **3 — Moderate** | 🟢 | 🟢 BR-[XX] | 🟡 BR-[XX], OR-[XX], OR-[XX], OR-[XX] | 🟡 | 🟠 |
| **2 — Minor** | 🟢 | 🟢 | 🟡 | 🟡 | 🟡 |
| **1 — Insignificant** | 🟢 | 🟢 | 🟢 | 🟢 | 🟡 |

> **Legend:** 🔴 Critical — Immediate action required | 🟠 High — Mitigation plan required | 🟡 Medium — Monitor and manage | 🟢 Low — Accept and monitor

---

## 9. Risk Response Strategies

### 9.1 Response Strategy Summary

| Strategy | Count | Risks |
|----------|-------|-------|
| **Mitigate** (reduce probability or impact) | [X] | TR-[XX], TR-[XX], TR-[XX], TR-[XX], FR-[XX], FR-[XX], OR-[XX], OR-[XX], OR-[XX], ER-[XX] |
| **Transfer** (shift to third party) | [X] | FR-[XX], FR-[XX] |
| **Accept** (acknowledge, monitor) | [X] | BR-[XX], TR-[XX], ER-[XX], ER-[XX], ER-[XX] |
| **Avoid** (eliminate the cause) | [X] | — |
| **Escalate** (to higher authority) | [X] | — |

### 9.2 Detailed Response Plans

> **Repeat for each 🟠/🔴 risk.**

#### OR-[XX]: [Risk Name]

| Field | Detail |
|-------|--------|
| **Risk** | [Risk description] |
| **Response Strategy** | [Mitigate / Transfer / Accept / Avoid / Escalate] |
| **Response Actions** | [Numbered response actions] |
| **Trigger** | [Condition that activates the response] |
| **Contingency** | [Fallback if trigger fires] |
| **Cost of Response** | $[X] |
| **Owner** | [Role] |
| **Due Date** | [Date / phase] |

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
| Business | [N] | [N] | [N] | [N] | [N] |
| Technical | [N] | [N] | [N] | [N] | [N] |
| Financial | [N] | [N] | [N] | [N] | [N] |
| Organizational | [N] | [N] | [N] | [N] | [N] |
| External | [N] | [N] | [N] | [N] | [N] |
| **Total** | **[N]** | **[N]** | **[N]** | **[N]** | **[N]** |

### Key Findings

1. **[Finding theme]** — [Finding detail with risk ID references]
2. **[Finding theme]** — [Finding detail]
3. **[Finding theme]** — [Finding detail]
4. **[Finding theme]** — [Finding detail]

### Risk-Based Recommendations

1. **[Recommendation]** — [Rationale and linked risk IDs]
2. **[Recommendation]** — [Rationale]
3. **[Recommendation]** — [Rationale]
4. **[Recommendation]** — [Rationale]

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
