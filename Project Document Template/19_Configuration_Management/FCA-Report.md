---
document_type: FCA Report (Functional Configuration Audit)
version: "1.0"
status: Draft
author: "[Configuration Manager]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [fca, functional-configuration-audit, swebok, sebok]
standard_ref:
  - SWEBOK v4 — Configuration Management
  - SEBoK v2 — Configuration Management
---

# FCA Report (Functional Configuration Audit)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Verifies that the product meets its functional requirements — that the implementation matches the specification.

## 2. FCA Summary

| Field | Detail |
|-------|--------|
| [Audit Date] | [YYYY-MM-DD] |
| [Auditor] | [Name] |
| [Scope] | [All functional requirements] |
| [Baseline] | [BL-XXX] |
| [Overall Result] | ✅ Pass / ⚠️ Conditional / ❌ Fail |

## 3. FCA Checklist

| # | Requirement | Specification | Implementation | Verified | Status |
|---|------------|-------------|---------------|---------|--------|
| 1 | [FR-001: Submit request] | [SRS §3.1] | [Request Service] | [TC-001] | ✅ |
| 2 | [FR-002: Track status] | [SRS §3.2] | [Request Service] | [TC-004] | ✅ |
| 3 | [FR-003: Upload documents] | [SRS §3.3] | [Request Service] | [TC-006] | ✅ |
| 4 | [FR-101: Validate inputs] | [SRS §4.1] | [Processing Service] | [TC-010] | ✅ |
| 5 | [FR-102: Classify requests] | [SRS §4.2] | [Processing Service] | [TC-013] | ✅ |
| 6 | [FR-103: Auto-approve] | [SRS §4.3] | [Processing Service] | [TC-015] | ✅ |
| 7 | [FR-104: Manual review] | [SRS §4.4] | [Processing Service] | [TC-020] | ✅ |
| 8 | [FR-201: Email notifications] | [SRS §5.1] | [Notification Service] | [TC-030] | ✅ |
| 9 | [FR-301: Dashboard] | [SRS §6.1] | [Reporting Service] | [TC-040] | ✅ |
| 10 | [FR-302: Reports] | [SRS §6.2] | [Reporting Service] | [TC-042] | ✅ |

## 4. FCA Results

| Metric | Value |
|--------|-------|
| [Total requirements audited] | [X] |
| [Requirements verified] | [X] |
| [Requirements with issues] | [X] |
| [Pass rate] | [X%] |

## 5. Findings

| # | Finding | Requirement | Severity | Status |
|---|--------|-----------|---------|--------|
| 1 | [No findings] | — | — | ✅ |

## 6. FCA Conclusion

> All functional requirements have been verified against the implementation. The product passes the Functional Configuration Audit.

### Sign-Off

| Role | Name | Signature | Date |
|------|------|-----------|------|
| [Auditor] | | | |
| [Technical Lead] | | | |
| [Configuration Manager] | | | |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[PCA-Report]] | Physical audit counterpart |
| [[Baseline-Records]] | Baselines audited |
| [[Traceability-Matrix-Req-Tests]] | Requirements traceability |

---

> **Template Standard:** Based on SWEBOK v4, SEBoK v2
> **Usage:** FCA answers "Does the product do what it's supposed to do?" Every requirement must have verification evidence.
