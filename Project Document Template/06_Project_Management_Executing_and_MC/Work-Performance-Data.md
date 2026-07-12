---
document_type: Work Performance Data
version: "1.0"
status: Active
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [work-performance-data, metrics, status, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Monitoring & Controlling
  - ISO 21502 — Project Management Guidance
---

# Work Performance Data

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document captures raw work performance data — observations and measurements from project execution. Data is collected continuously and analyzed to produce work performance information and reports.

## 2. Performance Dashboard

| Metric | Current | Previous | Trend | Target | Status |
|--------|---------|----------|-------|--------|--------|
| [Schedule Performance Index (SPI)] | [X.XX] | [X.XX] | ↑↓→ | [≥0.95] | 🟢🟡🔴 |
| [Cost Performance Index (CPI)] | [X.XX] | [X.XX] | ↑↓→ | [≥0.95] | 🟢🟡🔴 |
| [Sprint Velocity] | [X SP] | [Y SP] | ↑↓→ | [20 SP] | 🟢🟡🔴 |
| [Sprint Burndown] | [X SP remaining] | — | → | [Linear decline] | 🟢🟡🔴 |
| [Defect Density] | [X/feature] | [Y/feature] | ↑↓→ | [<2/feature] | 🟢🟡🔴 |
| [Test Coverage] | [X%] | [Y%] | ↑↓→ | [≥80%] | 🟢🟡🔴 |
| [Open Issues] | [X] | [Y] | ↑↓→ | [<5] | 🟢🟡🔴 |
| [Open Risks — High] | [X] | [Y] | ↑↓→ | [<3] | 🟢🟡🔴 |

## 3. Schedule Performance

### 3.1 Sprint Performance

| Sprint | Planned SP | Completed SP | Velocity | Carryover | SPI |
|--------|-----------|-------------|---------|-----------|-----|
| [Sprint 1] | [20] | [18] | [18] | [2] | [0.90] |
| [Sprint 2] | [20] | [20] | [20] | [0] | [1.00] |
| [Sprint 3] | [20] | [22] | [22] | [0] | [1.10] |
| [Sprint 4] | [20] | [19] | [19] | [1] | [0.95] |
| [Sprint 5] | [20] | [21] | [21] | [0] | [1.05] |
| **Average** | **[20]** | **[20]** | **[20]** | | **[1.00]** |

### 3.2 Milestone Performance

| Milestone | Baseline Date | Forecast Date | Actual Date | Variance (days) | Status |
|----------|--------------|---------------|-------------|----------------|--------|
| [Requirements Baselined] | [YYYY-MM-DD] | [YYYY-MM-DD] | [YYYY-MM-DD] | [0] | ✅ On Time |
| [Design Approved] | [YYYY-MM-DD] | [YYYY-MM-DD] | [YYYY-MM-DD] | [+2] | 🟡 Slightly Late |
| [Sprint 1 Complete] | [YYYY-MM-DD] | [YYYY-MM-DD] | [YYYY-MM-DD] | [0] | ✅ On Time |
| [Sprint 2 Complete] | [YYYY-MM-DD] | [YYYY-MM-DD] | [YYYY-MM-DD] | [0] | ✅ On Time |
| [Sprint 3 Complete] | [YYYY-MM-DD] | [YYYY-MM-DD] | [YYYY-MM-DD] | [-1] | ✅ Early |
| [Sprint 4 Complete] | [YYYY-MM-DD] | [YYYY-MM-DD] | | | ⏳ In Progress |
| [Sprint 5 Complete] | [YYYY-MM-DD] | | | | ⬜ Not Started |
| [System Testing] | [YYYY-MM-DD] | | | | ⬜ Not Started |
| [UAT Complete] | [YYYY-MM-DD] | | | | ⬜ Not Started |
| [Go-Live] | [YYYY-MM-DD] | | | | ⬜ Not Started |

### 3.3 Earned Value Metrics

| Period | PV | EV | AC | SV | CV | SPI | CPI | EAC | VAC |
|--------|-----|-----|-----|-----|-----|------|------|------|------|
| [Week 1-2] | $[X] | $[Y] | $[Z] | $[SV] | $[CV] | [SPI] | [CPI] | $[EAC] | $[VAC] |
| [Week 3-4] | $[X] | $[Y] | $[Z] | $[SV] | $[CV] | [SPI] | [CPI] | $[EAC] | $[VAC] |
| [Week 5-6] | $[X] | $[Y] | $[Z] | $[SV] | $[CV] | [SPI] | [CPI] | $[EAC] | $[VAC] |
| [Week 7-8] | $[X] | $[Y] | $[Z] | $[SV] | $[CV] | [SPI] | [CPI] | $[EAC] | $[VAC] |

## 4. Cost Performance

| Category | Budget | Actual | Variance | % Spent | Status |
|----------|--------|--------|---------|---------|--------|
| [Personnel] | $[X] | $[Y] | $[Z] | [W%] | 🟢🟡🔴 |
| [Software] | $[X] | $[Y] | $[Z] | [W%] | 🟢🟡🔴 |
| [Infrastructure] | $[X] | $[Y] | $[Z] | [W%] | 🟢🟡🔴 |
| [Vendor] | $[X] | $[Y] | $[Z] | [W%] | 🟢🟡🔴 |
| [Training] | $[X] | $[Y] | $[Z] | [W%] | 🟢🟡🔴 |
| **Total** | **$[BAC]** | **$[Actual]** | **$[Variance]** | **[%]** | **🟢🟡🔴** |

## 5. Quality Performance

| Metric | Current Sprint | Previous Sprint | Project Average | Target | Status |
|--------|---------------|----------------|----------------|--------|--------|
| [Defects Found] | [X] | [Y] | [Z] | — | 🟢🟡🔴 |
| [Defects Fixed] | [X] | [Y] | [Z] | — | 🟢🟡🔴 |
| [Defects Open] | [X] | [Y] | [Z] | [<5] | 🟢🟡🔴 |
| [Defect Density] | [X/feature] | [Y/feature] | [Z/feature] | [<2] | 🟢🟡🔴 |
| [Code Coverage] | [X%] | [Y%] | [Z%] | [≥80%] | 🟢🟡🔴 |
| [Code Review Coverage] | [X%] | [Y%] | [Z%] | [100%] | 🟢🟡🔴 |
| [Test Pass Rate] | [X%] | [Y%] | [Z%] | [≥95%] | 🟢🟡🔴 |

## 6. Risk & Issue Performance

| Metric | Current | Previous | Trend | Target | Status |
|--------|---------|----------|-------|--------|--------|
| [Open Risks — Total] | [X] | [Y] | ↑↓→ | [<15] | 🟢🟡🔴 |
| [Open Risks — 🔴 Critical] | [X] | [Y] | ↑↓→ | [0] | 🟢🟡🔴 |
| [Open Risks — 🟠 High] | [X] | [Y] | ↑↓→ | [<3] | 🟢🟡🔴 |
| [Risks Materialized] | [X] | [Y] | ↑↓→ | [0] | 🟢🟡🔴 |
| [Open Issues — Total] | [X] | [Y] | ↑↓→ | [<5] | 🟢🟡🔴 |
| [Open Issues — 🔴 Critical] | [X] | [Y] | ↑↓→ | [0] | 🟢🟡🔴 |
| [Overdue Actions] | [X] | [Y] | ↑↓→ | [0] | 🟢🟡🔴 |

## 7. Change Performance

| Metric | Current Period | Project Total | Target | Status |
|--------|---------------|--------------|--------|--------|
| [Changes Submitted] | [X] | [Y] | — | — |
| [Changes Approved] | [X] | [Y] | — | — |
| [Changes Rejected] | [X] | [Y] | — | — |
| [Requirements Stability] | [X%] | [Y%] | [≥85%] | 🟢🟡🔴 |
| [Schedule Impact (Cumulative)] | [+X days] | [+Y days] | [<10% of baseline] | 🟢🟡🔴 |
| [Cost Impact (Cumulative)] | [+$X] | [+$Y] | [<10% of BAC] | 🟢🟡🔴 |

## 8. Team Performance

| Metric | Current Sprint | Previous Sprint | Target | Status |
|--------|---------------|----------------|--------|--------|
| [Sprint Velocity] | [X SP] | [Y SP] | [20 SP] | 🟢🟡🔴 |
| [Team Satisfaction] | [X/5] | [Y/5] | [≥4/5] | 🟢🟡🔴 |
| [Meeting Attendance] | [X%] | [Y%] | [≥80%] | 🟢🟡🔴 |
| [Action Item Completion] | [X%] | [Y%] | [≥90%] | 🟢🟡🔴 |
| [Knowledge Sharing Sessions] | [X] | [Y] | [1/sprint] | 🟢🟡🔴 |

## 9. Data Collection Schedule

| Data Point | Collection Method | Frequency | Owner |
|-----------|------------------|-----------|-------|
| [Sprint velocity] | [Jira — completed story points] | Per sprint | [PM] |
| [Defect metrics] | [Jira — defect report] | Per sprint | [QA Lead] |
| [Code coverage] | [CI/CD — coverage tool] | Per build | [TL] |
| [Cost actuals] | [Finance system] | Monthly | [PM] |
| [Risk/issue counts] | [Risk register / Issue log] | Weekly | [PM] |
| [Team satisfaction] | [Anonymous survey] | Monthly | [PM] |
| [EVM metrics] | [Manual calculation] | Bi-weekly | [PM] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Project-Schedule]] | Schedule baseline |
| [[Cost-Baseline]] | Cost baseline |
| [[Risk-Register]] | Risk data |
| [[Issue-Log]] | Issue data |
| [[Quality-Metrics]] | Quality data |
| [[Change-Log]] | Change data |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** This is the *raw data* layer — observations and measurements from project execution. Analyze this data to produce Work Performance Information (trends, patterns) and Work Performance Reports (status, forecasts for stakeholders).
