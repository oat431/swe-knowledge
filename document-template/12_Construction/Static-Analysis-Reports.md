---
document_type: Static Analysis Reports
version: "1.0"
status: Active
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [static-analysis, linting, security, swebok]
standard_ref:
  - SWEBOK v4 — Construction
---

# Static Analysis Reports

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Static analysis catches code quality issues, security vulnerabilities, and style violations before they reach production.

## 2. Analysis Tools

| Tool | Purpose | Frequency | Integration |
|------|---------|----------|-----------|
| [ESLint] | [Code quality, style] | [Every commit] | [CI/CD, pre-commit] |
| [Prettier] | [Code formatting] | [Every commit] | [CI/CD, pre-commit] |
| [TypeScript] | [Type checking] | [Every commit] | [CI/CD] |
| [npm audit] | [Dependency vulnerabilities] | [Every build] | [CI/CD] |
| [SonarQube] | [Code quality, security] | [Every PR] | [CI/CD] |
| [Snyk] | [Dependency security] | [Daily] | [CI/CD, scheduled] |

## 3. Quality Gates

| Gate | Tool | Threshold | Action on Fail |
|------|------|----------|---------------|
| [Linting] | [ESLint] | [0 errors] | [Block merge] |
| [Formatting] | [Prettier] | [0 violations] | [Block merge] |
| [Type Check] | [TypeScript] | [0 errors] | [Block merge] |
| [Test Coverage] | [Jest] | [≥ 80%] | [Block merge] |
| [Security] | [npm audit] | [0 critical/high] | [Block merge] |
| [Code Smells] | [SonarQube] | [< 10] | [Warn] |
| [Duplication] | [SonarQube] | [< 5%] | [Warn] |

## 4. Report Template

### Report: [YYYY-MM-DD]

| Metric | Value | Threshold | Status |
|--------|-------|----------|--------|
| [ESLint Errors] | [X] | [0] | ✅❌ |
| [TypeScript Errors] | [X] | [0] | ✅❌ |
| [Test Coverage] | [X%] | [≥ 80%] | ✅❌ |
| [Vulnerabilities — Critical] | [X] | [0] | ✅❌ |
| [Vulnerabilities — High] | [X] | [0] | ✅❌ |
| [Code Smells] | [X] | [< 10] | ✅❌ |
| [Duplication] | [X%] | [< 5%] | ✅❌ |

## 5. CI/CD Integration

```yaml
# GitHub Actions example
- name: Lint
  run: npm run lint

- name: Type Check
  run: npm run type-check

- name: Test
  run: npm test -- --coverage

- name: Security Audit
  run: npm audit --audit-level=high

- name: SonarQube
  uses: sonarcloud/scan@master
```

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Coding-Standards]] | Standards being enforced |
| [[Build-Scripts]] | Build pipeline |
| [[SBOM]] | Dependency scanning |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Static analysis is *automated quality*. If it's not in CI, it's not enforced. Block merge on critical issues.
