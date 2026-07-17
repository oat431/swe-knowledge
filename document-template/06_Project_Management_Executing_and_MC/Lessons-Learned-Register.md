---
document_type: Lessons Learned Register
version: "1.0"
status: Active
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [lessons-learned, retrospective, continuous-improvement, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Executing (Knowledge Management)
  - ISO 21502 — Project Management Guidance
---

# Lessons Learned Register

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This register captures lessons learned throughout the project — not just at closure. Lessons are recorded as they happen, categorized, and used to improve current and future projects.

## 2. Lesson Categories

| Category | Description |
|----------|-------------|
| **Requirements** | [Elicitation, documentation, validation, change management] |
| **Design** | [Architecture, technology choices, design patterns] |
| **Development** | [Coding, code review, testing, DevOps] |
| **Testing** | [Test strategy, execution, defect management] |
| **Project Management** | [Planning, scheduling, risk, communication] |
| **Stakeholder** | [Engagement, communication, expectations] |
| **Team** | [Collaboration, skills, processes, tools] |
| **Vendor** | [Vendor management, contracts, delivery] |

## 3. Lessons Learned Register

| Lesson ID | Date | Category | Title | Description | What Went Well | What Could Improve | Recommendation | Impact | Source | Status |
|-----------|------|----------|-------|-------------|---------------|-------------------|---------------|--------|--------|--------|
| LL-001 | [YYYY-MM-DD] | Requirements | [Workshop-based elicitation more effective] | [80% of quality requirements came from workshops vs interviews] | [Structured workshops with cross-functional team] | [Could have done more workshops earlier] | [Use workshops as primary elicitation for complex requirements] | High | [Sprint 1 Retro] | ✅ Applied |
| LL-002 | [YYYY-MM-DD] | Development | [Integration POC saved weeks] | [Early ERP API POC revealed limitations before sprint work] | [Caught integration issues early] | [POC could have been more thorough] | [Always do integration POC for external systems] | High | [Sprint 2] | ✅ Applied |
| LL-003 | [YYYY-MM-DD] | Testing | [Test data preparation underestimated] | [No realistic test data caused UAT delays] | [N/A] | [Start test data prep in Sprint 1] | [Allocate test data preparation as a separate work package] | Medium | [UAT Phase] | ⏳ Future |
| LL-004 | [YYYY-MM-DD] | Stakeholder | [Stakeholder availability is the #1 risk] | [Operations Manager unavailable for 2 weeks during critical phase] | [BA covered for stakeholder] | [Get backup stakeholders identified early] | [Identify backup for every key stakeholder] | High | [Sprint 3] | ✅ Applied |
| LL-005 | [YYYY-MM-DD] | Team | [Code reviews improve quality significantly] | [Bugs caught in review reduced production defects by 40%] | [Team culture of constructive feedback] | [Review turnaround sometimes slow] | [Set 24h SLA for code reviews] | High | [Sprint 2] | ✅ Applied |
| LL-006 | [YYYY-MM-DD] | Requirements | [BDD acceptance criteria format works well] | [Zero ambiguity-related defects with GWT format] | [Clear, testable criteria] | [Takes more time to write] | [Use BDD format for all requirements] | High | [Sprint 1] | ✅ Applied |
| LL-007 | [YYYY-MM-DD] | PM | [Weekly status reports keep stakeholders aligned] | [No stakeholder complaints about visibility] | [Consistent format, on-time delivery] | [Could add more visual elements] | [Continue weekly reports, add dashboard link] | Medium | [Monthly] | ✅ Applied |
| LL-008 | [YYYY-MM-DD] | Vendor | [Fixed-price contracts reduce cost risk] | [Vendor delivered within budget] | [Clear SOW, milestone payments] | [SOW could be more detailed] | [Use fixed-price for well-defined scope, T&M for evolving scope] | High | [Project] | ✅ Applied |

## 4. Lesson Summary

### 4.1 By Category

| Category | Lessons | Applied | Future | High Impact |
|----------|---------|---------|--------|------------|
| Requirements | [2] | [2] | [0] | [2] |
| Development | [1] | [1] | [0] | [1] |
| Testing | [1] | [0] | [1] | [0] |
| Stakeholder | [1] | [1] | [0] | [1] |
| Team | [1] | [1] | [0] | [1] |
| PM | [1] | [1] | [0] | [0] |
| Vendor | [1] | [1] | [0] | [1] |
| **Total** | **[8]** | **[7]** | **[1]** | **[6]** |

### 4.2 Top Lessons (By Impact)

| # | Lesson | Category | Recommendation |
|---|--------|----------|---------------|
| 1 | [Workshop-based elicitation more effective] | Requirements | [Use workshops as primary technique] |
| 2 | [Integration POC saved weeks] | Development | [Always POC external integrations] |
| 3 | [Stakeholder availability is #1 risk] | Stakeholder | [Identify backup stakeholders] |
| 4 | [Code reviews reduce defects 40%] | Team | [Mandatory reviews with 24h SLA] |
| 5 | [BDD format eliminates ambiguity] | Requirements | [Use GWT for all acceptance criteria] |
| 6 | [Fixed-price reduces cost risk] | Vendor | [Use fixed-price for defined scope] |

## 5. Retrospective Integration

| Sprint | Date | Lessons Captured | Key Improvements Implemented |
|--------|------|-----------------|---------------------------|
| [Sprint 1] | [YYYY-MM-DD] | [LL-001, LL-006] | [Workshop approach, BDD format] |
| [Sprint 2] | [YYYY-MM-DD] | [LL-002, LL-005] | [Integration POC, code review SLA] |
| [Sprint 3] | [YYYY-MM-DD] | [LL-004] | [Backup stakeholders] |
| [UAT] | [YYYY-MM-DD] | [LL-003] | [Test data prep plan] |
| [Project] | [YYYY-MM-DD] | [LL-007, LL-008] | [Weekly reports, contract approach] |

## 6. Lessons Applied to Current Project

| Lesson | Applied | How | Impact |
|--------|---------|-----|--------|
| [LL-001] | ✅ | [Used workshops for all complex requirements] | [Higher quality requirements] |
| [LL-002] | ✅ | [Integration POC in Week 2] | [Caught issues early] |
| [LL-004] | ✅ | [Backup stakeholders identified] | [Reduced availability risk] |
| [LL-005] | ✅ | [24h code review SLA] | [Faster defect detection] |
| [LL-006] | ✅ | [BDD format for all ACs] | [Zero ambiguity defects] |

## 7. Lessons for Future Projects

| # | Lesson | Recommendation | Applicability |
|---|--------|---------------|--------------|
| 1 | [Workshop-based elicitation] | [Use workshops as primary technique] | All projects |
| 2 | [Integration POC] | [Always POC external integrations] | Projects with integrations |
| 3 | [Backup stakeholders] | [Identify backups for key stakeholders] | All projects |
| 4 | [Test data prep] | [Allocate as separate work package] | All projects |
| 5 | [Code review SLA] | [Set 24h turnaround] | All dev projects |
| 6 | [BDD format] | [Use GWT for acceptance criteria] | All projects |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Lessons-Learned-Register]] | Sprint-level lessons |
| [[Risk-Register]] | Lessons inform risk identification |
| [[Change-Log]] | Lessons from changes |
| [[Issue-Log]] | Lessons from issues |
| [[Project-Closure-Document]] | Final lessons compilation |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** Capture lessons *as they happen*, not just at project closure. Use sprint retrospectives as the primary source. Apply lessons to the current project immediately — don't wait for the next project.
