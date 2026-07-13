---
document_type: Compliance Assessment Report
version: "1.0"
status: Draft
author: "[Security Officer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [compliance, assessment, audit, cyberok, iso-27001]
standard_ref:
  - CyBOK v1 — Security Governance
---

# Compliance Assessment Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Maps security controls to regulatory and contractual requirements — demonstrating compliance posture.

## 2. Compliance Frameworks

| Framework | Applicability | Status |
|---------|-------------|--------|
| [ISO 27001] | [Information security management] | ✅ Compliant |
| [OWASP Top 10] | [Web application security] | ✅ Compliant |
| [GDPR] | [Data protection (EU users)] | ✅ Compliant |
| [PCI DSS] | [Payment card data] | ⚠️ Partial (if applicable) |
| [SOC 2 Type II] | [Service organization controls] | 🔄 In Progress |

## 3. Control Assessment

### 3.1 ISO 27001 Controls

| Control | Description | Implemented | Evidence | Status |
|---------|-----------|-------------|---------|--------|
| [A.5.1] | [Security policies] | ✅ | [[Security-Policy]] | ✅ |
| [A.6.1] | [Internal organization] | ✅ | [[ISMS-Documentation]] | ✅ |
| [A.8.1] | [Asset management] | ✅ | [[System-Bill-of-Materials]] | ✅ |
| [A.9.1] | [Access control] | ✅ | [[Access-Control-Policy]] | ✅ |
| [A.9.2] | [User access management] | ✅ | [[Authentication-Standard]] | ✅ |
| [A.10.1] | [Cryptographic controls] | ✅ | [Encryption at rest + transit] | ✅ |
| [A.12.1] | [Operations security] | ✅ | [[Operations-Manual-Runbook]] | ✅ |
| [A.12.6] | [Vulnerability management] | ✅ | [[Vulnerability-Management-Report]] | ✅ |
| [A.14.1] | [Security in development] | ✅ | [[SSDLC-Process-Documentation]] | ✅ |
| [A.14.2] | [Security in development lifecycle] | ✅ | [[Secure-Coding-Guidelines]] | ✅ |
| [A.16.1] | [Incident management] | ✅ | [[Incident-Response-Plan]] | ✅ |
| [A.17.1] | [Business continuity] | ✅ | [[Business-Continuity-Plan-BCP]] | ✅ |

### 3.2 OWASP Top 10 Compliance

| # | Category | Controls | Status |
|---|---------|---------|--------|
| A01 | [Broken Access Control] | [RBAC, least privilege, access logging] | ✅ |
| A02 | [Cryptographic Failures] | [AES-256, TLS 1.3, bcrypt] | ✅ |
| A03 | [Injection] | [Parameterized queries, input validation] | ✅ |
| A04 | [Insecure Design] | [Threat modeling, secure design review] | ✅ |
| A05 | [Security Misconfiguration] | [Security headers, no defaults] | ✅ |
| A06 | [Vulnerable Components] | [SCA scanning, patch management] | ✅ |
| A07 | [Auth Failures] | [MFA, rate limiting, session mgmt] | ✅ |
| A08 | [Data Integrity Failures] | [Input validation, CI/CD integrity] | ✅ |
| A09 | [Logging Failures] | [Audit logging, no sensitive data] | ✅ |
| A10 | [SSRF] | [URL validation, allowlisting] | ✅ |

### 3.3 GDPR Compliance

| Requirement | Implementation | Status |
|------------|---------------|--------|
| [Lawful basis] | [Consent + legitimate interest] | ✅ |
| [Data minimization] | [Collect only necessary data] | ✅ |
| [Right to access] | [Data export API] | ✅ |
| [Right to erasure] | [Data deletion process] | ✅ |
| [Data portability] | [Standard format export] | ✅ |
| [Breach notification] | [72-hour notification process] | ✅ |
| [DPO appointment] | [DPO designated] | ✅ |

## 4. Compliance Gaps

| # | Gap | Framework | Risk | Remediation | Status |
|---|-----|----------|------|-----------|--------|
| 1 | [SOC 2 Type II audit pending] | [SOC 2] | [Medium] | [Complete audit by YYYY-MM-DD] | 🔄 |

## 5. Compliance Score

| Framework | Controls | Compliant | Partial | Non-Compliant | Score |
|---------|---------|----------|---------|-------------|-------|
| [ISO 27001] | [12] | [12] | [0] | [0] | [100%] |
| [OWASP Top 10] | [10] | [10] | [0] | [0] | [100%] |
| [GDPR] | [7] | [7] | [0] | [0] | [100%] |
| **Overall** | **[29]** | **[29]** | **[0]** | **[0]** | **[100%]** |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[ISMS-Documentation]] | ISMS framework |
| [[Security-Policy]] | Security policies |
| [[Audit-Reports]] | Audit evidence |

---

> **Template Standard:** Based on CyBOK v1
> **Usage:** Compliance is *evidence-based*. Document everything. Auditors want proof, not promises.
