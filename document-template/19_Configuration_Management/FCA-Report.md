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
| [N] | [FR-XXX: Requirement name] | [SRS §X.Y] | [Implementing component] | [TC-XXX] | [✅/❌] |

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
