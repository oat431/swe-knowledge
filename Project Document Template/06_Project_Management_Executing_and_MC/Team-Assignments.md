---
document_type: Team Assignments
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [team-assignments, resource-allocation, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Executing (Resource Management)
  - ISO 21502 — Project Management Guidance
---

# Team Assignments

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document tracks which team members are assigned to which roles, activities, and work packages. It provides visibility into resource allocation and supports workload management.

## 2. Team Roster

| ID | Name | Role | Email | Start Date | End Date | Allocation | Status |
|----|------|------|-------|-----------|---------|-----------|--------|
| TM-01 | [Name] | [Project Manager] | [email] | [YYYY-MM-DD] | [YYYY-MM-DD] | 50% | ✅ Active |
| TM-02 | [Name] | [Business Analyst] | [email] | [YYYY-MM-DD] | [YYYY-MM-DD] | 100% | ✅ Active |
| TM-03 | [Name] | [Technical Lead] | [email] | [YYYY-MM-DD] | [YYYY-MM-DD] | 100% | ✅ Active |
| TM-04 | [Name] | [Senior Developer 1] | [email] | [YYYY-MM-DD] | [YYYY-MM-DD] | 100% | ✅ Active |
| TM-05 | [Name] | [Senior Developer 2] | [email] | [YYYY-MM-DD] | [YYYY-MM-DD] | 100% | ✅ Active |
| TM-06 | [Name] | [Junior Developer] | [email] | [YYYY-MM-DD] | [YYYY-MM-DD] | 100% | ✅ Active |
| TM-07 | [Name] | [QA Lead] | [email] | [YYYY-MM-DD] | [YYYY-MM-DD] | 100% | ✅ Active |
| TM-08 | [Name] | [QA Engineer] | [email] | [YYYY-MM-DD] | [YYYY-MM-DD] | 100% | ✅ Active |
| TM-09 | [Name] | [Solution Architect] | [email] | [YYYY-MM-DD] | [YYYY-MM-DD] | 25% | ✅ Active |
| TM-10 | [Name] | [Change Manager] | [email] | [YYYY-MM-DD] | [YYYY-MM-DD] | 25% | ✅ Active |
| TM-11 | [Name] | [DevOps Engineer] | [email] | [YYYY-MM-DD] | [YYYY-MM-DD] | 50% | ✅ Active |

## 3. Assignment Matrix — Current Sprint

| Activity | TM-01 PM | TM-02 BA | TM-03 TL | TM-04 Dev1 | TM-05 Dev2 | TM-06 Dev3 | TM-07 QA1 | TM-08 QA2 |
|----------|---------|---------|---------|----------|----------|----------|---------|---------|
| [Sprint Planning] | A | R | R | C | C | C | C | I |
| [Feature A — Portal] | I | C | C | R | R | I | I | I |
| [Feature B — Processing] | I | C | A | C | C | R | I | I |
| [Feature C — Admin] | I | C | C | R | I | I | I | I |
| [Code Review] | I | I | A | R | R | R | I | I |
| [Bug Fixes] | I | I | C | R | R | R | C | I |
| [Sprint Review] | A | R | C | C | C | C | C | I |
| [Sprint Retro] | A | R | R | R | R | R | R | R |

## 4. Workload Summary

### 4.1 Current Sprint

| Team Member | Assigned Tasks | Story Points | Utilization | Status |
|------------|---------------|-------------|------------|--------|
| [PM] | [5] | [N/A] | [50%] | 🟢 Balanced |
| [BA] | [4] | [3] | [90%] | 🟢 Balanced |
| [TL] | [6] | [8] | [100%] | 🟡 Full |
| [Dev 1] | [4] | [8] | [100%] | 🟡 Full |
| [Dev 2] | [3] | [5] | [80%] | 🟢 Balanced |
| [Dev 3] | [3] | [5] | [80%] | 🟢 Balanced |
| [QA Lead] | [3] | [N/A] | [60%] | 🟢 Balanced |
| [QA Engineer] | [3] | [N/A] | [60%] | 🟢 Balanced |

### 4.2 Overallocation Alerts

| Team Member | Allocated | Capacity | Overallocation | Action |
|------------|----------|---------|---------------|--------|
| [TL] | [100%] | [100%] | [At capacity — no buffer] | [Defer non-critical tasks] |
| [Dev 1] | [100%] | [100%] | [At capacity — no buffer] | [Monitor] |

## 5. Assignment History

| Date | Change | Reason | Impact |
|------|--------|--------|--------|
| [YYYY-MM-DD] | [Dev 3 added to project] | [Capacity need for Sprint 3] | [Team fully staffed] |
| [YYYY-MM-DD] | [Architect reduced to 25%] | [Architecture phase complete] | [Cost savings] |
| [YYYY-MM-DD] | [QA team starts] | [Testing phase begins] | [Testing capacity available] |

## 6. Backup Assignments

| Primary | Backup | Skills Coverage | When to Activate |
|---------|--------|----------------|-----------------|
| [PM] | [BA] | [Project management basics] | [PM unavailable >2 days] |
| [BA] | [PM] | [Requirements, stakeholder mgmt] | [BA unavailable >2 days] |
| [TL] | [Senior Dev 1] | [Architecture, technical decisions] | [TL unavailable >3 days] |
| [QA Lead] | [QA Engineer] | [Test strategy, defect management] | [QA Lead unavailable >2 days] |

## 7. Release Schedule

| Team Member | Planned Release Date | Reason | Knowledge Transfer Required |
|------------|---------------------|--------|---------------------------|
| [Solution Architect] | [YYYY-MM-DD] | [Architecture complete] | [Design docs, ADRs] |
| [Implementation Consultant] | [YYYY-MM-DD] | [Configuration complete] | [Config docs, runbook] |
| [Data Migration Specialist] | [YYYY-MM-DD] | [Migration complete] | [Scripts, validation] |
| [Developers] | [YYYY-MM-DD] | [Development complete] | [Code docs, handover] |
| [QA team] | [YYYY-MM-DD] | [Testing complete] | [Test results, defect log] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Resource Management Plan]] | Resource management strategy |
| [[Resource Requirements]] | Resource needs per WBS |
| [[RACI Matrix]] | Responsibility assignments |
| [[Team Charter]] | Team working agreements |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** Update this document at every sprint boundary. Track utilization to prevent burnout and identify capacity for additional work. Overallocation is an early warning for schedule risk.
