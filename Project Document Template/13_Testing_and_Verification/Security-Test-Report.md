---
document_type: Security Test Report
version: "1.0"
status: Draft
author: "[QA Lead / Security Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [security-testing, vulnerability, pen-test, swebok, owasp]
standard_ref:
  - SWEBOK v4 — Testing
  - OWASP Testing Guide
---

# Security Test Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Reports security testing results — vulnerability scanning, penetration testing, and compliance verification.

## 2. Security Test Summary

| Field | Detail |
|-------|--------|
| [Test Date] | [YYYY-MM-DD] |
| [Test Type] | [Automated scan + Manual pen test] |
| [Tools] | [OWASP ZAP, Snyk, npm audit] |
| [Tester] | [Internal / External] |
| [Scope] | [Full application] |
| [Overall Risk] | 🟢 Low / 🟡 Medium / 🟠 High / 🔴 Critical |

## 3. Vulnerability Summary

| Severity | Found | Fixed | Remaining | Status |
|---------|-------|-------|----------|--------|
| 🔴 Critical | [0] | [0] | [0] | ✅ Clean |
| 🟠 High | [1] | [1] | [0] | ✅ Clean |
| 🟡 Medium | [3] | [2] | [1] | 🟡 Acceptable |
| 🟢 Low | [5] | [3] | [2] | 🟢 Acceptable |
| **Total** | **[9]** | **[6]** | **[3]** | **🟡** |

## 4. OWASP Top 10 Assessment

| # | Category | Status | Notes |
|---|---------|--------|-------|
| A01 | [Broken Access Control] | ✅ Pass | [RBAC implemented, tested] |
| A02 | [Cryptographic Failures] | ✅ Pass | [TLS 1.3, AES-256, bcrypt] |
| A03 | [Injection] | ✅ Pass | [Parameterized queries, input validation] |
| A04 | [Insecure Design] | ✅ Pass | [Threat modeling done] |
| A05 | [Security Misconfiguration] | ✅ Pass | [Security headers, no debug mode] |
| A06 | [Vulnerable Components] | 🟡 Minor | [1 medium dependency issue] |
| A07 | [Auth Failures] | ✅ Pass | [MFA, rate limiting, lockout] |
| A08 | [Data Integrity Failures] | ✅ Pass | [Input validation, CSRF protection] |
| A09 | [Logging Failures] | ✅ Pass | [Audit logging, no sensitive data in logs] |
| A10 | [SSRF] | ✅ Pass | [No server-side request functionality] |

## 5. Findings

### Finding 1: Medium — Dependency Vulnerability

| Field | Detail |
|-------|--------|
| **ID** | [SEC-001] |
| **Severity** | 🟡 Medium |
| **Category** | [A06 — Vulnerable Components] |
| **Component** | [axios v1.6.0] |
| **Vulnerability** | [CVE-2024-XXXX — SSRF in redirect handling] |
| **Impact** | [Potential SSRF via crafted redirect] |
| **Remediation** | [Update to axios v1.6.2+] |
| **Status** | ✅ Fixed |

### Finding 2: Low — Missing Rate Limiting

| Field | Detail |
|-------|--------|
| **ID** | [SEC-002] |
| **Severity** | 🟢 Low |
| **Category** | [A07 — Auth Failures] |
| **Description** | [Rate limiting not applied to password reset endpoint] |
| **Impact** | [Potential brute force on password reset] |
| **Remediation** | [Add rate limiting to all auth endpoints] |
| **Status** | ⬜ Open |

## 6. Penetration Test Results

| Test | Result | Notes |
|------|--------|-------|
| [SQL Injection] | ✅ Pass | [Parameterized queries verified] |
| [XSS] | ✅ Pass | [Output encoding verified] |
| [CSRF] | ✅ Pass | [CSRF tokens implemented] |
| [Authentication Bypass] | ✅ Pass | [Cannot bypass login] |
| [Authorization Bypass] | ✅ Pass | [Cannot access other users' data] |
| [Session Management] | ✅ Pass | [Secure session handling] |
| [File Upload] | ✅ Pass | [Type/size validation enforced] |
| [API Security] | ✅ Pass | [Rate limiting, input validation] |

## 7. Security Recommendations

| # | Recommendation | Priority | Status |
|---|---------------|---------|--------|
| 1 | [Update axios to v1.6.2+] | 🔴 | ✅ Done |
| 2 | [Add rate limiting to all auth endpoints] | 🟡 | ⬜ Open |
| 3 | [Implement Content Security Policy] | 🟡 | ⬜ Open |
| 4 | [Add security headers (HSTS, X-Frame-Options)] | 🟢 | ⬜ Open |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Security-Controls]] | Security implementation |
| [[Test-Report]] | Overall test results |
| [[Vulnerability-Assessment]] | Detailed vulnerability analysis |

---

> **Template Standard:** Based on SWEBOK v4, OWASP Testing Guide
> **Usage:** Security testing is *not optional*. Run automated scans in CI/CD. Do pen testing before every major release.
