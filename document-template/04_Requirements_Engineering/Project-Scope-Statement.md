---
document_type: Project Scope Statement
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
ba_owner: "[Business Analyst Name]"
classification: "Internal / Confidential"
tags: [project-scope, scope-statement, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Planning (Scope Management)
  - ISO 21502 — Project Management Guidance
---

# Project Scope Statement

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Project Manager | [Name / Role] |
| Business Analyst | [Name / Role] |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial draft |
| 1.0 | [YYYY-MM-DD] | [Name] | Baselined version |

### Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Sponsor | | | |
| Business Owner | | | |
| Project Manager | | | |

---

## 1. Project Description

| Field | Detail |
|-------|--------|
| Project Name | [Name] |
| Project ID | [ID] |
| Sponsor | [Name / Role] |
| Project Manager | [Name / Role] |
| Business Analyst | [Name / Role] |
| Start Date | [YYYY-MM-DD] |
| Target Completion | [YYYY-MM-DD] |
| Estimated Budget | $[X] |

### 1.1 Project Purpose

> [Why this project exists — link to business case]

### 1.2 Project Description

> [What the project will deliver — high-level description]

## 2. Scope Description

### 2.1 In Scope

| # | Scope Item | Description | Deliverable |
|---|-----------|-------------|------------|
| 1 | [e.g., Customer Portal] | [Web + mobile responsive portal for request submission] | Portal application |
| 2 | [e.g., Admin Portal] | [Web portal for operations staff] | Admin application |
| 3 | [e.g., Processing Engine] | [Automated validation, routing, approval] | Backend service |
| 4 | [e.g., Notification Service] | [Email + in-app notifications] | Notification service |
| 5 | [e.g., Reporting Dashboard] | [Real-time operational dashboards] | Dashboard application |
| 6 | [e.g., API Integration] | [ERP, payment gateway connectivity] | Integration layer |
| 7 | [e.g., Data Migration] | [Migrate 500K customer records] | Migration scripts + data |
| 8 | [e.g., Training] | [User training materials and sessions] | Training package |

### 2.2 Out of Scope

| # | Item | Reason | Future Phase |
|---|------|--------|-------------|
| 1 | [e.g., ERP replacement] | [Separate initiative] | TBD |
| 2 | [e.g., International support] | [Domestic first] | Phase 2 |
| 3 | [e.g., Mobile native app] | [Responsive web sufficient] | Phase 3 |
| 4 | [e.g., Advanced analytics/AI] | [Foundation first] | Phase 3 |
| 5 | [e.g., Legacy system decommission] | [Post-migration stability] | Phase 2 |

## 3. Deliverables

| # | Deliverable | Description | Acceptance Criteria | Due Date |
|---|------------|-------------|-------------------|----------|
| D-01 | [Customer Portal] | [Web + mobile responsive portal] | [All portal FRs verified, UAT passed] | [YYYY-MM-DD] |
| D-02 | [Admin Portal] | [Operations management portal] | [All admin FRs verified, UAT passed] | [YYYY-MM-DD] |
| D-03 | [Processing Engine] | [Backend automation service] | [All workflow FRs verified, perf tested] | [YYYY-MM-DD] |
| D-04 | [API Integrations] | [ERP + payment + email connectors] | [Integration tests passed] | [YYYY-MM-DD] |
| D-05 | [Data Migration] | [Migrated and validated data] | [100% record reconciliation] | [YYYY-MM-DD] |
| D-06 | [Training Package] | [Materials + sessions] | [90% training completion] | [YYYY-MM-DD] |
| D-07 | [Documentation] | [User guides, admin guides, runbooks] | [Reviewed and approved] | [YYYY-MM-DD] |

## 4. Acceptance Criteria

| # | Criterion | Verification Method |
|---|----------|-------------------|
| 1 | [All 🔴 functional requirements verified] | [Test execution — 100% pass] |
| 2 | [All 🔴 non-functional requirements met] | [Performance, security, usability tests] |
| 3 | [Data migration — 100% reconciliation] | [Record count + data validation] |
| 4 | [UAT sign-off from Business Owner] | [Formal sign-off document] |
| 5 | [Zero critical defects open] | [Defect report] |
| 6 | [Training completion ≥90%] | [Training records] |
| 7 | [Operations runbook approved] | [IT sign-off] |

## 5. Constraints

| ID | Constraint | Type | Impact |
|----|-----------|------|--------|
| C-01 | [Must use existing cloud provider] | Technical | [Platform limitation] |
| C-02 | [Data sovereignty — in-country only] | Legal | [Hosting constraint] |
| C-03 | [Budget cap $500K] | Financial | [Scope limitation] |
| C-04 | [Go-live by YYYY-MM-DD] | Time | [Schedule pressure] |
| C-05 | [No new headcount] | Resource | [Current team must absorb] |

## 6. Assumptions

| ID | Assumption | Impact if Invalid |
|----|-----------|-------------------|
| A-01 | [ERP API remains available] | [Integration rework] |
| A-02 | [Key staff available as planned] | [Schedule delays] |
| A-03 | [No major regulatory changes] | [Scope change] |
| A-04 | [Data quality is sufficient for migration] | [Extended cleansing effort] |
| A-05 | [Vendor delivers on committed timeline] | [Schedule dependency] |

## 7. Risks

| ID | Risk | Probability | Impact | Level | Mitigation |
|----|------|------------|--------|-------|-----------|
| R-01 | [User adoption failure] | High | High | 🔴 | [Change management program] |
| R-02 | [Data migration quality] | Medium | High | 🟠 | [Data profiling, parallel run] |
| R-03 | [Integration complexity] | Medium | Medium | 🟡 | [Early POC] |
| R-04 | [Vendor delivery delays] | Medium | Medium | 🟡 | [SLA penalties] |
| R-05 | [Scope creep] | High | Medium | 🟠 | [Strict change control] |

## 8. Scope Exclusions (Explicit)

> These items are explicitly **not** part of this project:

| # | Exclusion | Why Not |
|---|----------|---------|
| 1 | [Changes to existing ERP system] | [Out of scope — separate initiative] |
| 2 | [Support for non-standard browsers] | [Only latest 2 versions of major browsers] |
| 3 | [Offline functionality] | [Internet connection required] |
| 4 | [Custom reporting beyond standard] | [Ad-hoc reporting via export only] |
| 5 | [24/7 support] | [Business hours only for Phase 1] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Business-Case]] | Justification for this scope |
| [[Solution-Scope]] | Detailed solution boundaries |
| [[WBS-WBS-Dictionary]] | Work breakdown decomposes this scope |
| [[Requirements-Traceability-Matrix]] | Requirements within this scope |
| [[Change-Log]] | Changes to this baseline |

---

> **Template Standard:** Based on PMBOK v8 (Scope Management), ISO 21502
> **Usage:** This is the *baseline* for scope control. All requirements, design, and development must stay within these boundaries. Changes require formal change control. "If it's not in the scope statement, it's out of scope."
