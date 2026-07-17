---
document_type: Penetration Test Report
version: "1.0"
status: Draft
author: "[External Vendor / Security Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [penetration-test, ethical-hacking, cyberok, swebok]
standard_ref:
  - CyBOK v1 — Security Testing
  - SWEBOK v4 — Security
  - OWASP Testing Guide
---

# Penetration Test Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Authorized simulated attack to identify exploitable vulnerabilities — the ultimate security validation.

## 2. Engagement Summary

| Field | Detail |
|-------|--------|
| [Tester] | [Internal / External Vendor Name] |
| [Test Date] | [YYYY-MM-DD to YYYY-MM-DD] |
| [Scope] | [Full application + API + infrastructure] |
| [Methodology] | [OWASP Testing Guide + PTES] |
| [Approach] | [Black-box (no prior knowledge)] |
| [Credentials] | [Test accounts provided] |

## 3. Executive Summary

| Metric | Value |
|--------|-------|
| [Total findings] | [X] |
| [Critical] | [X] |
| [High] | [X] |
| [Medium] | [X] |
| [Low] | [X] |
| [Overall risk rating] | 🟢 Low / 🟡 Medium / 🟠 High / 🔴 Critical |

## 4. Findings

### Critical

| ID | Finding | CVSS | Impact | Exploitability | Status |
|----|--------|------|--------|---------------|--------|
| [PT-001] | [Finding description] | [9.1] | [Full system compromise] | [Easy] | ✅ Fixed |

### High

| ID | Finding | CVSS | Impact | Exploitability | Status |
|----|--------|------|--------|---------------|--------|
| [PT-002] | [Finding description] | [7.5] | [Data exposure] | [Moderate] | ✅ Fixed |

### Medium

| ID | Finding | CVSS | Impact | Exploitability | Status |
|----|--------|------|--------|---------------|--------|
| [PT-003] | [Finding description] | [5.3] | [Information disclosure] | [Moderate] | ⬜ Open |

### Low

| ID | Finding | CVSS | Impact | Exploitability | Status |
|----|--------|------|--------|---------------|--------|
| [PT-004] | [Finding description] | [3.1] | [Minor information leak] | [Difficult] | ⬜ Open |

## 5. Attack Narrative

| Phase | Activity | Result |
|-------|---------|--------|
| [Reconnaissance] | [OSINT, subdomain enumeration, port scanning] | [X services identified] |
| [Scanning] | [Vulnerability scanning, service enumeration] | [X vulnerabilities found] |
| [Exploitation] | [Attempted exploitation of findings] | [X successful exploits] |
| [Post-Exploitation] | [Privilege escalation, lateral movement] | [Limited success] |
| [Reporting] | [Documentation of findings] | [This report] |

## 6. Recommendations

| # | Finding | Recommendation | Priority | Effort |
|---|--------|---------------|---------|--------|
| 1 | [PT-001] | [Immediate fix required] | 🔴 | [X days] |
| 2 | [PT-002] | [Fix within 1 week] | 🟡 | [X days] |
| 3 | [PT-003] | [Fix within 1 month] | 🟢 | [X days] |
| 4 | [PT-004] | [Fix when convenient] | 🟢 | [X hours] |

## 7. Retest Results

| Finding | Original | Retest | Status |
|---------|---------|--------|--------|
| [PT-001] | [Vulnerable] | [Fixed] | ✅ |
| [PT-002] | [Vulnerable] | [Fixed] | ✅ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[DAST-Report]] | Automated testing complement |
| [[SAST-Report]] | Static analysis complement |
| [[Security-Test-Report]] | Overall security testing |

---

> **Template Standard:** Based on CyBOK v1, OWASP Testing Guide
> **Usage:** Pen test annually, before major releases, and after significant changes. Retest critical/high findings within 30 days.
