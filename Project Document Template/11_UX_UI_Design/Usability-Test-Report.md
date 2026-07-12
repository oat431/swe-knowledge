---
document_type: Usability Test Report
version: "1.0"
status: Draft
author: "[UX Researcher]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [usability-test-report, findings, ux-research]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Usability Test Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Executive Summary

| Field | Detail |
|-------|--------|
| [Test Date] | [YYYY-MM-DD to YYYY-MM-DD] |
| [Participants] | [15 total: 8 customers, 4 staff, 3 managers] |
| [Key Findings] | [5 critical, 8 major, 12 minor] |
| [SUS Score] | [72 — Above Average] |
| [Overall Assessment] | [Usable with improvements needed] |

## 2. Task Results

| Task | Success Rate | Avg Time | Errors | Verdict |
|------|-------------|---------|--------|---------|
| [Submit Request] | [85%] | [6:32] | [2.1 avg] | 🟡 Needs improvement |
| [Check Status] | [95%] | [1:45] | [0.3 avg] | 🟢 Good |
| [Process Request] | [90%] | [3:15] | [1.2 avg] | 🟢 Good |
| [Find Request by ID] | [100%] | [0:45] | [0.1 avg] | 🟢 Excellent |
| [Generate Report] | [75%] | [4:20] | [2.8 avg] | 🔴 Needs redesign |

## 3. SUS Score Analysis

| Question | Avg Score | Interpretation |
|---------|----------|---------------|
| [Q1: Use frequently] | [3.8] | [Positive] |
| [Q2: Unnecessarily complex] | [2.1] | [Good — low complexity] |
| [Q3: Easy to use] | [4.0] | [Positive] |
| [Q4: Need technical support] | [2.3] | [Good — low dependency] |
| [Q5: Well integrated] | [3.9] | [Positive] |
| **Total SUS** | **[72]** | **Above Average (68 = avg)** |

## 4. Critical Findings

| # | Finding | Task | Severity | Frequency | Recommendation |
|---|---------|------|---------|----------|---------------|
| F-001 | [Upload fails silently on mobile] | [Submit] | 🔴 Critical | [6/8 customers] | [Add upload progress + error state] |
| F-002 | [Report filters confusing] | [Report] | 🔴 Critical | [3/3 managers] | [Simplify filter UI, add presets] |
| F-003 | [No confirmation before reject] | [Process] | 🔴 Critical | [4/4 staff] | [Add confirmation dialog] |
| F-004 | [Status timeline hard to read] | [Status] | 🟡 Major | [5/8 customers] | [Larger icons, clearer labels] |
| F-005 | [Queue sorting not obvious] | [Queue] | 🟡 Major | [3/4 staff] | [Add sort dropdown, default to priority] |

## 5. Participant Quotes

| # | Quote | Participant | Context |
|---|-------|-----------|---------|
| 1 | ["I clicked upload but nothing happened. I didn't know if it worked."] | [P-003] | [Mobile upload] |
| 2 | ["The filters are overwhelming. I just want to see this week's data."] | [P-012] | [Report generation] |
| 3 | ["I almost rejected a request by accident. There should be a confirmation."] | [P-009] | [Request processing] |
| 4 | ["Once I found my request, the status was clear. But finding it took a while."] | [P-005] | [Status tracking] |

## 6. Heatmap Summary

| Area | Click Density | Observation |
|------|-------------|------------|
| [Submit Button] | [High] | [Users found it easily] |
| [Navigation — New Request] | [High] | [Clear and discoverable] |
| [Filter Bar] | [Low] | [Users didn't notice filters] |
| [Help Icon] | [Very Low] | [Never clicked] |
| [Document Upload] | [High + Errors] | [Clicked but failed] |

## 7. Recommendations

| # | Recommendation | Priority | Effort | Impact |
|---|---------------|---------|--------|--------|
| 1 | [Fix mobile upload with progress indicator] | 🔴 | [3 days] | [High] |
| 2 | [Add confirmation dialog for reject] | 🔴 | [1 day] | [High] |
| 3 | [Simplify report filters with presets] | 🔴 | [5 days] | [High] |
| 4 | [Improve status timeline readability] | 🟡 | [2 days] | [Medium] |
| 5 | [Add sort dropdown to queue] | 🟡 | [1 day] | [Medium] |
| 6 | [Move filters above table] | 🟡 | [1 day] | [Medium] |
| 7 | [Remove or redesign help icon] | 🟢 | [1 day] | [Low] |

## 8. Iteration Plan

| Iteration | Changes | Test Date |
|-----------|---------|----------|
| [Sprint 1] | [Fix #1, #2 — critical issues] | [YYYY-MM-DD] |
| [Sprint 2] | [Fix #3, #4, #5 — major issues] | [YYYY-MM-DD] |
| [Sprint 3] | [Fix #6, #7 — minor issues] | [YYYY-MM-DD] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Usability-Test-Plan]] | Test plan that generated this report |
| [[Interactive-Prototype]] | Prototype tested |
| [[User-Personas]] | Participants matched personas |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** This report drives design iteration. Fix critical issues first, then re-test. A SUS score of 72 is good — aim for 80+ after iteration.
