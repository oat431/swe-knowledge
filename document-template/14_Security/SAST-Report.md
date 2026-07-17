---
document_type: SAST Report
version: "1.0"
status: Active
author: "[Security Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [sast, static-analysis, security-scanning, cyberok, swebok]
standard_ref:
  - CyBOK v1 — Secure Software Engineering
  - SWEBOK v4 — Security
---

# SAST Report (Static Application Security Testing)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Reports findings from static code analysis — identifying security vulnerabilities in source code without executing it.

## 2. SAST Configuration

| Field | Detail |
|-------|--------|
| [Tool] | [Semgrep / ESLint Security / SonarQube] |
| [Scan Date] | [YYYY-MM-DD] |
| [Codebase Version] | [Git SHA: abc123] |
| [Rulesets] | [OWASP Top 10, CWE Top 25, custom] |
| [Languages] | [TypeScript, JavaScript] |

## 3. Findings Summary

| Severity | Found | Fixed | Remaining | Status |
|---------|-------|-------|----------|--------|
| 🔴 Critical | [0] | [0] | [0] | ✅ Clean |
| 🟠 High | [1] | [1] | [0] | ✅ Clean |
| 🟡 Medium | [3] | [2] | [1] | 🟡 Acceptable |
| 🟢 Low | [5] | [3] | [2] | 🟢 Acceptable |
| **Total** | **[9]** | **[6]** | **[3]** | **✅** |

## 4. Findings

### Critical / High

| ID | Severity | CWE | File | Line | Description | Status |
|----|---------|-----|------|------|-----------|--------|
| [SAST-001] | 🟠 High | [CWE-89] | [api/requests.ts] | [45] | [Potential SQL injection in query builder] | ✅ Fixed |

### Medium

| ID | Severity | CWE | File | Line | Description | Status |
|----|---------|-----|------|------|-----------|--------|
| [SAST-002] | 🟡 Medium | [CWE-79] | [components/Form.tsx] | [23] | [Potential XSS via dangerouslySetInnerHTML] | ✅ Fixed |
| [SAST-003] | 🟡 Medium | [CWE-200] | [api/errors.ts] | [12] | [Stack trace exposed in error response] | ✅ Fixed |
| [SAST-004] | 🟡 Medium | [CWE-352] | [api/auth.ts] | [67] | [Missing CSRF protection on state-changing endpoint] | ⬜ Open |

### Low

| ID | Severity | CWE | File | Line | Description | Status |
|----|---------|-----|------|------|-----------|--------|
| [SAST-005] | 🟢 Low | [CWE-330] | [utils/token.ts] | [8] | [Weak random number generation for token] | ✅ Fixed |
| [SAST-006] | 🟢 Low | [CWE-521] | [config/auth.ts] | [15] | [Password minimum length < 12] | ✅ Fixed |
| [SAST-007] | 🟢 Low | [CWE-601] | [api/redirect.ts] | [34] | [Open redirect vulnerability] | ⬜ Open |
| [SAST-008] | 🟢 Low | [CWE-16] | [config/default.ts] | [5] | [Insecure default configuration] | ✅ Fixed |
| [SAST-009] | 🟢 Low | [CWE-532] | [utils/logger.ts] | [22] | [Sensitive data in log output] | ⬜ Open |

## 5. Trend Analysis

| Scan Date | Critical | High | Medium | Low | Total | Trend |
|-----------|---------|------|--------|-----|-------|-------|
| [Month 1] | [1] | [3] | [5] | [8] | [17] | — |
| [Month 2] | [0] | [2] | [4] | [6] | [12] | ↓ |
| [Month 3] | [0] | [1] | [3] | [5] | [9] | ↓ |
| **Current** | **[0]** | **[0]** | **[1]** | **[2]** | **[3]** | **↓** |

## 6. CI/CD Integration

```yaml
# GitHub Actions SAST step
- name: SAST Scan
  run: |
    npm install -g @semgrep/semgrep
    semgrep --config=owasp-top-ten --config=cwe-top-25 --json --output=sast-results.json
    # Fail on critical/high
    if jq -e '.results[] | select(.extra.severity == "ERROR")' sast-results.json; then
      echo "Critical SAST findings detected"
      exit 1
    fi
```

## 7. Remediation Plan

| # | Finding | Remediation | Owner | Due Date | Status |
|---|--------|-----------|-------|---------|--------|
| 1 | [SAST-004: CSRF] | [Add CSRF tokens] | [Dev] | [YYYY-MM-DD] | ⬜ |
| 2 | [SAST-007: Open redirect] | [Whitelist redirect URLs] | [Dev] | [YYYY-MM-DD] | ⬜ |
| 3 | [SAST-009: Sensitive data in logs] | [Mask sensitive fields] | [Dev] | [YYYY-MM-DD] | ⬜ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Secure-Coding-Guidelines]] | Standards being enforced |
| [[SCA-Report]] | Dependency scanning |
| [[DAST-Report]] | Dynamic testing |

---

> **Template Standard:** Based on CyBOK v1, SWEBOK v4
> **Usage:** Run SAST in CI/CD — fail the build on critical findings. Review findings weekly. Track trends.
