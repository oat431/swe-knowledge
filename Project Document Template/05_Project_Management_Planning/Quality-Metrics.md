---
document_type: Quality Metrics
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
qa_owner: "[QA Lead]"
classification: "Internal / Confidential"
tags: [quality-metrics, kpi, defect-density, pmbok, iso-25010]
standard_ref:
  - PMBOK v8 — Planning (Quality Management)
  - ISO/IEC 25010 — SQuaRE (Quality Model)
  - IEEE 1633 — Software Reliability
---

# Quality Metrics

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document defines, tracks, and reports quality metrics for the project. Metrics are grouped by quality characteristic per ISO 25010.

## 2. Quality Metrics Dashboard

| Metric | Target | Current | Status | Trend |
|--------|--------|---------|--------|-------|
| [Defect Density] | [<X/feature] | [Y/feature] | 🟢🟡🔴 | ↑↓→ |
| [Defect Escape Rate] | [<5%] | [X%] | 🟢🟡🔴 | ↑↓→ |
| [Code Coverage] | [≥80%] | [X%] | 🟢🟡🔴 | ↑↓→ |
| [Code Review Coverage] | [100%] | [X%] | 🟢🟡🔴 | ↑↓→ |
| [Test Case Pass Rate] | [≥95%] | [X%] | 🟢🟡🔴 | ↑↓→ |
| [Requirements Traceability] | [100%] | [X%] | 🟢🟡🔴 | ↑↓→ |
| [Sprint Velocity Variance] | [±10%] | [X%] | 🟢🟡🔴 | ↑↓→ |
| [Open Critical Defects] | [0] | [X] | 🟢🟡🔴 | ↑↓→ |

## 3. Metrics by Quality Characteristic

### 3.1 Functional Suitability

| Metric | Definition | Target | Measurement | Frequency |
|--------|-----------|--------|-------------|-----------|
| [Functional Completeness] | [% of specified functions implemented] | [100%] | [RTM — functions implemented / specified] | [Per sprint] |
| [Functional Correctness] | [% of functions producing correct results] | [≥99%] | [Test pass rate] | [Per test cycle] |
| [Functional Appropriateness] | [% of functions facilitating tasks] | [≥90%] | [User feedback] | [UAT] |

### 3.2 Performance Efficiency

| Metric | Definition | Target | Measurement | Frequency |
|--------|-----------|--------|-------------|-----------|
| [Response Time — Page] | [Time to render page] | [<2s] | [95th percentile] | [Per sprint] |
| [Response Time — API] | [Time to return API response] | [<1s] | [95th percentile] | [Per sprint] |
| [Throughput] | [Requests processed per hour] | [≥200] | [Load test] | [Pre-release] |
| [Resource Utilization] | [CPU/Memory usage under load] | [<80%] | [Monitoring] | [Continuous] |

### 3.3 Compatibility

| Metric | Definition | Target | Measurement | Frequency |
|--------|-----------|--------|-------------|-----------|
| [Browser Compatibility] | [% of supported browsers working correctly] | [100%] | [Cross-browser test] | [Per release] |
| [Device Compatibility] | [% of target devices working correctly] | [100%] | [Cross-device test] | [Per release] |
| [Integration Success] | [% of integrations working correctly] | [100%] | [Integration test] | [Per sprint] |

### 3.4 Usability

| Metric | Definition | Target | Measurement | Frequency |
|--------|-----------|--------|-------------|-----------|
| [Task Completion Rate] | [% of users completing tasks successfully] | [≥90%] | [Usability test] | [Pre-release] |
| [Task Completion Time] | [Average time to complete tasks] | [<5 min (customer), <3 min (ops)] | [Usability test] | [Pre-release] |
| [User Error Rate] | [% of user errors during tasks] | [<5%] | [Usability test] | [Pre-release] |
| [User Satisfaction (SUS)] | [System Usability Scale score] | [≥80] | [Post-task survey] | [Pre-release] |

### 3.5 Reliability

| Metric | Definition | Target | Measurement | Frequency |
|--------|-----------|--------|-------------|-----------|
| [Availability] | [System uptime percentage] | [99.9%] | [Monitoring] | [Monthly] |
| [MTBF] | [Mean Time Between Failures] | [≥720 hours] | [Incident logs] | [Monthly] |
| [MTTR] | [Mean Time To Repair] | [<30 minutes] | [Incident logs] | [Per incident] |
| [Defect Density] | [Defects per feature] | [<X] | [Defect report] | [Per sprint] |

### 3.6 Security

