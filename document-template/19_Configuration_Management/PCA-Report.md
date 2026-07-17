---
document_type: PCA Report (Physical Configuration Audit)
version: "1.0"
status: Draft
author: "[Configuration Manager]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [pca, physical-configuration-audit, swebok, sebok]
standard_ref:
  - SWEBOK v4 — Configuration Management
  - SEBoK v2 — Configuration Management
---

# PCA Report (Physical Configuration Audit)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Verifies that the as-built product matches the as-designed documentation — that the physical configuration matches the baseline.

## 2. PCA Summary

| Field | Detail |
|-------|--------|
| [Audit Date] | [YYYY-MM-DD] |
| [Auditor] | [Name] |
| [Scope] | [All configuration items] |
| [Baseline] | [BL-XXX] |
| [Overall Result] | ✅ Pass / ⚠️ Conditional / ❌ Fail |

## 3. PCA Checklist

| # | CI | Baseline Version | As-Built Version | Match | Status |
|---|-----|-----------------|-----------------|-------|--------|
| 1 | [Source Code] | [v1.0.0] | [v1.0.0] | ✅ | ✅ |
| 2 | [Docker Image] | [v1.0.0] | [v1.0.0] | ✅ | ✅ |
| 3 | [Database Schema] | [v1.0] | [v1.0] | ✅ | ✅ |
| 4 | [Configuration Files] | [v1.0] | [v1.0] | ✅ | ✅ |
| 5 | [Documentation] | [v1.0] | [v1.0] | ✅ | ✅ |
| 6 | [Test Suite] | [v1.0] | [v1.0] | ✅ | ✅ |

## 4. Verification Methods

| CI | Verification Method | Evidence |
|----|-------------------|---------|
| [Source Code] | [Git tag comparison] | [git log v1.0.0] |
| [Docker Image] | [Image digest comparison] | [docker inspect] |
| [Database Schema] | [Schema diff] | [Migration log] |
| [Configuration Files] | [File comparison] | [diff] |
| [Documentation] | [Version comparison] | [Git history] |

## 5. PCA Results

| Metric | Value |
|--------|-------|
| [Total CIs audited] | [X] |
| [CIs matching baseline] | [X] |
| [CIs with discrepancies] | [X] |
| [Match rate] | [X%] |

## 6. Findings

| # | Finding | CI | Baseline | As-Built | Status |
|---|--------|-----|---------|---------|--------|
| 1 | [No discrepancies found] | — | — | — | ✅ |

## 7. PCA Conclusion

> The physical configuration matches the baseline. All configuration items are as documented. The product passes the Physical Configuration Audit.

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
| [[FCA-Report]] | Functional audit counterpart |
| [[Baseline-Records]] | Baselines audited |
| [[Version-Description-Document]] | Version details |

---

> **Template Standard:** Based on SWEBOK v4, SEBoK v2
> **Usage:** PCA answers "Is what we built what we documented?" FCA checks function; PCA checks physical configuration.
