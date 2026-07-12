---
document_type: Verification Reports
version: "1.0"
status: Active
author: "[Systems Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [verification-reports, v-and-v, sebok, iso-15288]
standard_ref:
  - SEBoK v2 — Verification & Validation (ISO/IEC/IEEE 15288)
---

# Verification Reports

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Summarizes verification results — confirming the product was built correctly according to specifications.

## 2. Verification Summary

| Level | Method | Items | Verified | Pass Rate | Status |
|-------|--------|-------|---------|----------|--------|
| [Requirements] | [Review] | [53] | [53] | [100%] | ✅ |
| [Design] | [Review] | [18] | [18] | [100%] | ✅ |
| [Code] | [Review] | [73 PRs] | [73] | [100%] | ✅ |
| [Unit Test] | [Execution] | [35] | [34] | [97%] | ✅ |
| [Integration] | [Execution] | [20] | [19] | [95%] | ✅ |
| [System] | [Execution] | [13] | [12] | [92%] | ✅ |

## 3. Requirements Verification

| Req ID | Requirement | Method | Result | Evidence |
|--------|-------------|--------|--------|---------|
| [FR-001] | [Submit request] | [System test] | ✅ Pass | [TC-001] |
| [FR-002] | [Track status] | [System test] | ✅ Pass | [TC-004] |
| [FR-101] | [Validate inputs] | [Unit + System] | ✅ Pass | [TC-010, TC-011] |
| [FR-103] | [Auto-approve] | [Integration] | ✅ Pass | [TC-015] |
| [NFR-001] | [Response < 2s] | [Performance test] | ✅ Pass | [Perf report] |
| [NFR-002] | [99.9% availability] | [Architecture review] | ✅ Pass | [SAD review] |

## 4. Design Verification

| Design Element | Method | Result | Evidence |
|---------------|--------|--------|---------|
| [Architecture] | [ATAM evaluation] | ✅ Pass | [[Architecture-Evaluation-Report]] |
| [Database schema] | [Design review] | ✅ Pass | [[Design-Review-Records]] |
| [API design] | [Design review] | ✅ Pass | [[Design-Review-Records]] |
| [Security design] | [Threat modeling] | ✅ Pass | [[Security-Test-Report]] |

## 5. Code Verification

| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| [Code review coverage] | [100% PRs] | [100%] | ✅ |
| [Coding standards compliance] | [100%] | [100%] | ✅ |
| [Static analysis — 0 errors] | [0] | [0] | ✅ |
| [Unit test coverage] | [≥ 80%] | [83%] | ✅ |

## 6. Verification Issues

| # | Issue | Level | Resolution | Status |
|---|-------|-------|-----------|--------|
| 1 | [No issues found] | — | — | ✅ |

## 7. Verification Conclusion

> All verification activities have been completed successfully. The product has been built according to specifications and meets all verification criteria.

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Verification-Plan]] | Plan that was executed |
| [[Validation-Reports]] | Validation counterpart |
| [[Test-Report]] | Test results |

---

> **Template Standard:** Based on SEBoK v2, ISO/IEC/IEEE 15288
> **Usage:** Verification reports answer "Did we build it right?" Every requirement should have verification evidence.
