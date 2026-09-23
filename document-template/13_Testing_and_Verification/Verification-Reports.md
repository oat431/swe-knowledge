---
document_type: Verification Reports
schema_version: 2
canonical_name: Verification Reports
doc_form: record
applicability: evidence
min_project_tier: 4  # Small-Prod
tier_trigger: Produced by the activity it records
minimum_form: Tracker entries with links to decisions
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
| [Requirements] | [Method] | [N] | [N] | [X]% | [Status] |
| [Design] | [Method] | [N] | [N] | [X]% | [Status] |
| [Code] | [Method] | [N] | [N] | [X]% | [Status] |
| [Unit Test] | [Method] | [N] | [N] | [X]% | [Status] |
| [Integration] | [Method] | [N] | [N] | [X]% | [Status] |
| [System] | [Method] | [N] | [N] | [X]% | [Status] |

## 3. Requirements Verification

| Req ID | Requirement | Method | Result | Evidence |
|--------|-------------|--------|--------|---------|
| [FR-XXX] | [Requirement description] | [Method] | [Result] | [Evidence] |
| [NFR-XXX] | [Requirement description] | [Method] | [Result] | [Evidence] |

## 4. Design Verification

| Design Element | Method | Result | Evidence |
|---------------|--------|--------|---------|
| [Architecture] | [Method] | [Result] | [Evidence] |
| [Database schema] | [Method] | [Result] | [Evidence] |
| [API design] | [Method] | [Result] | [Evidence] |
| [Security design] | [Method] | [Result] | [Evidence] |

## 5. Code Verification

| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| [Code review coverage] | [Target] | [X]% | [Status] |
| [Coding standards compliance] | [Target] | [X]% | [Status] |
| [Static analysis — 0 errors] | [Target] | [N] | [Status] |
| [Unit test coverage] | [Target] | [X]% | [Status] |

## 6. Verification Issues

| # | Issue | Level | Resolution | Status |
|---|-------|-------|-----------|--------|
| 1 | [Issue description] | [Level] | [Resolution] | [Status] |

## 7. Verification Conclusion

> [Summary of verification outcome — whether the product was built according to specifications and meets all verification criteria.]

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
