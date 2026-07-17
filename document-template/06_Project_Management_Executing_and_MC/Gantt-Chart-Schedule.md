---
document_type: Gantt Chart / Schedule
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [gantt-chart, schedule, timeline, mermaid, swebok]
standard_ref:
  - SWEBOK v4 — Cross-Cutting (Project Management)
  - PMBOK v8 — Planning (Schedule Management)
  - ISO 21502 — Project Management Guidance
---

# Gantt Chart / Schedule

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document provides a visual Gantt chart representation of the project schedule using Mermaid syntax. It renders in GitHub, Obsidian, VS Code, and most markdown viewers.

## 2. Full Project Gantt Chart

```mermaid
gantt
    title Project Schedule — [Project Name]
    dateFormat YYYY-MM-DD
    axisFormat %d %b %Y
    tickInterval 1week

    section 1.0 Initiation
    Project Charter           :a1, 2026-08-01, 3d
    Charter Approval          :a2, after a1, 2d
    Kickoff Meeting           :milestone, after a2, 0d

    section 2.0 Planning
    Stakeholder Analysis      :b1, after a2, 3d
    Requirements Elicitation  :b2, after b1, 12d
    Requirements Documentation:b3, after b2, 12d
    Requirements Review       :b4, after b3, 3d
    Requirements Baseline     :milestone, after b4, 0d
    Architecture Design       :b5, after b4, 7d
    Architecture Review       :b6, after b5, 2d
    Detailed Design           :b7, after b6, 10d
    Design Review             :milestone, after b7, 0d

    section 3.0 Sprint 1 — Portal Core
    Sprint 1 Development      :c1, after b7, 10d
    Sprint 1 Review           :milestone, after c1, 0d

    section 4.0 Sprint 2 — Portal + Processing
    Sprint 2 Development      :d1, after c1, 10d
    Sprint 2 Review           :milestone, after d1, 0d

    section 5.0 Sprint 3 — Processing + Admin
    Sprint 3 Development      :e1, after d1, 10d
    Sprint 3 Review           :milestone, after e1, 0d

    section 6.0 Sprint 4 — Admin + Notifications
    Sprint 4 Development      :f1, after e1, 10d
    Sprint 4 Review           :milestone, after f1, 0d

    section 7.0 Sprint 5 — Dashboard + Polish
    Sprint 5 Development      :g1, after f1, 10d
    Sprint 5 Review           :milestone, after g1, 0d
    Code Freeze               :milestone, after g1, 0d

    section 8.0 System Testing
    System Test Execution     :h1, after g1, 10d
    Integration Testing       :h2, after h1, 5d
    Performance Testing       :h3, after h2, 5d
    Security Testing          :h4, after h2, 5d
    Defect Fix & Retest       :h5, after h3, 5d

    section 9.0 UAT
    UAT Execution             :i1, after h5, 10d
    UAT Sign-off              :milestone, after i1, 0d

    section 10.0 Deployment
    Deployment Preparation    :j1, after i1, 5d
    Data Migration            :j2, after j1, 3d
    Go-Live                   :milestone, crit, after j2, 0d
    Training Delivery         :j3, after j2, 5d
    Hypercare Support         :j4, after j2, 20d

    section 11.0 Closure
    Lessons Learned           :k1, after j4, 5d
    Documentation Archive     :k2, after k1, 3d
    Project Closure Report    :k3, after k2, 2d
    Closure Sign-off          :milestone, after k3, 0d
```

## 3. Phase Summary Gantt

```mermaid
gantt
    title Project Phases — High Level
    dateFormat YYYY-MM-DD

    section Initiation
    Charter & Kickoff         :a1, 2026-08-01, 6d

    section Planning
    Requirements              :b1, after a1, 30d
    Design                    :b2, after b1, 19d

    section Execution
    Sprint 1                  :c1, after b2, 10d
    Sprint 2                  :c2, after c1, 10d
    Sprint 3                  :c3, after c2, 10d
    Sprint 4                  :c4, after c3, 10d
    Sprint 5                  :c5, after c4, 10d

    section Testing
    System Testing            :d1, after c5, 25d
    UAT                       :d2, after d1, 10d

    section Deployment
    Go-Live Prep              :e1, after d2, 8d
    Go-Live                   :milestone, crit, after e1, 0d
    Hypercare                 :e2, after e1, 20d

    section Closure
    Closure                   :f1, after e2, 10d
```

## 4. Sprint-Level Gantt

```mermaid
gantt
    title Sprint Schedule
    dateFormat YYYY-MM-DD

    section Sprint 1
    Planning                  :a1, 2026-10-05, 1d
    Development               :a2, after a1, 8d
    Review & Retro            :a3, after a2, 1d

    section Sprint 2
    Planning                  :b1, after a3, 1d
    Development               :b2, after b1, 8d
    Review & Retro            :b3, after b2, 1d

    section Sprint 3
    Planning                  :c1, after b3, 1d
    Development               :c2, after c1, 8d
    Review & Retro            :c3, after c2, 1d

    section Sprint 4
    Planning                  :d1, after c3, 1d
    Development               :d2, after d1, 8d
    Review & Retro            :d3, after d2, 1d

    section Sprint 5
    Planning                  :e1, after d3, 1d
    Development               :e2, after e1, 8d
    Review & Retro            :e3, after e2, 1d
```

