---
document_type: Data Policy
version: "1.0"
status: Draft
author: "[Data Governance Officer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-policy, governance, dmbok]
standard_ref:
  - DMBOK v2 — Data Governance
---

# Data Policy

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines mandatory rules for data management — collection, storage, access, use, sharing, and disposal.

## 2. Policy Statements

### 2.1 Data Collection

| # | Policy | Enforcement |
|---|--------|-----------|
| 1 | [Collect only data necessary for business purpose] | [Privacy Impact Assessment] |
| 2 | [Obtain consent before collecting personal data] | [Consent management] |
| 3 | [Document data sources and collection methods] | [Data catalog] |
| 4 | [Validate data at point of entry] | [Input validation] |

### 2.2 Data Storage

| # | Policy | Enforcement |
|---|--------|-----------|
| 5 | [Classify all data before storage] | [Data classification] |
| 6 | [Encrypt sensitive data at rest] | [KMS + AES-256] |
| 7 | [Store data in approved locations only] | [Infrastructure controls] |
| 8 | [Apply retention schedules] | [Automated archival] |

### 2.3 Data Access

| # | Policy | Enforcement |
|---|--------|-----------|
| 9 | [Apply least privilege access] | [RBAC] |
| 10 | [Authenticate all data access] | [MFA + audit logging] |
| 11 | [Log all data access] | [Audit trail] |
| 12 | [Review access quarterly] | [Access review process] |

### 2.4 Data Use

| # | Policy | Enforcement |
|---|--------|-----------|
| 13 | [Use data only for stated purpose] | [Purpose limitation] |
| 14 | [Anonymize data for analytics where possible] | [Data masking] |
| 15 | [Do not copy production data without approval] | [Change control] |

### 2.5 Data Sharing

| # | Policy | Enforcement |
|---|--------|-----------|
| 16 | [Execute data sharing agreements] | [Legal review] |
| 17 | [Encrypt data in transit] | [TLS 1.3] |
| 18 | [Log all data exports] | [Audit trail] |

### 2.6 Data Disposal

| # | Policy | Enforcement |
|---|--------|-----------|
| 19 | [Follow retention schedules] | [Automated disposal] |
| 20 | [Verify secure deletion] | [Deletion certificates] |
| 21 | [Archive before disposal] | [Archive process] |

## 3. Compliance

| Violation | Severity | Consequence |
|----------|---------|-----------|
| [Unauthorized data access] | 🔴 Critical | [Investigation + disciplinary] |
| [Data breach (negligence)] | 🔴 Critical | [Termination + legal] |
| [Policy bypass] | 🟡 High | [Warning + training] |
| [Incomplete documentation] | 🟢 Medium | [Remediation + coaching] |

## 4. Policy Review

| Frequency | Reviewer | Approver |
|----------|---------|---------|
| [Annual] | [DGO] | [Governance Council] |
| [After incident] | [DGO] | [Governance Council] |
| [After regulatory change] | [DGO] | [Governance Council] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Governance-Charter]] | Authority |
| [[Data-Standards]] | Implementation standards |
| [[Security-Policy]] | Security alignment |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Data policy is *mandatory*. Ignorance is not an excuse. Train all staff on data policies.
