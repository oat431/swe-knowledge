---
document_type: Resource Requirements
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [resource-requirements, staffing, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Planning (Resource Management)
  - ISO 21502 — Project Management Guidance
---

# Resource Requirements

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document specifies the types and quantities of resources required for each project activity and work package.

## 2. Resource Requirements by WBS

### 2.1 Project Management (1.1)

| WBS | Activity | Role | Quantity | Duration | Skills | Start | End |
|-----|----------|------|----------|----------|--------|-------|-----|
| 1.1.1 | Planning | PM | 1 (50%) | 10 days | PMP, Agile | [Date] | [Date] |
| 1.1.1 | Planning | BA | 1 (100%) | 10 days | BABOK, workshops | [Date] | [Date] |
| 1.1.2 | Monitoring | PM | 1 (50%) | 170 days | EVM, reporting | [Date] | [Date] |
| 1.1.3 | Change Control | PM | 1 (25%) | 170 days | Change mgmt | [Date] | [Date] |
| 1.1.4 | Risk Management | PM | 1 (25%) | 170 days | Risk analysis | [Date] | [Date] |

### 2.2 Requirements & Design (1.2)

| WBS | Activity | Role | Quantity | Duration | Skills | Start | End |
|-----|----------|------|----------|----------|--------|-------|-----|
| 1.2.1 | Elicitation | BA | 1 (100%) | 15 days | Workshops, interviews | [Date] | [Date] |
| 1.2.1 | Elicitation | SMEs | 3-5 (25%) | 15 days | Domain expertise | [Date] | [Date] |
| 1.2.2 | Documentation | BA | 1 (100%) | 15 days | SRS, BRD, RTM | [Date] | [Date] |
| 1.2.3 | Architecture | Architect | 1 (100%) | 7 days | Cloud, security | [Date] | [Date] |
| 1.2.3 | Architecture | TL | 1 (100%) | 7 days | Architecture | [Date] | [Date] |
| 1.2.4 | Detailed Design | TL | 1 (100%) | 10 days | HLD, LLD | [Date] | [Date] |
| 1.2.4 | Detailed Design | Developers | 2 (50%) | 10 days | Design patterns | [Date] | [Date] |

### 2.3 Development (1.3)

| WBS | Activity | Role | Quantity | Duration | Skills | Start | End |
|-----|----------|------|----------|----------|--------|-------|-----|
| 1.3.1 | Customer Portal | Senior Dev | 2 (100%) | 20 days | React, CSS | [Date] | [Date] |
| 1.3.1 | Customer Portal | Junior Dev | 1 (100%) | 20 days | React | [Date] | [Date] |
| 1.3.2 | Admin Portal | Senior Dev | 2 (100%) | 20 days | React | [Date] | [Date] |
| 1.3.3 | Processing Engine | TL | 1 (100%) | 25 days | Node.js, rules | [Date] | [Date] |
| 1.3.3 | Processing Engine | Senior Dev | 1 (100%) | 25 days | Node.js | [Date] | [Date] |
| 1.3.4 | Integration | TL | 1 (100%) | 15 days | REST API | [Date] | [Date] |
| 1.3.4 | Integration | Implementation Consultant | 1 (100%) | 15 days | CRM platform | [Date] | [Date] |
| 1.3.5 | Data Migration | Data Specialist | 1 (100%) | 10 days | ETL, SQL | [Date] | [Date] |
| 1.3.5 | Data Migration | TL | 1 (50%) | 10 days | Data validation | [Date] | [Date] |

### 2.4 Testing (1.4)

| WBS | Activity | Role | Quantity | Duration | Skills | Start | End |
|-----|----------|------|----------|----------|--------|-------|-----|
| 1.4.1 | System Testing | QA Lead | 1 (100%) | 10 days | Test strategy | [Date] | [Date] |
| 1.4.1 | System Testing | QA Engineer | 1 (100%) | 10 days | Manual testing | [Date] | [Date] |
| 1.4.2 | Integration Testing | QA | 2 (100%) | 5 days | API testing | [Date] | [Date] |
| 1.4.2 | Integration Testing | TL | 1 (50%) | 5 days | Integration | [Date] | [Date] |
| 1.4.3 | Performance Testing | QA | 1 (100%) | 5 days | k6, JMeter | [Date] | [Date] |
| 1.4.4 | Security Testing | Security Consultant | 1 (100%) | 5 days | Pen testing | [Date] | [Date] |
| 1.4.5 | UAT | BA | 1 (100%) | 10 days | UAT facilitation | [Date] | [Date] |
| 1.4.5 | UAT | End Users | 3-5 (50%) | 10 days | Domain knowledge | [Date] | [Date] |

### 2.5 Deployment (1.5)

| WBS | Activity | Role | Quantity | Duration | Skills | Start | End |
|-----|----------|------|----------|----------|--------|-------|-----|
| 1.5.1 | Deployment Prep | TL | 1 (100%) | 5 days | DevOps, CI/CD | [Date] | [Date] |
| 1.5.1 | Deployment Prep | DevOps | 1 (100%) | 5 days | Cloud, deployment | [Date] | [Date] |
| 1.5.2 | Go-Live | PM | 1 (100%) | 1 day | Coordination | [Date] | [Date] |
| 1.5.2 | Go-Live | TL | 1 (100%) | 1 day | Technical support | [Date] | [Date] |
| 1.5.3 | Training | BA | 1 (100%) | 5 days | Training delivery | [Date] | [Date] |
| 1.5.4 | Hypercare | Support team | 2 (100%) | 20 days | Troubleshooting | [Date] | [Date] |

## 3. Resource Summary

### 3.1 Headcount by Phase

| Phase | Internal | Vendor | Total | Peak |
|-------|---------|--------|-------|------|
| Initiation | [2] | [0] | [2] | [2] |
| Planning | [3] | [0] | [3] | [3] |
| Execution | [7] | [2] | [9] | [9] |
| Testing | [5] | [1] | [6] | [6] |
| Deployment | [4] | [1] | [5] | [5] |
| Closure | [2] | [0] | [2] | [2] |

### 3.2 Resource Utilization

| Resource | Total Days | Utilization | Status |
|----------|-----------|-------------|--------|
| [PM] | [85 days] | [50%] | ✅ Balanced |
| [BA] | [100 days] | [100%] | ⚠️ Peak during planning |
| [TL] | [120 days] | [100%] | ⚠️ Peak during dev+test |
| [Senior Dev 1] | [55 days] | [100%] | ✅ Sprint-only |
| [Senior Dev 2] | [55 days] | [100%] | ✅ Sprint-only |
| [Junior Dev] | [55 days] | [100%] | ✅ Sprint-only |
| [QA Lead] | [35 days] | [100%] | ✅ Testing-only |
| [QA Engineer] | [35 days] | [100%] | ✅ Testing-only |

## 4. Resource Constraints

| # | Constraint | Impact | Mitigation |
|---|-----------|--------|-----------|
| 1 | [BA at 100% — no buffer] | [No capacity for additional work] | [PM supports BA during peak] |
| 2 | [TL at 100% during dev+test] | [No capacity for support] | [Defer non-critical tasks] |
| 3 | [Vendor resources — 4 week lead time] | [Must order early] | [Procure before Phase 3] |
| 4 | [End users — limited availability for UAT] | [May extend UAT] | [Schedule UAT early, get commitment] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Resource-Management-Plan]] | How resources are managed |
| [[Resource-Breakdown-Structure]] | Resource hierarchy |
| [[WBS-WBS-Dictionary]] | Work packages requiring resources |
| [[Activity-List]] | Activities requiring resources |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** This document specifies *what* resources are needed and *when*. Use it for resource acquisition planning and capacity management. Identify conflicts early — resource constraints are a top cause of schedule delays.
