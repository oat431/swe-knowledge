---
document_type: Risk Register
version: "1.0"
status: Active
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [risk-register, risk-tracking, pmbok, iso-31000]
standard_ref:
  - PMBOK v8 — Planning (Risk Management)
  - ISO 31000 — Risk Management
---

# Risk Register

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This is the *living document* for tracking all identified risks throughout the project lifecycle.

## 2. Risk Register

| Risk ID | Category | Risk Description | Probability | Impact | Score | Level | Response Strategy | Response Actions | Owner | Trigger | Status | Due Date | Last Reviewed |
|---------|----------|-----------------|------------|--------|-------|-------|------------------|-----------------|-------|---------|--------|----------|--------------|
| R-001 | Resource | [Key developer leaves during sprints] | 3 — Possible | 4 — Major | 12 | 🟠 High | Mitigate | [Knowledge sharing, documentation, backup resource identified] | PM | [Resignation notice] | Open | [Ongoing] | [YYYY-MM-DD] |
| R-002 | Technical | [Legacy ERP API unstable] | 3 — Possible | 4 — Major | 12 | 🟠 High | Mitigate | [Integration POC in Week 2, fallback to batch file] | TL | [POC failure] | Open | [YYYY-MM-DD] | [YYYY-MM-DD] |
| R-003 | Technical | [Data migration quality issues] | 3 — Possible | 3 — Moderate | 9 | 🟡 Medium | Mitigate | [Data profiling before migration, parallel run, validation scripts] | Data Architect | [>5% error rate in profiling] | Open | [YYYY-MM-DD] | [YYYY-MM-DD] |
| R-004 | Schedule | [Stakeholder availability for UAT] | 4 — Likely | 3 — Moderate | 12 | 🟠 High | Mitigate | [Schedule UAT early, get manager commitment, backup users] | BA | [<50% attendance at workshops] | Open | [YYYY-MM-DD] | [YYYY-MM-DD] |
| R-005 | Cost | [Vendor price increase] | 2 — Unlikely | 3 — Moderate | 6 | 🟡 Medium | Transfer | [Fixed-price contract with penalty clauses] | PM | [Vendor notification] | Open | [Ongoing] | [YYYY-MM-DD] |
| R-006 | Quality | [Performance below NFRs] | 2 — Unlikely | 4 — Major | 8 | 🟡 Medium | Mitigate | [Load testing each sprint, architecture review, CDN] | TL | [Load test failure] | Open | [YYYY-MM-DD] | [YYYY-MM-DD] |
| R-007 | External | [Regulatory change mid-project] | 2 — Unlikely | 4 — Major | 8 | 🟡 Medium | Accept | [Monitor regulatory updates, contingency plan] | Compliance | [Regulatory announcement] | Open | [Ongoing] | [YYYY-MM-DD] |
| R-008 | Resource | [Team skill gap — cloud] | 3 — Possible | 2 — Minor | 6 | 🟡 Medium | Mitigate | [Cloud training before Sprint 1, vendor support] | PM | [Slow progress on cloud tasks] | Open | [YYYY-MM-DD] | [YYYY-MM-DD] |
| R-009 | Schedule | [Scope creep] | 4 — Likely | 3 — Moderate | 12 | 🟠 High | Mitigate | [Strict change control, MoSCoW, baseline management] | PM | [>3 CRs in a sprint] | Open | [Ongoing] | [YYYY-MM-DD] |
| R-010 | Technical | [Security vulnerability discovered] | 2 — Unlikely | 5 — Critical | 10 | 🟡 Medium | Mitigate | [SAST/DAST each sprint, pen test before go-live] | Security | [Critical vulnerability found] | Open | [YYYY-MM-DD] | [YYYY-MM-DD] |

## 3. Risk Heat Map

| Impact \ Probability | 1 — Rare | 2 — Unlikely | 3 — Possible | 4 — Likely | 5 — Almost Certain |
|---------------------|----------|-------------|-------------|-----------|-------------------|
| **5 — Critical** | 🟡 | 🟡 R-010 | 🟠 | 🔴 | 🔴 |
| **4 — Major** | 🟢 | 🟡 R-006, R-007 | 🟠 R-001, R-002 | 🟠 R-004, R-009 | 🔴 |
| **3 — Moderate** | 🟢 | 🟡 R-005 | 🟡 R-003 | 🟡 | 🟠 |
| **2 — Minor** | 🟢 | 🟢 | 🟡 R-008 | 🟡 | 🟡 |
| **1 — Insignificant** | 🟢 | 🟢 | 🟢 | 🟢 | 🟡 |

> **Legend:** 🔴 Critical — Immediate action required | 🟠 High — Mitigation plan required | 🟡 Medium — Monitor and manage | 🟢 Low — Accept and monitor

## 4. Risk Summary

| Level | Count | Risks |
|-------|-------|-------|
| 🔴 Critical | [0] | — |
| 🟠 High | [4] | R-001, R-002, R-004, R-009 |
| 🟡 Medium | [6] | R-003, R-005, R-006, R-007, R-008, R-010 |
| 🟢 Low | [0] | — |
| **Total** | **[10]** | |

## 5. Risk Response Status

| Risk ID | Response | Planned Actions | Completed Actions | Status |
|---------|----------|----------------|------------------|--------|
| R-001 | Mitigate | [4 actions] | [1 — backup identified] | 🟡 In Progress |
| R-002 | Mitigate | [2 actions] | [0] | 🟡 In Progress |
| R-003 | Mitigate | [3 actions] | [0] | ⬜ Not Started |
| R-004 | Mitigate | [3 actions] | [1 — early scheduling] | 🟡 In Progress |
| R-009 | Mitigate | [3 actions] | [2 — change control active] | 🟡 In Progress |

## 6. Retired / Closed Risks

| Risk ID | Description | Close Date | Reason | Outcome |
|---------|-------------|-----------|--------|---------|
| [R-XXX] | [Description] | [YYYY-MM-DD] | [Mitigated / Occurred / Passed / N/A] | [What happened] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Risk-Management-Plan]] | How risks are managed |
| [[Risk-Report]] | Risk status reporting |
| [[Risk-Analysis-Results]] | Initial risk analysis |
| [[Assumption-Log]] | Invalidated assumptions → risks |
| [[Change-Log]] | Scope changes may create risks |

---

> **Template Standard:** Based on PMBOK v8, ISO 31000
> **Usage:** This is a *living document* — update at every risk review (bi-weekly). Every 🟠/🔴 risk must have a mitigation plan, owner, and trigger. Review at sprint retrospectives and phase gates.
