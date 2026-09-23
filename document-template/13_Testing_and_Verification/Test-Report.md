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
| [Test Environment] | [Environment] |
| [Total Test Cases] | [N] |
| [Executed] | [N] |
| [Passed] | [N] |
| [Failed] | [N] |
| [Blocked] | [N] |
| [Skipped] | [N] |
| **Pass Rate** | **[X]%** |

## 3. Results by Module

| Module | Total | Passed | Failed | Pass Rate | Status |
|--------|-------|--------|--------|----------|--------|
| [Module Name] | [N] | [N] | [N] | [X]% | [Status] |
| **Total** | **[N]** | **[N]** | **[N]** | **[X]%** | **[Status]** |

## 4. Results by Type

| Type | Total | Passed | Failed | Pass Rate |
|------|-------|--------|--------|----------|
| [Unit] | [N] | [N] | [N] | [X]% |
| [Integration] | [N] | [N] | [N] | [X]% |
| [System] | [N] | [N] | [N] | [X]% |
| [E2E] | [N] | [N] | [N] | [X]% |

## 5. Failed Test Cases

| Test Case | Module | Failure | Defect | Root Cause |
|-----------|--------|---------|--------|-----------|
| [TC-XXX] | [Module Name] | [Failure description] | [DEF-XXX] | [Root cause] |

## 6. Defect Summary

| Severity | Found | Fixed | Remaining |
|---------|-------|-------|----------|
| 🔴 Critical | [N] | [N] | [N] |
| 🟡 High | [N] | [N] | [N] |
| 🟢 Medium | [N] | [N] | [N] |
| ⚪ Low | [N] | [N] | [N] |
| **Total** | **[N]** | **[N]** | **[N]** |

## 7. Test Coverage

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Requirements coverage] | [X]% | [Target] | [Status] |
| [Code coverage] | [X]% | [Target] | [Status] |
| [Critical path coverage] | [X]% | [Target] | [Status] |
| [Automation rate] | [X]% | [Target] | [Status] |

## 8. Quality Assessment

| Aspect | Assessment | Evidence |
|--------|-----------|---------|
| [Functional Quality] | [Assessment] | [Evidence] |
| [Stability] | [Assessment] | [Evidence] |
| [Performance] | [Assessment] | [Evidence] |
| [Security] | [Assessment] | [Evidence] |
| [Readiness] | [Assessment] | [Evidence] |

## 9. Recommendations

| # | Recommendation | Priority | Owner |
|---|---------------|---------|-------|
| 1 | [Recommendation] | [Priority] | [Owner] |
| 2 | [Recommendation] | [Priority] | [Owner] |
| 3 | [Recommendation] | [Priority] | [Owner] |
| 4 | [Recommendation] | [Priority] | [Owner] |

## 10. Release Recommendation

| Field | Detail |
|-------|--------|
| [Release Ready?] | [Yes / No / Conditional] |
| [Conditions] | [Conditions, e.g. fix DEF-[XXX] before release] |
| [Risk Level] | [Risk level and rationale] |
| [Sign-Off] | [Sign-off status] |

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
