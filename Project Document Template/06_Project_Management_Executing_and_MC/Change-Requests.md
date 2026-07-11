---
document_type: Change Requests
version: "1.0"
status: Active
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [change-requests, change-control, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Executing (Integration Management)
  - ISO 21502 — Project Management Guidance
---

# Change Requests

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document provides the formal change request form and tracks all change requests submitted during the project.

## 2. Change Request Form Template

### CR-[XXX]: [Change Title]

| Field | Detail |
|-------|--------|
| **CR ID** | [CR-XXX] |
| **Date Submitted** | [YYYY-MM-DD] |
| **Submitter** | [Name / Role] |
| **Category** | [Scope / Schedule / Cost / Quality / Resource] |
| **Classification** | [Minor / Moderate / Major / Emergency] |

#### Change Description

| What is changing | [Clear, specific description] |
|-----------------|------------------------------|
| **Why** | [Business justification] |
| **Urgency** | [🔴 Immediate / 🟡 Next Sprint / 🟢 Next Phase / ⚪ Future] |

#### Current State (Before)

| Aspect | Current |
|--------|--------|
| [Scope] | [What exists today] |
| [Schedule] | [Current timeline] |
| [Cost] | [Current budget] |

#### Proposed State (After)

| Aspect | Proposed |
|--------|---------|
| [Scope] | [What would exist after change] |
| [Schedule] | [New timeline] |
| [Cost] | [New budget] |

#### Impact Analysis

| Dimension | Impact | Details |
|-----------|--------|---------|
| **Scope** | [🔴 High / 🟡 Medium / 🟢 Low / None] | [What must be added/removed/modified] |
| **Schedule** | [X days added / No impact] | [Which milestones affected] |
| **Cost** | [$X additional / No impact] | [Development, testing, infrastructure] |
| **Quality** | [🔴 High / 🟡 Medium / 🟢 Low / None] | [Effect on NFRs] |
| **Risk** | [🔴 High / 🟡 Medium / 🟢 Low / None] | [New risks introduced] |
| **Other Requirements** | [X requirements affected] | [Which reqs impacted] |
| **Test Cases** | [X tests affected] | [Which tests need updating] |

#### Alternatives Considered

| # | Alternative | Pros | Cons | Recommendation |
|---|-----------|------|------|---------------|
| 1 | [Approach A] | [Pros] | [Cons] | ⬜ |
| 2 | [Approach B] | [Pros] | [Cons] | ⬜ |

#### Recommendation

| Decision | Recommendation | Rationale |
|----------|---------------|-----------|
| **Approve** | [Recommended / Not Recommended] | [Why] |
| **Reject** | [With rationale] | [Why not] |
| **Defer** | [To which phase] | [Why defer] |

#### Decision

| Field | Detail |
|-------|--------|
| **Decision** | ✅ Approved / ❌ Rejected / ⏸️ Deferred |
| **Decision Date** | [YYYY-MM-DD] |
| **Authority** | [PM / CCB / Steering Committee] |
| **Conditions** | [Any conditions attached] |

## 3. Change Request Register

| CR ID | Date | Submitter | Title | Category | Classification | Impact Summary | Decision | Authority | Date |
|-------|------|----------|-------|----------|---------------|---------------|----------|----------|------|
| CR-001 | [YYYY-MM-DD] | [Name] | [Add save draft feature] | Scope | Minor | [+3 days, +$2K] | ✅ Approved | PM | [Date] |
| CR-002 | [YYYY-MM-DD] | [Name] | [Change response time target] | Quality | Minor | [+$5K (CDN)] | ✅ Approved | PM | [Date] |
| CR-003 | [YYYY-MM-DD] | [Name] | [Add ML anomaly detection] | Scope | Major | [+3 months, +$50K] | ⏸️ Deferred | Steering | [Date] |
| CR-004 | [YYYY-MM-DD] | [Name] | [Add bulk upload for corporate] | Scope | Moderate | [+5 days, +$3K] | ✅ Approved | CCB | [Date] |
| CR-005 | [YYYY-MM-DD] | [Name] | [Integrate with new payment provider] | Scope | Major | [+2 weeks, +$15K] | ❌ Rejected | Steering | [Date] |

## 4. Change Classification

| Level | Criteria | Authority | Processing Time |
|-------|---------|----------|----------------|
| **Minor** | Clarification, no cost/schedule impact | PM | Same day |
| **Moderate** | <10% budget, <2 weeks schedule | CCB | Within 1 week |
| **Major** | >10% budget, >2 weeks schedule | Steering Committee | Within 2 weeks |
| **Emergency** | Critical issue requiring immediate action | Sponsor (ratified later) | Immediate |

## 5. Change Control Board (CCB)

| Member | Role | Voting |
|--------|------|--------|
| [PM] | [Chair — presents changes] | ✅ |
| [BA Lead] | [Impact analysis] | ✅ |
| [Tech Lead] | [Technical feasibility] | ✅ |
| [Business Owner] | [Business impact] | ✅ |
| [Sponsor] | [Final authority for major changes] | ✅ |

### CCB Meeting Schedule

| Meeting | Frequency | Purpose |
|---------|-----------|---------|
| [CCB Review] | Bi-weekly | [Review pending change requests] |
| [Emergency CCB] | As needed | [Critical changes requiring immediate decision] |

## 6. Change Statistics

| Metric | Value | Target |
|--------|-------|--------|
| [Total CRs Submitted] | [X] | — |
| [Approved] | [X] ([Y%]) | — |
| [Rejected] | [X] ([Y%]) | — |
| [Deferred] | [X] ([Y%]) | — |
| [Pending] | [X] | [<5] |
| [Average Processing Time] | [X days] | [≤5 days] |
| [Requirements Stability Index] | [Y%] | [≥85%] |

## 7. Change Impact Summary

| Category | Approved Changes | Total Impact |
|----------|-----------------|-------------|
| [Features Added] | [X] | [Y story points] |
| [Schedule Impact] | [X changes] | [Y days total] |
| [Cost Impact] | [X changes] | [$Y total] |
| [Requirements Modified] | [X] | — |
| [Requirements Added] | [X] | — |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Change Log]] | Comprehensive list of all changes |
| [[Requirements Change Log]] | Requirements-specific changes |
| [[Requirements Change Assessment]] | Impact analysis template |
| [[Governance Approach]] | Change authority definitions |
| [[Configuration Management Plan]] | Version control for changes |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** Every change to a baselined artifact must go through this process. The form ensures consistent impact analysis. The register provides audit trail. The CCB ensures appropriate authority for decisions.
