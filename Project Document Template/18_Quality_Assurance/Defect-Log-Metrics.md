---
document_type: Defect Log / Metrics
version: "1.0"
status: Active
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [defect-log, defect-metrics, quality, swebok]
standard_ref:
  - SWEBOK v4 — Quality Assurance
---

# Defect Log / Metrics

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Tracks defects found during all phases — construction, testing, and production. Measures defect trends and quality improvement.

## 2. Defect Log

| ID | Date | Phase | Severity | Module | Description | Status | Resolution |
|----|------|-------|---------|--------|-----------|--------|-----------|
| [DEF-001] | [YYYY-MM-DD] | [Testing] | 🔴 Critical | [Request] | [Upload fails on mobile] | ✅ Fixed | [File size validation] |
| [DEF-002] | [YYYY-MM-DD] | [Testing] | 🔴 Critical | [Auth] | [Token refresh race condition] | ✅ Fixed | [Mutex lock added] |
| [DEF-003] | [YYYY-MM-DD] | [Testing] | 🟡 High | [Reporting] | [Filter not applied] | ✅ Fixed | [Query builder fix] |
| [DEF-004] | [YYYY-MM-DD] | [Production] | 🟢 Medium | [UI] | [Tooltip positioning] | ⬜ Open | — |
| [DEF-005] | [YYYY-MM-DD] | [Production] | ⚪ Low | [Request] | [Date format inconsistency] | ⬜ Open | — |

## 3. Defect Metrics

| Metric | Definition | Target | Current | Status |
|--------|-----------|--------|---------|--------|
| [Defect Density] | [Defects / Feature] | [< 2] | [X] | 🟢🟡🔴 |
| [Defect Removal Efficiency] | [Defects found before prod / Total] | [> 95%] | [X%] | 🟢🟡🔴 |
| [Defect Leakage] | [Defects found in prod / Total] | [< 5%] | [X%] | 🟢🟡🔴 |
| [MTTR] | [Mean time to resolution] | [< 1 day] | [X hours] | 🟢🟡🔴 |
| [Reopen Rate] | [Reopened defects / Total] | [< 10%] | [X%] | 🟢🟡🔴 |

## 4. Defect Trends

| Month | Found | Fixed | Remaining | Leakage |
|-------|-------|-------|----------|---------|
| [Month 1] | [12] | [10] | [2] | [0%] |
| [Month 2] | [8] | [9] | [1] | [0%] |
| [Month 3] | [5] | [6] | [0] | [0%] |
| [Month 4] | [6] | [5] | [1] | [16%] |
| **Total** | **[31]** | **[30]** | **[1]** | **[3%]** |

## 5. Defects by Phase Found

| Phase | Count | Percentage | Trend |
|-------|-------|-----------|-------|
| [Code Review] | [X] | [X%] | ↑↓→ |
| [Unit Testing] | [X] | [X%] | ↑↓→ |
| [Integration Testing] | [X] | [X%] | ↑↓→ |
| [System Testing] | [X] | [X%] | ↑↓→ |
| [UAT] | [X] | [X%] | ↑↓→ |
| [Production] | [X] | [X%] | ↑↓→ |

## 6. Defects by Module

| Module | Defects | Density | Status |
|--------|---------|---------|--------|
| [Request] | [X] | [X/feature] | 🟢🟡🔴 |
| [Processing] | [X] | [X/feature] | 🟢🟡🔴 |
| [Auth] | [X] | [X/feature] | 🟢🟡🔴 |
| [Notification] | [X] | [X/feature] | 🟢🟡🔴 |
| [Reporting] | [X] | [X/feature] | 🟢🟡🔴 |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Defect-Report]] | Individual defect details |
| [[Quality-Metrics-Dashboard]] | Quality visualization |
| [[Test-Report]] | Test results |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Defects found early are cheap to fix. Track where defects are found — if most are found in production, shift left.
