---
document_type: Technical Debt Register
version: "1.0"
status: Active
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [technical-debt, code-quality, swebok]
standard_ref:
  - SWEBOK v4 — Maintenance
---

# Technical Debt Register

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Tracks technical debt — shortcuts taken, suboptimal solutions, and areas needing improvement. What we owe the codebase.

## 2. Technical Debt Categories

| Category | Description | Example |
|---------|-----------|---------|
| [Code] | [Suboptimal code] | [Duplicated logic, no error handling] |
| [Architecture] | [Architectural shortcuts] | [Tight coupling, missing abstractions] |
| [Testing] | [Insufficient testing] | [Low coverage, missing edge cases] |
| [Documentation] | [Missing/outdated docs] | [Undocumented APIs, outdated README] |
| [Infrastructure] | [Infrastructure gaps] | [No staging environment, manual deploy] |
| [Security] | [Security gaps] | [Missing input validation, no rate limiting] |
| [Performance] | [Performance issues] | [N+1 queries, missing indexes] |

## 3. Debt Register

| ID | Category | Description | Impact | Effort | Priority | Status |
|----|---------|-----------|--------|--------|---------|--------|
| [TD-001] | [Code] | [Duplicated validation logic in 3 services] | [Maintainability] | [2 days] | 🟡 | ⬜ Open |
| [TD-002] | [Testing] | [No integration tests for payment flow] | [Reliability] | [3 days] | 🟡 | ⬜ Open |
| [TD-003] | [Performance] | [N+1 query in request list endpoint] | [Performance] | [1 day] | 🟡 | 🔄 In Progress |
| [TD-004] | [Documentation] | [API docs outdated for v1.1] | [Developer experience] | [1 day] | 🟢 | ⬜ Open |
| [TD-005] | [Infrastructure] | [No staging environment parity] | [Testing reliability] | [2 days] | 🟡 | ⬜ Open |
| [TD-006] | [Security] | [Missing rate limiting on auth endpoints] | [Security] | [1 day] | 🔴 | ⬜ Open |

## 4. Debt Metrics

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Total debt items] | [6] | [< 10] | 🟢 |
| [Critical debt] | [1] | [0] | 🟡 |
| [Debt trend] | [Stable] | [Decreasing] | 🟡 |
| [Debt resolution rate] | [X/month] | [≥ 2/month] | 🟢🟡🔴 |
| [Debt creation rate] | [X/month] | [< resolution rate] | 🟢🟡🔴 |

## 5. Debt Reduction Plan

| Sprint | Debt Items | Effort | Focus Area |
|--------|-----------|--------|-----------|
| [Sprint 5] | [TD-006, TD-003] | [2 days] | [Security + Performance] |
| [Sprint 6] | [TD-001, TD-002] | [5 days] | [Code + Testing] |
| [Sprint 7] | [TD-004, TD-005] | [3 days] | [Documentation + Infrastructure] |

## 6. Debt Trend

| Period | New Debt | Resolved | Net | Total |
|--------|---------|---------|-----|-------|
| [Sprint 1] | [2] | [0] | [+2] | [2] |
| [Sprint 2] | [1] | [1] | [0] | [2] |
| [Sprint 3] | [2] | [2] | [0] | [2] |
| [Sprint 4] | [1] | [1] | [0] | [2] |
| **Total** | **[6]** | **[4]** | **[+2]** | **[6]** |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Maintenance-Plan]] | Maintenance strategy |
| [[Architecture-Metrics-Report]] | Code quality metrics |
| [[Static-Analysis-Reports]] | Automated debt detection |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Technical debt is *not* bad — sometimes you take shortcuts deliberately. The key is tracking it and paying it back.
