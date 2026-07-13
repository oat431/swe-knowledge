---
document_type: SCA Report
version: "1.0"
status: Active
author: "[Security Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [sca, software-composition-analysis, dependencies, cyberok]
standard_ref:
  - CyBOK v1 — Secure Software Engineering
---

# SCA Report (Software Composition Analysis)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Scans third-party and open-source components for known vulnerabilities and license compliance issues.

## 2. SCA Configuration

| Field | Detail |
|-------|--------|
| [Tool] | [Snyk / npm audit / OWASP Dependency-Check] |
| [Scan Date] | [YYYY-MM-DD] |
| [Package Manager] | [npm] |
| [Total Dependencies] | [X direct, Y transitive] |

## 3. Vulnerability Summary

| Severity | Found | Fixed | Remaining | Status |
|---------|-------|-------|----------|--------|
| 🔴 Critical | [0] | [0] | [0] | ✅ Clean |
| 🟠 High | [1] | [1] | [0] | ✅ Clean |
| 🟡 Medium | [2] | [1] | [1] | 🟡 Acceptable |
| 🟢 Low | [3] | [2] | [1] | 🟢 Acceptable |
| **Total** | **[6]** | **[4]** | **[2]** | **✅** |

## 4. Vulnerabilities

| ID | Package | Version | Severity | CVE | Description | Fix Version | Status |
|----|---------|---------|---------|-----|-----------|------------|--------|
| [SCA-001] | [axios] | [1.6.0] | 🟠 High | [CVE-2024-XXXX] | [SSRF in redirect handling] | [1.6.2] | ✅ Fixed |
| [SCA-002] | [jsonwebtoken] | [9.0.0] | 🟡 Medium | [CVE-2024-XXXX] | [Algorithm confusion] | [9.0.2] | ✅ Fixed |
| [SCA-003] | [express] | [4.18.0] | 🟡 Medium | [CVE-2024-XXXX] | [Open redirect] | [4.18.2] | ⬜ Open |
| [SCA-004] | [lodash] | [4.17.20] | 🟢 Low | [CVE-2021-XXXX] | [Prototype pollution] | [4.17.21] | ✅ Fixed |
| [SCA-005] | [minimist] | [1.2.5] | 🟢 Low | [CVE-2021-XXXX] | [Prototype pollution] | [1.2.8] | ✅ Fixed |
| [SCA-006] | [node-fetch] | [2.6.0] | 🟢 Low | [CVE-2022-XXXX] | [Information exposure] | [2.6.7] | ⬜ Open |

## 5. License Analysis

| License | Packages | Status | Risk |
|---------|---------|--------|------|
| [MIT] | [X] | ✅ Approved | [None] |
| [Apache-2.0] | [X] | ✅ Approved | [None] |
| [ISC] | [X] | ✅ Approved | [None] |
| [BSD-2-Clause] | [X] | ✅ Approved | [None] |
| [GPL-3.0] | [0] | ❌ Blocked | [Copyleft — not allowed] |

## 6. Dependency Health

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Outdated packages] | [X] | [< 10] | 🟢🟡🔴 |
| [Vulnerable packages] | [2] | [0] | 🟡 |
| [License issues] | [0] | [0] | ✅ |
| [Direct dependencies] | [X] | — | — |
| [Transitive dependencies] | [X] | — | — |

## 7. Remediation

| # | Package | Action | Owner | Due Date | Status |
|---|---------|--------|-------|---------|--------|
| 1 | [express] | [Update to 4.18.2] | [Dev] | [YYYY-MM-DD] | ⬜ |
| 2 | [node-fetch] | [Update to 2.6.7] | [Dev] | [YYYY-MM-DD] | ⬜ |

## 8. CI/CD Integration

```yaml
# GitHub Actions SCA step
- name: SCA Scan
  run: |
    npm audit --audit-level=high
    # Fail on critical/high vulnerabilities
    if npm audit --audit-level=critical; then
      echo "Critical vulnerabilities found"
      exit 1
    fi
```

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SAST-Report]] | Code-level analysis |
| [[Dependency-Manifest]] | Dependencies being scanned |
| [[SBOM]] | Software bill of materials |

---

> **Template Standard:** Based on CyBOK v1
> **Usage:** Run SCA on every build. Update vulnerable packages within SLA (Critical: 48h, High: 7d).
