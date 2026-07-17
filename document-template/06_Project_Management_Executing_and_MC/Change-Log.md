---
document_type: Change Log
version: "1.0"
status: Active
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [change-log, change-tracking, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Executing (Integration Management)
  - ISO 21502 — Project Management Guidance
---

# Change Log

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This log provides a comprehensive record of all changes made to the project — scope, schedule, cost, requirements, design, and baselines. It is the single source of truth for "what changed and why."

## 2. Change Log Register

| Change ID | Date | Category | Title | Description | Reason | Impact | Decision | Authority | Implemented |
|-----------|------|----------|-------|-------------|--------|--------|----------|----------|------------|
| CHG-001 | [YYYY-MM-DD] | Scope | [Added save draft feature] | [Users can save incomplete forms] | [User feedback — 40% abandon] | [+3 days, +$2K] | ✅ Approved | PM | [Date] |
| CHG-002 | [YYYY-MM-DD] | Quality | [Changed response time target] | [From <2s to <1.5s] | [Competitive benchmark] | [+$5K CDN] | ✅ Approved | PM | [Date] |
| CHG-003 | [YYYY-MM-DD] | Scope | [Added bulk upload] | [Corporate clients can upload CSV] | [Missed in initial elicitation] | [+5 days, +$3K] | ✅ Approved | CCB | [Date] |
| CHG-004 | [YYYY-MM-DD] | Scope | [Deferred ML anomaly detection] | [Moved to Phase 3] | [Budget constraint] | [-$50K, -3 months Phase 1] | ✅ Approved | Steering | [Date] |
| CHG-005 | [YYYY-MM-DD] | Schedule | [Sprint 3 extended by 3 days] | [Integration POC took longer] | [ERP API complexity] | [+3 days] | ✅ Approved | PM | [Date] |
| CHG-006 | [YYYY-MM-DD] | Requirement | [Modified FR-005] | [Added auto-save every 5 min] | [User feedback] | [No cost/schedule impact] | ✅ Approved | PM | [Date] |

## 3. Change Categories

| Category | Description | Examples |
|----------|-------------|---------|
| **Scope** | [Features added, removed, or modified] | [New feature, feature cut] |
| **Schedule** | [Timeline changes] | [Sprint extension, milestone delay] |
| **Cost** | [Budget changes] | [Additional vendor cost, contingency use] |
| **Quality** | [Quality attribute changes] | [NFR target changes] |
| **Requirement** | [Requirements modifications] | [Requirement added, modified, removed] |
| **Design** | [Design changes] | [Architecture change, technology switch] |
| **Resource** | [Resource changes] | [Team member added/removed] |
| **Baseline** | [Baseline revisions] | [Re-baselining after major change] |

## 4. Change Summary

### 4.1 By Category

| Category | Count | Total Impact |
|----------|-------|-------------|
| Scope | [3] | [+8 days, +$5K] |
| Quality | [1] | [+$5K] |
| Schedule | [1] | [+3 days] |
| Requirement | [1] | [No impact] |
| Design | [0] | — |
| Resource | [0] | — |
| Baseline | [0] | — |
| **Total** | **[6]** | **[+11 days, +$10K]** |

### 4.2 By Decision

| Decision | Count | Percentage |
|----------|-------|-----------|
| ✅ Approved | [6] | [100%] |
| ❌ Rejected | [0] | [0%] |
| ⏸️ Deferred | [0] | [0%] |
| ⏳ Pending | [0] | [0%] |
| **Total** | **[6]** | **100%** |

### 4.3 Change Trend

| Period | Changes Submitted | Approved | Impact |
|--------|------------------|---------|--------|
| [Month 1] | [2] | [2] | [+3 days, +$2K] |
| [Month 2] | [3] | [3] | [+5 days, +$8K] |
| [Month 3] | [1] | [1] | [+3 days] |
| **Total** | **[6]** | **[6]** | **[+11 days, +$10K]** |

## 5. Impact on Baselines

| Baseline | Original | Current | Change | Status |
|----------|---------|---------|--------|--------|
| [Scope Baseline] | [v1.0] | [v1.1] | [+1 feature, 1 deferred] | Updated |
| [Schedule Baseline] | [YYYY-MM-DD go-live] | [YYYY-MM-DD go-live] | [+11 days] | Updated |
| [Cost Baseline] | $[X] | $[X + $10K] | [+$10K] | Updated |

## 6. Lessons from Changes

| # | Lesson | Impact |
|---|--------|--------|
| 1 | [More thorough elicitation could have caught bulk upload earlier] | [Invest more in workshops] |
| 2 | [ERP API complexity underestimated] | [Better POC before sprint] |
| 3 | [User feedback valuable — save draft was quick win] | [Continue user involvement] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Change-Requests]] | Formal CR process |
| [[Requirements-Change-Log]] | Requirements-specific changes |
| [[Requirements-Change-Assessment]] | Impact analysis per change |
| [[Schedule-Baseline]] | Baseline being tracked |
| [[Cost-Baseline]] | Baseline being tracked |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** This is the *master change record* — every change to any baselined artifact is logged here. Use it for trend analysis, lessons learned, and stakeholder reporting. If a change happened and it's not here, it didn't go through proper change control.
