---
document_type: SQAP (Software Quality Assurance Plan)
version: "1.0"
status: Draft
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [sqap, quality-assurance, swebok, ieee-730, iso-9001]
standard_ref:
  - SWEBOK v4 — Quality Assurance
  - IEEE 730-2014 — Software Quality Assurance Processes
  - ISO/IEC/IEEE 90003 — Quality Assurance
  - ISO 9001 — Quality Management
---

# SQAP (Software Quality Assurance Plan)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [QA Lead] |
| Approvals | [PM, Tech Lead, QA Lead] |

### Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Manager | | | |
| Technical Lead | | | |
| QA Lead | | | |

---

## 1. Purpose

> Defines the quality assurance activities, standards, and responsibilities for the project, in accordance with IEEE 730-2014 process requirements.

## 2. SQA Independence

> Per IEEE 730-2014, the SQA function must be organizationally independent from development to ensure objective assessment.

| Independence Aspect | Implementation |
|--------------------|---------------| 
| **Reporting Structure** | [QA Lead reports to: Quality Director / PM, not Dev Lead] |
| **Technical Independence** | [QA team has separate evaluation authority: QA can block release] |
| **Budget Independence** | [QA budget is a separate line item, not controlled by Dev] |
| **Conflict Resolution** | [If QA and Dev disagree: escalate to [CCB / Steering Committee]] |

> **Critical:** SQA independence means QA can report non-compliance without fear of reprisal. If QA reports to the development manager, independence is compromised.

## 3. Quality Objectives

| # | Objective | Measurement | Target |
|---|----------|-----------|--------|
| 1 | [Deliver defect-free software] | [Defect density] | [< 2 defects/feature] |
| 2 | [Meet all requirements] | [Requirements coverage] | [100%] |
| 3 | [Follow coding standards] | [Linting pass rate] | [100%] |
| 4 | [Complete on time] | [Schedule variance] | [< 5%] |
| 5 | [Stay within budget] | [Cost variance] | [< 10%] |

## 4. Quality Standards

| Standard | Applicability | Compliance |
|---------|-------------|-----------|
| [ISO 9001] | [Quality management system] | [Compliant] |
| [ISO/IEC/IEEE 90003] | [Software quality assurance] | [Compliant] |
| [IEEE 730-2014] | [SQA process requirements] | [Compliant] |
| [WCAG 2.1 AA] | [Accessibility] | [Compliant] |
| [OWASP Top 10] | [Security] | [Compliant] |

## 5. QA Activities

| Activity | Phase | Frequency | Responsible | Standards |
|---------|-------|----------|-----------|----------|
| [Requirements Review] | [Requirements] | [Per sprint] | [BA + QA] | [[Review-Records]] |
| [Design Review] | [Design] | [Per sprint] | [Architect + QA] | [[Review-Records]] |
| [Code Review] | [Construction] | [Every PR] | [Developers] | [[Coding-Standards]] |
| [Static Analysis] | [Construction] | [Every commit] | [CI/CD] | [[Static-Analysis-Reports]] |
| [Unit Testing] | [Construction] | [Every commit] | [Developers] | [[Test-Strategy]] |
| [Integration Testing] | [Testing] | [Every PR] | [QA] | [[Test-Plan]] |
| [System Testing] | [Testing] | [Per sprint] | [QA] | [[Test-Plan]] |
| [UAT] | [Testing] | [Pre-release] | [Business] | [[UAT-Sign-off]] |
| [Performance Testing] | [Testing] | [Per release] | [QA] | [[Performance-Test-Report]] |
| [Security Testing] | [Testing] | [Per release] | [Security] | [[Security-Test-Report]] |

## 6. Quality Metrics

| Metric | Definition | Target | Collection |
|--------|-----------|--------|-----------|
| [Defect Density] | [Defects per feature] | [< 2] | [Defect tracking] |
| [Test Coverage] | [Code coverage %] | [≥ 80%] | [Jest] |
| [Requirements Coverage] | [Requirements tested %] | [100%] | [[Traceability-Matrix-Req-Tests]] |
| [Code Review Coverage] | [PRs reviewed %] | [100%] | [GitHub] |
| [Static Analysis] | [Linting errors] | [0] | [ESLint] |