## 5. Resource Gantt

```mermaid
gantt
    title Resource Allocation Timeline
    dateFormat YYYY-MM-DD

    section Management
    PM (50%)                  :a1, 2026-08-01, 170d
    Change Manager (25%)      :a2, 2026-08-15, 140d

    section Analysis
    Business Analyst          :b1, 2026-08-01, 140d

    section Architecture
    Solution Architect (25%)  :c1, 2026-09-01, 30d
    Technical Lead            :c2, 2026-09-01, 150d

    section Development
    Senior Dev 1              :d1, 2026-10-01, 90d
    Senior Dev 2              :d2, 2026-10-01, 90d
    Junior Dev                :d3, 2026-10-01, 90d
    DevOps (50%)              :d4, 2026-10-01, 120d

    section Quality
    QA Lead                   :e1, 2026-11-15, 60d
    QA Engineer               :e2, 2026-11-15, 60d

    section Vendor
    Implementation Consultant :f1, 2026-10-01, 40d
    Data Migration Specialist :f2, 2026-12-15, 15d
```

## 6. Milestone Gantt

```mermaid
gantt
    title Key Milestones
    dateFormat YYYY-MM-DD

    section Gates
    Project Kickoff           :milestone, 2026-08-08, 0d
    Requirements Baselined    :milestone, 2026-09-05, 0d
    Design Approved           :milestone, 2026-09-26, 0d

    section Sprints
    Sprint 1 Complete         :milestone, 2026-10-17, 0d
    Sprint 2 Complete         :milestone, 2026-10-31, 0d
    Sprint 3 Complete         :milestone, 2026-11-14, 0d
    Sprint 4 Complete         :milestone, 2026-11-28, 0d
    Sprint 5 Complete         :milestone, 2026-12-12, 0d
    Code Freeze               :milestone, 2026-12-12, 0d

    section Testing
    System Testing Complete   :milestone, 2027-01-09, 0d
    UAT Complete              :milestone, 2027-01-23, 0d

    section Deployment
    Go-Live                   :milestone, crit, 2027-01-30, 0d
    Hypercare End             :milestone, 2027-02-27, 0d

    section Closure
    Project Closure           :milestone, 2027-03-06, 0d
```

## 7. Critical Path Gantt

```mermaid
gantt
    title Critical Path
    dateFormat YYYY-MM-DD

    section Critical Path
    Requirements Elicitation  :crit, a1, 2026-08-11, 12d
    Requirements Documentation:crit, a2, after a1, 12d
    Requirements Review       :crit, a3, after a2, 3d
    Architecture Design       :crit, a4, after a3, 7d
    Architecture Review       :crit, a5, after a4, 2d
    Detailed Design           :crit, a6, after a5, 10d
    Sprint 1-5                :crit, a7, after a6, 50d
    System Testing            :crit, a8, after a7, 25d
    UAT                       :crit, a9, after a8, 10d
    Go-Live Prep              :crit, a10, after a9, 8d
    Go-Live                   :milestone, crit, after a10, 0d

    section Non-Critical (Float)
    Stakeholder Analysis      :b1, 2026-08-11, 3d
    Training                  :b2, after a10, 5d
    Hypercare                 :b3, after a10, 20d
    Closure                   :b4, after b3, 10d
```

## 8. Gantt Chart Legend

| Element | Meaning |
|---------|---------|
| `task name` | Regular activity |
| `crit, task name` | Critical path activity |
| `milestone` | Zero-duration milestone |
| `milestone, crit` | Critical milestone |
| Arrow `-->` | Dependency (finish-to-start) |
| `after task_id` | Starts after specified task |

## 9. How to Use These Charts

| Viewer | Renders Mermaid? | Notes |
|--------|-----------------|-------|
| [GitHub] | ✅ Yes | [Renders in markdown preview] |
| [Obsidian] | ✅ Yes | [Mermaid plugin built-in] |
| [VS Code] | ✅ Yes | [Mermaid extension required] |
| [GitLab] | ✅ Yes | [Renders in markdown] |
| [Notion] | ✅ Yes | [Mermaid block] |
| [Confluence] | ⚠️ Plugin | [Mermaid plugin required] |
| [Export to PDF] | ⚠️ Via tool | [Use mmdc CLI or Pandoc] |

### Export to Image/PDF

```bash
# Install Mermaid CLI
npm install -g @mermaid-js/mermaid-cli

# Export to PNG
mmdc -i schedule.mmd -o schedule.png

# Export to PDF
mmdc -i schedule.mmd -o schedule.pdf

# Export to SVG
mmdc -i schedule.mmd -o schedule.svg
```

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Project-Schedule]] | Detailed schedule with dates |
| [[Schedule-Baseline]] | Approved baseline dates |
| [[Activity-List]] | Activities being visualized |
| [[Milestone-List]] | Milestones being visualized |
| [[Resource-Management-Plan]] | Resource allocation visualized |
| [[Schedule-Management-Plan]] | How schedule is managed |

---

> **Template Standard:** Based on SWEBOK v4, PMBOK v8
> **Usage:** These Gantt charts provide *visual* schedule representation. Use the Full Project Gantt for comprehensive view, Phase Summary for executive reporting, Sprint Gantt for team planning, and Resource Gantt for capacity management. All charts are Mermaid — they render natively in most markdown viewers.
