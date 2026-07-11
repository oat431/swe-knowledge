---
document_type: Work Performance Reports
version: "1.0"
status: Active
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [work-performance-reports, status-reports, dashboards, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Monitoring & Controlling
  - ISO 21502 — Project Management Guidance
---

# Work Performance Reports

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document compiles work performance information into formal reports for stakeholder consumption. While data is raw and information is analyzed, reports are *communicated* — tailored for specific audiences.

## 2. Report Types

| Report | Audience | Frequency | Format | Owner |
|--------|---------|-----------|--------|-------|
| [Weekly Status Report] | [Sponsor, stakeholders] | Weekly | [Email + dashboard] | PM |
| [Sprint Summary] | [All stakeholders] | Bi-weekly | [Email + demo] | PM |
| [Monthly Dashboard] | [Steering Committee] | Monthly | [Presentation] | PM |
| [Phase Gate Report] | [Steering Committee] | Per phase | [Formal report] | PM |
| [Risk Report] | [Sponsor] | Bi-weekly | [Email] | PM |
| [Financial Report] | [Sponsor, Finance] | Monthly | [Email] | PM |
| [Quality Report] | [Sponsor, QA] | Per sprint | [Email] | QA Lead |

## 3. Weekly Status Report Template

```markdown
# Weekly Status Report — Week of [YYYY-MM-DD]

## Executive Summary
- **Overall Status:** 🟢 On Track / 🟡 At Risk / 🔴 Behind
- **Sprint:** [X] of [5] | **Velocity:** [X] SP completed
- **Key Achievement:** [What was accomplished this week]
- **Key Risk:** [Top risk this week]
- **Decision Needed:** [If any]

## Schedule
| Milestone | Baseline | Forecast | Variance | Status |
|-----------|---------|---------|---------|--------|
| [Next milestone] | [Date] | [Date] | [X days] | 🟢🟡🔴 |

## Budget
| Category | Budget | Actual | Variance | Status |
|----------|--------|--------|---------|--------|
| [Total] | $[X] | $[Y] | $[Z] | 🟢🟡🔴 |

## Sprint Progress
- **Planned:** [X] SP | **Completed:** [Y] SP | **Remaining:** [Z] SP
- **Burndown:** [On track / Ahead / Behind]

## Quality
| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| [Defect Density] | [<2/feature] | [X/feature] | 🟢🟡🔴 |
| [Code Coverage] | [≥80%] | [X%] | 🟢🟡🔴 |
| [Open Defects] | [<5] | [X] | 🟢🟡🔴 |

## Risks & Issues
| ID | Description | Level | Status | Action |
|----|-------------|-------|--------|--------|
| [R-XXX] | [Description] | 🟠 | [In progress] | [Action] |

## Decisions Needed
| # | Decision | Deadline | Authority |
|---|---------|----------|-----------|
| 1 | [Decision] | [Date] | [Sponsor] |

## Next Week
- [Key activities planned]
- [Milestones expected]
```

## 4. Monthly Dashboard Report Template

```markdown
# Monthly Dashboard — [Month Year]

## Overall Status
| Dimension | Status | Trend | Notes |
|-----------|--------|-------|-------|
| Schedule | 🟢🟡🔴 | ↑↓→ | [Brief note] |
| Budget | 🟢🟡🔴 | ↑↓→ | [Brief note] |
| Quality | 🟢🟡🔴 | ↑↓→ | [Brief note] |
| Risk | 🟢🟡🔴 | ↑↓→ | [Brief note] |
| Team | 🟢🟡🔴 | ↑↓→ | [Brief note] |

## Key Metrics
| Metric | Target | Actual | Status | Trend |
|--------|--------|--------|--------|-------|
| SPI | ≥0.95 | [X] | 🟢🟡🔴 | ↑↓→ |
| CPI | ≥0.95 | [X] | 🟢🟡🔴 | ↑↓→ |
| Sprint Velocity | 20 SP | [X] SP | 🟢🟡🔴 | ↑↓→ |
| Defect Density | <2/feature | [X] | 🟢🟡🔴 | ↑↓→ |
| Open Issues | <5 | [X] | 🟢🟡🔴 | ↑↓→ |

## Milestone Status
| Milestone | Baseline | Forecast | Actual | Variance |
|----------|---------|---------|--------|---------|
| [Milestone] | [Date] | [Date] | [Date] | [X days] |

## Budget Status
| Category | Budget | Actual | EAC | Variance |
|----------|--------|--------|-----|---------|
| [Total] | $[X] | $[Y] | $[Z] | $[W] |

## Top Risks
| Risk | Level | Status | Action |
|------|-------|--------|--------|
| [R-XXX] | 🟠 | [In progress] | [Action] |

## Accomplishments This Month
- [Achievement 1]
- [Achievement 2]

## Planned Next Month
- [Activity 1]
- [Activity 2]

## Decisions / Escalations
| # | Item | Authority | Deadline |
|---|------|----------|----------|
| 1 | [Item] | [Sponsor] | [Date] |
```

## 5. Phase Gate Report Template

```markdown
# Phase Gate Report — [Phase Name]

## Gate Summary
| Field | Detail |
|-------|--------|
| Phase | [Phase name] |
| Gate Date | [YYYY-MM-DD] |
| Recommendation | ✅ Proceed / ⚠️ Conditional / ❌ Do Not Proceed |

## Gate Criteria Assessment
| # | Criterion | Required | Actual | Status |
|---|----------|---------|--------|--------|
| 1 | [All deliverables complete] | [100%] | [X%] | ✅❌ |
| 2 | [Quality gates passed] | [All pass] | [Status] | ✅❌ |
| 3 | [No critical defects open] | [0] | [X] | ✅❌ |
| 4 | [Stakeholder sign-off] | [All signed] | [Status] | ✅❌ |
| 5 | [Budget within tolerance] | [±10%] | [X%] | ✅❌ |

## Phase Performance
| Metric | Plan | Actual | Variance |
|--------|------|--------|---------|
| Duration | [X days] | [Y days] | [Z days] |
| Cost | $[X] | $[Y] | $[Z] |
| Deliverables | [X] | [Y] | [Z] |
| Defects Found | [X] | [Y] | [Z] |

## Lessons Learned
| # | Lesson | Recommendation |
|---|--------|---------------|
| 1 | [Lesson] | [Recommendation] |

## Risks for Next Phase
| Risk | Level | Mitigation |
|------|-------|-----------|
| [R-XXX] | 🟠 | [Mitigation plan] |

## Decision Requested
[Approve proceeding to next phase / Approve with conditions / Do not proceed]
```

## 6. Report Distribution Matrix

| Report | Sponsor | Steering | BA | TL | QA | Dev | Change Mgr | Frequency |
|--------|---------|----------|-----|-----|-----|-----|-----------|-----------|
| [Weekly Status] | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Weekly |
| [Sprint Summary] | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Bi-weekly |
| [Monthly Dashboard] | ✅ | ✅ | I | I | I | I | I | Monthly |
| [Phase Gate] | ✅ | ✅ | C | C | C | I | I | Per phase |
| [Risk Report] | ✅ | I | C | C | C | I | I | Bi-weekly |
| [Financial Report] | ✅ | ✅ | I | I | I | I | I | Monthly |
| [Quality Report] | C | I | C | C | ✅ | I | I | Per sprint |

> **✅** = Receives report | **C** = Consulted before report | **I** = Informed after report

## 7. Report Metrics

| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| [Reports delivered on time] | [100%] | [X%] | 🟢🟡🔴 |
| [Stakeholder satisfaction with reports] | [≥4/5] | [X/5] | 🟢🟡🔴 |
| [Report-related escalations] | [0] | [X] | 🟢🟡🔴 |
| [Decisions made from reports] | [Tracking] | [X] | — |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Work Performance Data]] | Raw data source |
| [[Work Performance Information]] | Analysis source |
| [[Communications Management Plan]] | Report distribution |
| [[Risk Report]] | Risk-specific reporting |
| [[Quality Metrics]] | Quality data source |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** Reports are the *communication* layer — data becomes information becomes decisions. Tailor reports to the audience: executives want status and decisions; teams want details and actions. Deliver on time, every time.
