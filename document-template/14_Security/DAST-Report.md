---
document_type: DAST Report
version: "1.0"
status: Active
author: "[Security Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [dast, dynamic-analysis, runtime-security, cyberok, swebok]
standard_ref:
  - CyBOK v1 — Secure Software Engineering
  - SWEBOK v4 — Security
---

# DAST Report (Dynamic Application Security Testing)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Tests the running application for security vulnerabilities — finding issues that only appear at runtime.

## 2. DAST Configuration

| Field | Detail |
|-------|--------|
| [Tool] | [OWASP ZAP / Burp Suite / Nuclei] |
| [Scan Date] | [YYYY-MM-DD] |
| [Target] | [https://staging.project.com] |
| [Authentication] | [Authenticated scan with test account] |
| [Scope] | [Full application, all endpoints] |
| [Scan Type] | [Active + Passive] |

## 3. Findings Summary

| Severity | Found | Fixed | Remaining | Status |
|---------|-------|-------|----------|--------|
| 🔴 Critical | [0] | [0] | [0] | ✅ Clean |
| 🟠 High | [1] | [1] | [0] | ✅ Clean |
| 🟡 Medium | [2] | [1] | [1] | 🟡 Acceptable |
| 🟢 Low | [4] | [3] | [1] | 🟢 Acceptable |
| ℹ️ Informational | [3] | [2] | [1] | 🟢 Acceptable |
| **Total** | **[10]** | **[7]** | **[3]** | **✅** |

## 4. Findings

### High

| ID | Severity | URL | Description | Status |
|----|---------|-----|-----------|--------|
| [DAST-001] | 🟠 High | [/api/requests] | [SQL injection via search parameter] | ✅ Fixed |

### Medium

| ID | Severity | URL | Description | Status |
|----|---------|-----|-----------|--------|
| [DAST-002] | 🟡 Medium | [/api/auth/login] | [Missing rate limiting on login] | ✅ Fixed |
| [DAST-003] | 🟡 Medium | [/api/requests] | [Verbose error messages exposing stack trace] | ⬜ Open |

### Low

| ID | Severity | URL | Description | Status |
|----|---------|-----|-----------|--------|
| [DAST-004] | 🟢 Low | [Global] | [Missing security headers (X-Frame-Options)] | ✅ Fixed |
| [DAST-005] | 🟢 Low | [Global] | [Missing security headers (X-Content-Type-Options)] | ✅ Fixed |
| [DAST-006] | 🟢 Low | [/api/requests] | [CORS misconfiguration] | ✅ Fixed |
| [DAST-007] | 🟢 Low | [/api/auth] | [Cookie without Secure flag] | ⬜ Open |

### Informational

| ID | Severity | URL | Description | Status |
|----|---------|-----|-----------|--------|
| [DAST-008] | ℹ️ Info | [Global] | [Server version disclosed] | ✅ Fixed |
| [DAST-009] | ℹ️ Info | [/api] | [API documentation publicly accessible] | ✅ Fixed |
| [DAST-010] | ℹ️ Info | [Global] | [HTTP Strict Transport Security not set] | ⬜ Open |

## 5. OWASP Top 10 Coverage

| OWASP Category | Tested | Findings | Status |
|---------------|--------|---------|--------|
| [A01 — Broken Access Control] | ✅ | [0] | ✅ |
| [A02 — Cryptographic Failures] | ✅ | [0] | ✅ |
| [A03 — Injection] | ✅ | [1] | ✅ Fixed |
| [A04 — Insecure Design] | ✅ | [0] | ✅ |
| [A05 — Security Misconfiguration] | ✅ | [3] | 🟡 |
| [A06 — Vulnerable Components] | ✅ | [0] | ✅ |
| [A07 — Auth Failures] | ✅ | [1] | ✅ Fixed |
| [A08 — Data Integrity] | ✅ | [0] | ✅ |
| [A09 — Logging Failures] | ✅ | [1] | 🟡 |
| [A10 — SSRF] | ✅ | [0] | ✅ |

## 6. Remediation Plan

| # | Finding | Remediation | Owner | Status |
|---|--------|-----------|-------|--------|
| 1 | [DAST-003: Verbose errors] | [Generic error messages] | [Dev] | ⬜ |
| 2 | [DAST-007: Cookie flags] | [Add Secure flag] | [Dev] | ⬜ |
| 3 | [DAST-010: HSTS] | [Add HSTS header] | [DevOps] | ⬜ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SAST-Report]] | Static analysis complement |
| [[Penetration-Test-Report]] | Manual testing |
| [[Security-Test-Report]] | Overall security testing |

---

> **Template Standard:** Based on CyBOK v1, SWEBOK v4
> **Usage:** DAST finds runtime issues that SAST misses. Run before every release against staging.
