---
document_type: Test Report
version: "1.0"
status: Active
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [test-report, test-results, swebok, iso-29119]
standard_ref:
  - SWEBOK v4 — Testing
  - ISO/IEC/IEEE 29119 — Software Testing
---

# Test Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Summarizes test execution results — what was tested, what passed, what failed, and the overall quality assessment.

## 2. Test Execution Summary

| Metric | Value |
|--------|-------|
| [Test Period] | [YYYY-MM-DD to YYYY-MM-DD] |
| [Build Version] | [vX.Y.Z] |
| [Test Environment] | [Staging] |
| [Total Test Cases] | [73] |
| [Executed] | [73] |
| [Passed] | [69] |
| [Failed] | [4] |
| [Blocked] | [0] |
| [Skipped] | [0] |
| **Pass Rate** | **[95%]** |

## 3. Results by Module

| Module | Total | Passed | Failed | Pass Rate | Status |
|--------|-------|--------|--------|----------|--------|
| [Request Management] | [25] | [24] | [1] | [96%] | 🟢 |
| [Processing] | [18] | [17] | [1] | [94%] | 🟢 |
| [Authentication] | [12] | [12] | [0] | [100%] | 🟢 |
| [Notifications] | [8] | [7] | [1] | [88%] | 🟡 |
| [Reporting] | [10] | [9] | [1] | [90%] | 🟡 |
| **Total** | **[73]** | **[69]** | **[4]** | **[95%]** | **🟢** |

## 4. Results by Type

| Type | Total | Passed | Failed | Pass Rate |
|------|-------|--------|--------|----------|
| [Unit] | [35] | [34] | [1] | [97%] |
| [Integration] | [20] | [19] | [1] | [95%] |
| [System] | [13] | [12] | [1] | [92%] |
| [E2E] | [5] | [4] | [1] | [80%] |

## 5. Failed Test Cases

| Test Case | Module | Failure | Defect | Root Cause |
|-----------|--------|---------|--------|-----------|
| [TC-008] | [Request] | [Document upload timeout] | [DEF-001] | [File size validation] |
| [TC-017] | [Processing] | [Auto-approve not triggered] | [DEF-003] | [Rule engine edge case] |
| [TC-031] | [Notification] | [Email not sent] | [DEF-005] | [Template rendering] |
| [TC-043] | [Reporting] | [Report filter not applied] | [DEF-003] | [Query builder] |

## 6. Defect Summary

| Severity | Found | Fixed | Remaining |
|---------|-------|-------|----------|
| 🔴 Critical | [2] | [2] | [0] |
| 🟡 High | [1] | [0] | [1] |
| 🟢 Medium | [1] | [0] | [1] |
| ⚪ Low | [0] | [0] | [0] |
| **Total** | **[4]** | **[2]** | **[2]** |

## 7. Test Coverage

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Requirements coverage] | [100%] | [100%] | ✅ |
| [Code coverage] | [85%] | [≥ 80%] | ✅ |
| [Critical path coverage] | [100%] | [100%] | ✅ |
| [Automation rate] | [80%] | [≥ 60%] | ✅ |

## 8. Quality Assessment

| Aspect | Assessment | Evidence |
|--------|-----------|---------|
| [Functional Quality] | 🟢 Good | [95% pass rate, all critical paths pass] |
| [Stability] | 🟢 Good | [No flaky tests, consistent results] |
| [Performance] | 🟢 Good | [All responses < 2s] |
| [Security] | 🟢 Good | [No critical vulnerabilities] |
| [Readiness] | 🟡 Conditional | [1 high defect remaining] |

## 9. Recommendations

| # | Recommendation | Priority | Owner |
|---|---------------|---------|-------|
| 1 | [Fix DEF-003 (High) before release] | 🔴 | [Dev Team] |
| 2 | [Fix DEF-005 (Medium) in next sprint] | 🟡 | [Dev Team] |
| 3 | [Add more E2E tests for notification flow] | 🟡 | [QA Team] |
| 4 | [Improve test data for edge cases] | 🟢 | [QA Team] |

## 10. Release Recommendation

| Field | Detail |
|-------|--------|
| [Release Ready?] | ⚠️ Conditional |
| [Conditions] | [Fix DEF-003 (High) before release] |
| [Risk Level] | [Low — 1 high defect with workaround] |
| [Sign-Off] | [Pending PM + QA Lead approval] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Test-Plan]] | Plan executed |
| [[Test-Cases]] | Cases executed |
| [[Defect-Report]] | Defects found |
| [[Traceability-Matrix-Req-Tests]] | Coverage traceability |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC/IEEE 29119
> **Usage:** The test report is the *quality evidence* for release decisions. Don't release without it.