## 7. Non-Compliance Process

```mermaid
flowchart TD
    DETECT[Non-Compliance<br>Detected] --> REPORT[Report to<br>QA Lead]
    REPORT --> ANALYZE[Analyze<br>Root Cause]
    ANALYZE --> CORRECTIVE[Corrective<br>Action]
    CORRECTIVE --> VERIFY[Verify<br>Effectiveness]
    VERIFY --> CLOSE[Close<br>Issue]

    style DETECT fill:#f44336,color:#fff
    style CLOSE fill:#4CAF50,color:#fff
```

## 8. Supplier and Vendor Quality Control

> Per IEEE 730-2014, if external vendors or contractors deliver components, their work must be subject to SQA oversight.

| Supplier Type | SQA Activity | Acceptance Criteria |
|---------------|-------------|---------------------|
| [Contractor developers] | [Code review by internal team] | [Meets [[Coding-Standards]], passes static analysis] |
| [Third-party APIs] | [Integration testing + SLA review] | [Meets NFR performance targets, documented SLA] |
| [Open-source dependencies] | [License check + security scan] | [No GPL violations, no known CVEs] |
| [Outsourced testing] | [Review test results, verify coverage] | [Test plan approved, exit criteria met] |
| [Cloud provider] | [Review SLA + compliance certifications] | [ISO 27001 certified, uptime ≥ 99.9%] |

## 9. Quality Records Management

> Per IEEE 730-2014, quality records must be collected, maintained, and retained as evidence of SQA activities.

| Record Type | Storage | Retention | Access |
|-------------|---------|-----------|--------|
| [Review Records] | [Repository / Wiki] | [Project + 7 years] | [Team] |
| [Test Results] | [CI/CD system] | [Project + 7 years] | [QA + Dev] |
| [Defect Reports] | [Defect tracking system] | [Project + 7 years] | [QA + Dev] |
| [Audit Reports] | [Document repository] | [Permanent] | [QA Lead + Management] |
| [SQA Reports] | [Document repository] | [Project + 7 years] | [Management] |
| [Non-compliance Records] | [Document repository] | [Project + 7 years] | [QA Lead + Management] |

## 10. SQA Training

| Training | Who | When | Duration |
|----------|-----|------|----------|
| [Quality process overview] | [All team members] | [Project kickoff] | [2 hours] |
| [Coding standards] | [Developers] | [Onboarding + per update] | [4 hours] |
| [Code review techniques] | [Reviewers] | [Before first review] | [2 hours] |
| [Static analysis tools] | [Developers + QA] | [Before CI/CD setup] | [2 hours] |
| [Defect reporting] | [QA + Developers] | [Onboarding] | [1 hour] |
| [ISO 9001 / IEEE 730 awareness] | [QA Lead] | [Annual] | [4 hours] |

## 11. QA Tools

| Tool | Purpose | Integration |
|------|---------|-----------|
| [Jest] | [Unit testing] | [CI/CD] |
| [Playwright] | [E2E testing] | [CI/CD] |
| [ESLint] | [Static analysis] | [Pre-commit] |
| [SonarQube] | [Code quality] | [CI/CD] |
| [axe] | [Accessibility] | [CI/CD] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Test-Strategy]] | Testing approach |
| [[Quality-Metrics-Dashboard]] | Quality metrics |
| [[QMS-Documentation]] | Quality management system |

---

> **Template Standard:** Based on SWEBOK v4, IEEE 730-2014, ISO/IEC/IEEE 90003, ISO 9001
> **Usage:** The SQAP is the *quality contract*. Everyone knows what quality means, how it's measured, and how non-compliance is handled. SQA independence ensures objectivity: QA can block a release if quality standards are not met.
