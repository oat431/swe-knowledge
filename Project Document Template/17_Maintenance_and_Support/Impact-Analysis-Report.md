---
document_type: Impact Analysis Report
version: "1.0"
status: Active
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [impact-analysis, change-impact, swebok]
standard_ref:
  - SWEBOK v4 — Maintenance
---

# Impact Analysis Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Analyzes the impact of proposed changes — what's affected, how much effort, and what risks are introduced.

## 2. Impact Analysis Template

### IA-XXX: [Change Title]

| Field | Detail |
|-------|--------|
| **Analysis ID** | [IA-XXX] |
| **Change Request** | [[MR-XXX]] |
| **Analyst** | [Name] |
| **Date** | [YYYY-MM-DD] |

### Change Description

> [Clear description of the proposed change]

### Impact Assessment

| Dimension | Impact | Details |
|---------|--------|---------|
| [Code Modules] | [X modules affected] | [List modules] |
| [Database] | [Schema changes / None] | [Details] |
| [API] | [Breaking / Non-breaking / None] | [Details] |
| [Tests] | [X tests need update] | [List test areas] |
| [Documentation] | [X docs need update] | [List docs] |
| [Dependencies] | [X dependencies affected] | [List dependencies] |
| [Users] | [User-facing / Internal / None] | [Details] |
| [Performance] | [Impact / None] | [Details] |
| [Security] | [Impact / None] | [Details] |

### Effort Estimation

| Task | Effort | Resources | Dependencies |
|------|--------|----------|-------------|
| [Development] | [X days] | [Y developers] | [None] |
| [Testing] | [X days] | [Y testers] | [Development] |
| [Documentation] | [X days] | [Y writers] | [Development] |
| [Deployment] | [X hours] | [Y DevOps] | [Testing] |
| **Total** | **[X days]** | | |

### Risk Assessment

| Risk | Probability | Impact | Mitigation |
|------|-----------|--------|-----------|
| [Regression in module X] | [Medium] | [High] | [Comprehensive regression testing] |
| [Performance degradation] | [Low] | [Medium] | [Performance testing] |
| [Breaking API consumers] | [Low] | [High] | [API versioning, communication] |

### Recommendation

| Field | Detail |
|-------|--------|
| [Approve?] | [Yes / No / Conditional] |
| [Rationale] | [Why] |
| [Conditions] | [If conditional] |

## 3. Impact Analysis Register

| ID | Change | Modules | Effort | Risk | Status |
|----|--------|---------|--------|------|--------|
| [IA-001] | [Add bulk upload] | [3] | [5 days] | [Medium] | ✅ Approved |
| [IA-002] | [New payment gateway] | [2] | [10 days] | [High] | ⏳ Pending |
| [IA-003] | [Update email template] | [1] | [1 day] | [Low] | ✅ Approved |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[MR-PR-Modification-Request]] | Change request being analyzed |
| [[Maintenance-Plan]] | Maintenance strategy |
| [[Risk-Register]] | Project risks |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Never estimate a change without impact analysis. "It's a small change" is the most expensive sentence in software.
