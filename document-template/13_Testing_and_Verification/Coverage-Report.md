---
document_type: Coverage Report
version: "1.0"
status: Active
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [coverage, code-coverage, test-coverage, swebok]
standard_ref:
  - SWEBOK v4 — Testing
---

# Coverage Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Measures test coverage — how much of the codebase and requirements are exercised by tests.

## 2. Coverage Types

| Type | Measurement | Target | Tool |
|------|-----------|--------|------|
| [Code Coverage] | [Lines/branches executed] | [≥ 80%] | [Jest / Istanbul] |
| [Requirements Coverage] | [Requirements with tests] | [100%] | [Manual / RTM] |
| [Branch Coverage] | [If/else paths executed] | [≥ 75%] | [Jest / Istanbul] |
| [Function Coverage] | [Functions called] | [≥ 90%] | [Jest / Istanbul] |

## 3. Code Coverage Summary

| Module | Statements | Branches | Functions | Lines | Status |
|--------|-----------|---------|----------|-------|--------|
| [Request Service] | [87%] | [82%] | [90%] | [86%] | 🟢 |
| [Processing Service] | [85%] | [78%] | [88%] | [84%] | 🟢 |
| [Auth Service] | [92%] | [88%] | [95%] | [91%] | 🟢 |
| [Notification Service] | [78%] | [70%] | [82%] | [77%] | 🟡 |
| [Reporting Service] | [75%] | [68%] | [80%] | [74%] | 🟡 |
| [Integration Service] | [80%] | [72%] | [85%] | [79%] | 🟢 |
| **Total** | **[83%]** | **[76%]** | **[87%]** | **[82%]** | **🟢** |

## 4. Coverage by File (Top Uncovered)

| File | Lines | Covered | Uncovered | Coverage |
|------|-------|---------|----------|---------|
| [notification-templates.ts] | [120] | [84] | [36] | [70%] |
| [report-builder.ts] | [200] | [148] | [52] | [74%] |
| [error-handler.ts] | [50] | [38] | [12] | [76%] |
| [rate-limiter.ts] | [80] | [62] | [18] | [78%] |

## 5. Requirements Coverage

| Category | Requirements | Covered | Coverage |
|---------|-------------|---------|---------|
| [Functional] | [33] | [33] | [100%] |
| [Non-Functional] | [12] | [12] | [100%] |
| [Security] | [8] | [8] | [100%] |
| **Total** | **[53]** | **[53]** | **[100%]** |

## 6. Coverage Trends

| Period | Statements | Branches | Functions | Lines |
|--------|-----------|---------|----------|-------|
| [Sprint 1] | [72%] | [65%] | [78%] | [71%] |
| [Sprint 2] | [78%] | [70%] | [82%] | [77%] |
| [Sprint 3] | [83%] | [76%] | [87%] | [82%] |
| **Trend** | **↑** | **↑** | **↑** | **↑** |

## 7. Coverage Gaps

| # | Gap | Impact | Action | Owner |
|---|-----|--------|--------|-------|
| 1 | [Notification templates] | [Edge cases untested] | [Add template tests] | [Dev] |
| 2 | [Report builder] | [Complex queries untested] | [Add query tests] | [Dev] |
| 3 | [Error handler] | [Some error paths untested] | [Add error tests] | [Dev] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Test-Report]] | Test results |
| [[Traceability-Matrix-Req-Tests]] | Requirements coverage |
| [[Static-Analysis-Reports]] | Code quality |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Coverage is a *guide*, not a goal. 100% coverage doesn't mean 100% quality. But low coverage means untested code.
