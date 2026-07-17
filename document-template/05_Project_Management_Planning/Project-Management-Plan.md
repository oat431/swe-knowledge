---
document_type: Project Management Plan
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [project-management-plan, master-plan, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Planning (Integration Management)
  - ISO 21502 — Project Management Guidance
---

# Project Management Plan

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Project Manager | [Name / Role] |
| Sponsor | [Name / Role] |

### Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Sponsor | | | |
| Project Manager | | | |
| Business Owner | | | |
| IT Director | | | |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial draft |
| 1.0 | [YYYY-MM-DD] | [Name] | Baselined version |

---

## 1. Executive Summary

| Field | Detail |
|-------|--------|
| Project Name | [Name] |
| Project ID | [ID] |
| Sponsor | [Name] |
| PM | [Name] |
| Start Date | [YYYY-MM-DD] |
| End Date | [YYYY-MM-DD] |
| Budget | $[X] |
| Methodology | [Hybrid — Agile + formal gates] |

## 2. Subsidiary Plans

> This plan consolidates the following subsidiary plans. Each is maintained as a separate document.

| # | Subsidiary Plan | Document | Status |
|---|----------------|---------|--------|
| 1 | Scope Management Plan | [[Scope-Management-Plan]] | Draft |
| 2 | Schedule Management Plan | [[Schedule-Management-Plan]] | Draft |
| 3 | Financial Management Plan | [[Financial-Management-Plan]] | Draft |
| 4 | Resource Management Plan | [[Resource-Management-Plan]] | Draft |
| 5 | Risk Management Plan | [[Risk-Management-Plan]] | Draft |
| 6 | Quality Management Plan | [[Quality-Management-Plan]] | Draft |
| 7 | Communications Management Plan | [[Communications-Management-Plan]] | Draft |
| 8 | Stakeholder Engagement Plan | [[Stakeholder-Engagement-Plan]] | Draft |
| 9 | Change Management Plan | [[Change-Strategy]] | Draft |
| 10 | Configuration Management Plan | [[Change-Requests]] | Draft |
| 11 | Procurement Management Plan | [[Procurement-Management-Plan]] | Draft |

## 3. Project Lifecycle

### 3.1 Development Approach

| Aspect | Approach | Rationale |
|--------|---------|-----------|
| **Overall** | [Hybrid — Agile delivery + formal gates] | [Regulatory requires baselines; team is Agile-capable] |
| **Requirements** | [Agile — iterative refinement] | [Requirements will evolve] |
| **Design** | [Formal — architecture review] | [Technical decisions need governance] |
| **Development** | [Agile — 2-week sprints] | [Fast feedback, incremental delivery] |
| **Testing** | [Formal — test plan, test cases] | [Quality gates for go-live] |
| **Deployment** | [Formal — change control] | [Production changes need approval] |

### 3.2 Project Phases

```mermaid
gantt
    title Project Phases
    dateFormat YYYY-MM-DD
    section Phase 1 - Initiation
    Project Charter        :a1, 2026-08-01, 7d
    Kickoff Meeting        :a2, after a1, 1d
    section Phase 2 - Planning
    Requirements           :b1, after a2, 28d
    Design                 :b2, after b1, 21d
    section Phase 3 - Execution
    Sprint 1-2 (Core)      :c1, after b2, 28d
    Sprint 3-4 (Features)  :c2, after c1, 28d
    Sprint 5 (Polish)      :c3, after c2, 14d
    section Phase 4 - Testing
    System Testing         :d1, after c3, 14d
    UAT                    :d2, after d1, 14d
    section Phase 5 - Closure
    Go-Live                :e1, after d2, 1d
    Hypercare              :e2, after e1, 30d
    Closure                :e3, after e2, 7d
```

### 3.3 Phase Gates

| Gate | Criteria | Authority | Date |
|------|---------|----------|------|
| **Gate 1** — Requirements | [Requirements baselined, stakeholder sign-off] | [Sponsor] | [YYYY-MM-DD] |
| **Gate 2** — Design | [Architecture approved, design reviewed] | [Sponsor + IT Director] | [YYYY-MM-DD] |
| **Gate 3** — Development | [All features complete, code reviewed, unit tested] | [PM + Tech Lead] | [YYYY-MM-DD] |
| **Gate 4** — Testing | [System test passed, UAT passed, zero critical defects] | [Sponsor + Business Owner] | [YYYY-MM-DD] |
| **Gate 5** — Go-Live | [Readiness assessment passed, rollback plan tested] | [Sponsor] | [YYYY-MM-DD] |
| **Gate 6** — Closure | [Benefits tracking started, lessons learned captured] | [Sponsor] | [YYYY-MM-DD] |

## 4. Project Organization

### 4.1 Team Structure

```mermaid
flowchart TD
    SPONSOR[Sponsor] --> PM[Project Manager]
    PM --> BA[Business Analyst]
    PM --> TL[Technical Lead]
    PM --> QA[QA Lead]
    PM --> CM[Change Manager]
    TL --> DEV1[Developer 1]
    TL --> DEV2[Developer 2]
    TL --> DEV3[Developer 3]
    QA --> QA1[QA Engineer]
    BA --> SME[Subject Matter Experts]

    style SPONSOR fill:#1a237e,color:#fff
    style PM fill:#283593,color:#fff
    style BA fill:#4CAF50,color:#fff
    style TL fill:#2196F3,color:#fff
    style QA fill:#FF9800,color:#fff
    style CM fill:#9C27B0,color:#fff
```

### 4.2 RACI Matrix

| Activity | Sponsor | PM | BA | TL | QA | Dev |
|----------|---------|-----|-----|-----|-----|-----|
| Project Authorization | **A** | R | I | I | I | I |
| Requirements | I | C | **A** | C | C | I |
| Design | I | C | C | **A** | C | R |
| Development | I | C | C | C | I | **A** |
| Testing | I | C | C | C | **A** | C |
| Go-Live Decision | **A** | R | C | C | C | I |
| Change Control | **A** | R | C | C | C | I |

## 5. Key Performance Indicators

| KPI | Target | Measurement | Frequency |
|-----|--------|-------------|-----------|
| [Schedule Performance Index (SPI)] | [≥0.95] | [Earned Value] | [Weekly] |
| [Cost Performance Index (CPI)] | [≥0.95] | [Earned Value] | [Weekly] |
| [Scope Completion] | [100%] | [Sprint velocity] | [Per sprint] |
| [Defect Density] | [<X per feature] | [Defect report] | [Per sprint] |
| [Stakeholder Satisfaction] | [≥4/5] | [Survey] | [Monthly] |

## 6. Communication Matrix

| Communication | Audience | Frequency | Channel | Owner |
|--------------|----------|-----------|---------|-------|
| [Daily Standup] | [Project team] | Daily | [Slack/Teams] | [PM] |
| [Sprint Review] | [All stakeholders] | Bi-weekly | [Demo + meeting] | [PM] |
| [Status Report] | [Sponsor, stakeholders] | Weekly | [Email] | [PM] |
| [Steering Committee] | [Leadership] | Monthly | [Meeting] | [PM] |
| [Risk Review] | [PM, BA, TL] | Bi-weekly | [Meeting] | [PM] |

## 7. Change Control

| Level | Definition | Authority | Timeline |
|-------|-----------|----------|----------|
| **Minor** | Clarification, no impact | PM | Same day |
| **Moderate** | <10% budget, <2 weeks schedule | CCB | Within 1 week |
| **Major** | >10% budget, >2 weeks schedule | Steering Committee | Within 2 weeks |
| **Emergency** | Critical issue | Sponsor (ratified later) | Immediate |

## 8. Baselines

| Baseline | Version | Date | Approved By |
|----------|---------|------|------------|
| [Scope Baseline] | v1.0 | [YYYY-MM-DD] | [Sponsor] |
| [Schedule Baseline] | v1.0 | [YYYY-MM-DD] | [Sponsor] |
| [Cost Baseline] | v1.0 | [YYYY-MM-DD] | [Sponsor] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Project-Charter]] | Authorization source |
| [[WBS-WBS-Dictionary]] | Scope decomposition |
| [[Project-Schedule]] | Timeline detail |
| [[Risk-Register]] | Risk management detail |
| [[Change-Log]] | Change control tracking |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** This is the *master plan* — it integrates all subsidiary plans. Keep it at the right level of detail; specifics belong in subsidiary plans. Update when baselines change.
