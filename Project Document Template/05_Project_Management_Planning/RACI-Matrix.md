---
document_type: RACI Matrix
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [raci, responsibility-assignment, accountability, pmbok]
standard_ref:
  - PMBOK v8 — Planning (Resource Management)
  - ISO 21502 — Project Management Guidance
---

# RACI Matrix

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This matrix defines Responsible, Accountable, Consulted, and Informed (RACI) assignments for all project activities and deliverables.

## 2. RACI Definitions

| Letter | Role | Definition |
|--------|------|-----------|
| **R** | Responsible | [Does the work — the "doer"] |
| **A** | Accountable | [Ultimately answerable — the "owner" — only one per activity] |
| **C** | Consulted | [Provides input before the work — two-way communication] |
| **I** | Informed | [Kept updated after the work — one-way communication] |

## 3. RACI — Project Activities

### 3.1 Initiation & Planning

| Activity | Sponsor | PM | BA | TL | QA Lead | Dev | Change Mgr |
|----------|---------|-----|-----|-----|---------|-----|-----------|
| [Project Charter] | **A** | R | C | I | I | I | I |
| [Project Management Plan] | **A** | R | C | C | C | I | I |
| [Requirements Elicitation] | I | C | **A** | C | C | I | I |
| [Requirements Documentation] | I | C | **A** | C | C | I | I |
| [Requirements Validation] | C | C | **A** | C | C | I | I |
| [Architecture Design] | I | C | C | **A** | C | R | I |
| [Detailed Design] | I | C | C | **A** | C | R | I |
| [Design Review] | I | I | C | **A** | R | R | I |

### 3.2 Development

| Activity | Sponsor | PM | BA | TL | QA Lead | Dev | Change Mgr |
|----------|---------|-----|-----|-----|---------|-----|-----------|
| [Sprint Planning] | I | **A** | R | R | C | R | I |
| [Feature Development] | I | I | C | **A** | I | R | I |
| [Code Review] | I | I | I | **A** | I | R | I |
| [Unit Testing] | I | I | I | C | I | **A** | I |
| [Integration] | I | I | I | **A** | C | R | I |
| [Data Migration] | I | C | C | **A** | C | R | I |

### 3.3 Testing

| Activity | Sponsor | PM | BA | TL | QA Lead | Dev | Change Mgr |
|----------|---------|-----|-----|-----|---------|-----|-----------|
| [Test Planning] | I | C | C | C | **A** | I | I |
| [System Testing] | I | I | C | C | **A** | C | I |
| [Integration Testing] | I | I | C | C | **A** | C | I |
| [Performance Testing] | I | I | I | C | **A** | C | I |
| [Security Testing] | I | I | I | C | **A** | C | I |
| [UAT] | C | C | **A** | I | C | I | I |
| [UAT Sign-off] | **A** | C | R | I | C | I | I |
| [Defect Management] | I | I | I | C | **A** | R | I |

### 3.4 Deployment & Closure

| Activity | Sponsor | PM | BA | TL | QA Lead | Dev | Change Mgr |
|----------|---------|-----|-----|-----|---------|-----|-----------|
| [Deployment Planning] | I | **A** | C | R | C | C | I |
| [Go-Live] | **A** | R | C | R | C | C | I |
| [Training] | I | C | **A** | I | I | I | R |
| [Hypercare Support] | I | **A** | C | R | C | R | I |
| [Change Management] | I | C | C | I | I | I | **A** |
| [Lessons Learned] | I | **A** | R | R | R | R | R |
| [Project Closure] | **A** | R | C | C | C | I | I |

### 3.5 Governance & Communication

| Activity | Sponsor | PM | BA | TL | QA Lead | Dev | Change Mgr |
|----------|---------|-----|-----|-----|---------|-----|-----------|
| [Steering Committee] | **A** | R | C | C | I | I | I |
| [Status Reporting] | I | **A** | C | C | C | I | I |
| [Risk Management] | C | **A** | C | C | C | I | I |
| [Change Control] | C | **A** | C | C | C | I | I |
| [Stakeholder Communication] | I | **A** | R | I | I | I | R |
| [Quality Assurance] | I | C | C | C | **A** | C | I |
| [Budget Management] | **A** | R | I | I | I | I | I |

## 4. RACI — Deliverables

| Deliverable | Sponsor | PM | BA | TL | QA Lead | Dev | Change Mgr |
|------------|---------|-----|-----|-----|---------|-----|-----------|
| [Project Charter] | **A** | R | C | I | I | I | I |
| [SRS] | I | C | **A** | C | C | I | I |
| [SAD / Architecture] | I | C | C | **A** | C | R | I |
| [HLD / LLD] | I | C | C | **A** | C | R | I |
| [Source Code] | I | I | I | **A** | I | R | I |
| [Test Plan] | I | C | C | C | **A** | I | I |
| [Test Report] | I | I | C | C | **A** | I | I |
| [UAT Sign-off] | **A** | C | R | I | C | I | I |
| [Training Materials] | I | C | **A** | I | I | I | R |
| [Operations Runbook] | I | C | C | **A** | C | R | I |
| [Deployment Plan] | I | **A** | C | R | C | C | I |
| [Release Notes] | I | **A** | C | R | C | C | I |
| [Lessons Learned] | I | **A** | R | R | R | R | R |
| [Project Closure Report] | **A** | R | C | C | C | I | I |

## 5. RACI Validation

| # | Check | Status |
|---|-------|--------|
| 1 | [Every activity has exactly one A] | ✅❌ |
| 2 | [Every activity has at least one R] | ✅❌ |
| 3 | [No person is both R and A for the same activity] | ✅❌ |
| 4 | [No person is R/A for everything] | ✅❌ |
| 5 | [Consulted stakeholders are identified] | ✅❌ |
| 6 | [Informed stakeholders are identified] | ✅❌ |
| 7 | [RACI reviewed with all stakeholders] | ✅❌ |

## 6. RACI Summary by Role

| Role | Responsible | Accountable | Consulted | Informed | Total |
|------|-----------|------------|----------|---------|-------|
| [Sponsor] | [1] | [8] | [4] | [10] | [23] |
| [PM] | [10] | [14] | [12] | [3] | [39] |
| [BA] | [6] | [5] | [14] | [6] | [31] |
| [TL] | [8] | [8] | [12] | [5] | [33] |
| [QA Lead] | [2] | [8] | [10] | [6] | [26] |
| [Dev] | [10] | [1] | [8] | [12] | [31] |
| [Change Mgr] | [2] | [1] | [2] | [16] | [21] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Project Management Plan]] | Parent plan |
| [[Resource Management Plan]] | Resource assignments |
| [[Team Charter]] | Team working agreements |
| [[Governance Approach]] | Decision authority |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** The RACI eliminates confusion about "who does what." Every activity must have exactly one Accountable person. If you can't assign RACI, the activity may need clarification.
