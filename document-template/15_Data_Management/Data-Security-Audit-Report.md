---
document_type: Data Security Audit Report
version: "1.0"
status: Draft
author: "[Security Officer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [data-security, audit, compliance, dmbok]
standard_ref:
  - DMBOK v2 — Data Security
---

# Data Security Audit Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Reports on data security controls effectiveness — evidence for compliance and continuous improvement.

## 2. Audit Summary

| Field | Detail |
|-------|--------|
| [Audit Date] | [YYYY-MM-DD] |
| [Auditor] | [Internal / External] |
| [Scope] | [All data stores and data processing activities] |
| [Result] | ✅ Pass / ⚠️ Conditional / ❌ Fail |

## 3. Control Assessment

### 3.1 Access Controls

| Control | Status | Evidence | Findings |
|---------|--------|---------|---------|
| [RBAC implemented] | ✅ | [IAM policies] | [No findings] |
| [Least privilege enforced] | ✅ | [Access reviews] | [No findings] |
| [MFA for L1 data] | ✅ | [Auth logs] | [No findings] |
| [Access reviews conducted] | ✅ | [Review records] | [No findings] |
| [Termination access revoked] | ✅ | [HR + IT logs] | [No findings] |

### 3.2 Encryption

| Control | Status | Evidence | Findings |
|---------|--------|---------|---------|
| [Encryption at rest] | ✅ | [KMS config] | [No findings] |
| [Encryption in transit] | ✅ | [TLS config] | [No findings] |
| [Key management] | ✅ | [KMS policies] | [No findings] |
| [Certificate management] | ✅ | [Certificate inventory] | [No findings] |

### 3.3 Data Classification

| Control | Status | Evidence | Findings |
|---------|--------|---------|---------|
| [Classification schema defined] | ✅ | [[Data-Classification-Schema]] | [No findings] |
| [Data classified] | ✅ | [Classification register] | [No findings] |
| [Controls per classification] | ✅ | [Control matrix] | [No findings] |

### 3.4 Data Lifecycle

| Control | Status | Evidence | Findings |
|---------|--------|---------|---------|
| [Retention policy defined] | ✅ | [[Data-Retention-Archival-Policy]] | [No findings] |
| [Automated retention] | ✅ | [Archival scripts] | [No findings] |
| [Secure disposal] | ✅ | [Disposal certificates] | [No findings] |

### 3.5 Monitoring & Logging

| Control | Status | Evidence | Findings |
|---------|--------|---------|---------|
| [Audit logging enabled] | ✅ | [Log configuration] | [No findings] |
| [No PII in logs] | ✅ | [Log review] | [No findings] |
| [Log retention adequate] | ✅ | [Retention config] | [No findings] |
| [Alerting configured] | ✅ | [Alert rules] | [No findings] |

## 4. Findings Summary

| Severity | Count | Remediated | Open |
|---------|-------|-----------|------|
| 🔴 Critical | [0] | [0] | [0] |
| 🟠 High | [0] | [0] | [0] |
| 🟡 Medium | [1] | [1] | [0] |
| 🟢 Low | [2] | [2] | [0] |
| **Total** | **[3]** | **[3]** | **[0]** |

## 5. Findings

| # | Finding | Severity | Recommendation | Status |
|---|--------|---------|---------------|--------|
| 1 | [Access review documentation incomplete] | 🟡 | [Standardize review templates] | ✅ Fixed |
| 2 | [Log rotation not documented] | 🟢 | [Document log rotation policy] | ✅ Fixed |
| 3 | [Encryption key rotation schedule unclear] | 🟢 | [Document key rotation schedule] | ✅ Fixed |

## 6. Recommendations

| # | Recommendation | Priority | Owner | Status |
|---|---------------|---------|-------|--------|
| 1 | [Automate access review reporting] | 🟡 | [DGO] | ⬜ |
| 2 | [Implement data loss prevention (DLP)] | 🟡 | [Security] | ⬜ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Access-Control-Policy]] | Access controls audited |
| [[Data-Encryption-Standards]] | Encryption audited |
| [[Data-Classification-Schema]] | Classification audited |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Audit is *evidence-based*. Document everything. Auditors want proof, not promises.