| Metric | Definition | Target | Measurement | Frequency |
|--------|-----------|--------|-------------|-----------|
| [Critical Vulnerabilities] | [Count of critical security issues] | [0] | [SAST/DAST] | [Per build/weekly] |
| [High Vulnerabilities] | [Count of high security issues] | [0] | [SAST/DAST] | [Per build/weekly] |
| [Vulnerability Remediation Time] | [Time to fix critical/high vulns] | [<24h critical, <7d high] | [Security tracking] | [Per vuln] |

### 3.7 Maintainability

| Metric | Definition | Target | Measurement | Frequency |
|--------|-----------|--------|-------------|-----------|
| [Code Coverage] | [% of code covered by tests] | [≥80%] | [Coverage tool] | [Per build] |
| [Code Complexity] | [Cyclomatic complexity per function] | [<10] | [Static analysis] | [Per sprint] |
| [Code Duplication] | [% of duplicated code] | [<5%] | [Static analysis] | [Per sprint] |
| [Technical Debt Ratio] | [Time to fix debt / Total dev time] | [<10%] | [SonarQube] | [Per sprint] |

### 3.8 Portability

| Metric | Definition | Target | Measurement | Frequency |
|--------|-----------|--------|-------------|-----------|
| [Deployment Success Rate] | [% of deployments without rollback] | [100%] | [Deployment logs] | [Per deployment] |
| [Environment Parity] | [% of config consistent across environments] | [100%] | [Config audit] | [Per deployment] |

## 4. Defect Metrics

### 4.1 Defect Summary

| Metric | Current Sprint | Previous Sprint | Project Total |
|--------|---------------|----------------|--------------|
| [Defects Found] | [X] | [Y] | [Z] |
| [Defects Fixed] | [X] | [Y] | [Z] |
| [Defects Open] | [X] | [Y] | [Z] |
| [Defects Deferred] | [X] | [Y] | [Z] |
| [Defect Density] | [X/feature] | [Y/feature] | [Z/feature] |

### 4.2 Defect Trend

| Sprint | Found | Fixed | Open | Density | Trend |
|--------|-------|-------|------|---------|-------|
| [Sprint 1] | [X] | [Y] | [Z] | [W/feature] | — |
| [Sprint 2] | [X] | [Y] | [Z] | [W/feature] | ↓ |
| [Sprint 3] | [X] | [Y] | [Z] | [W/feature] | ↓ |
| [Sprint 4] | [X] | [Y] | [Z] | [W/feature] | ↓ |
| [Sprint 5] | [X] | [Y] | [Z] | [W/feature] | ↓ |

### 4.3 Defects by Severity

| Severity | Found | Fixed | Open | % of Total |
|----------|-------|-------|------|-----------|
| 🔴 Critical | [X] | [Y] | [Z] | [%] |
| 🟠 High | [X] | [Y] | [Z] | [%] |
| 🟡 Medium | [X] | [Y] | [Z] | [%] |
| 🟢 Low | [X] | [Y] | [Z] | [%] |
| **Total** | **[Sum]** | **[Sum]** | **[Sum]** | **100%** |

### 4.4 Defects by Source

| Source | Count | % of Total |
|--------|-------|-----------|
| [Requirements] | [X] | [Y%] |
| [Design] | [X] | [Y%] |
| [Code] | [X] | [Y%] |
| [Environment] | [X] | [Y%] |
| [Unknown] | [X] | [Y%] |

## 5. Quality Gate Criteria

| Gate | Criteria | Current | Status |
|------|---------|---------|--------|
| [Sprint Review] | [All sprint tests pass, no 🔴 open] | [Status] | ✅❌ |
| [System Test Complete] | [All system tests pass, coverage ≥80%] | [Status] | ✅❌ |
| [UAT Complete] | [UAT sign-off, zero 🔴🟠 open] | [Status] | ✅❌ |
| [Go-Live] | [All quality gates passed, zero 🔴 open] | [Status] | ✅❌ |

## 6. Quality Reporting

| Report | Audience | Frequency | Content |
|--------|----------|-----------|---------|
| [Sprint Quality Report] | [Project team] | Per sprint | [Defects, coverage, velocity] |
| [Monthly Quality Report] | [Sponsor, Steering] | Monthly | [Trends, KPIs, risks] |
| [Release Quality Report] | [All stakeholders] | Per release | [Full quality assessment] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Quality Management Plan]] | How quality is managed |
| [[Nonfunctional Requirements Catalog]] | Quality attribute requirements |
| [[Test Plan]] | Testing strategy |
| [[Defect Report]] | Detailed defect tracking |

---

> **Template Standard:** Based on PMBOK v8, ISO/IEC 25010, IEEE 1633
> **Usage:** Metrics drive decisions — if a metric is consistently off target, investigate root cause and take corrective action. Don't just collect metrics; use them.
