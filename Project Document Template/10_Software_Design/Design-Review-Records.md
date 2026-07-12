---
document_type: Design Review Records
version: "1.0"
status: Active
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [design-review, review-records, swebok, iso-20246]
standard_ref:
  - SWEBOK v4 — Design
  - ISO/IEC 20246 — Work Product Reviews
---

# Design Review Records

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document records all formal design reviews — findings, action items, dispositions, and sign-offs.

## 2. Review Types

| Type | When | Participants | Purpose |
|------|------|-------------|---------|
| [Architecture Review] | [Before design] | [Architect, TL, QA] | [Validate architecture decisions] |
| [HLD Review] | [Before LLD] | [TL, Dev team, QA] | [Validate module decomposition] |
| [LLD Review] | [Before coding] | [Dev team, QA] | [Validate detailed design] |
| [Code Review] | [During coding] | [Developer peers] | [Validate implementation] |
| [Design Inspection] | [Critical artifacts] | [Trained inspectors] | [Defect detection] |

## 3. Review Register

| Review ID | Date | Type | Artifact | Reviewers | Findings | Actions | Disposition | Sign-Off |
|-----------|------|------|---------|-----------|---------|---------|------------|---------|
| DR-001 | [YYYY-MM-DD] | [Architecture Review] | [SAD v1.0] | [Architect, TL, QA Lead] | [3] | [2] | ✅ Approved with conditions | [Date] |
| DR-002 | [YYYY-MM-DD] | [HLD Review] | [HLD v1.0] | [TL, Dev Team, QA] | [5] | [4] | ✅ Approved with conditions | [Date] |
| DR-003 | [YYYY-MM-DD] | [LLD Review — Request Service] | [LLD v1.0] | [Dev Team, QA] | [4] | [3] | ✅ Approved with conditions | [Date] |
| DR-004 | [YYYY-MM-DD] | [LLD Review — Processing Service] | [LLD v1.0] | [Dev Team, QA] | [3] | [2] | ✅ Approved | [Date] |
| DR-005 | [YYYY-MM-DD] | [Database Design Review] | [ERD + DDL v1.0] | [TL, DBA, QA] | [2] | [2] | ✅ Approved with conditions | [Date] |

## 4. Review Template

### DR-XXX: [Review Title]

| Field | Detail |
|-------|--------|
| **Review ID** | [DR-XXX] |
| **Date** | [YYYY-MM-DD] |
| **Type** | [Architecture / HLD / LLD / Code / Inspection] |
| **Artifact** | [Document name + version] |
| **Author** | [Name] |
| **Facilitator** | [Name] |
| **Reviewers** | [Names] |
| **Duration** | [X hours] |

#### Entry Criteria

| # | Criterion | Status |
|---|----------|--------|
| 1 | [Artifact complete and distributed 24h in advance] | ✅❌ |
| 2 | [Review checklist prepared] | ✅❌ |
| 3 | [Reviewers have reviewed artifact] | ✅❌ |

#### Findings

| # | Finding | Severity | Location | Owner | Action | Due Date | Status |
|---|---------|---------|---------|-------|--------|----------|--------|
| 1 | [Finding description] | [Critical / Major / Minor] | [Section X.Y] | [Name] | [Action required] | [Date] | ⬜ Open |
| 2 | [Finding description] | [Critical / Major / Minor] | [Section X.Y] | [Name] | [Action required] | [Date] | ✅ Closed |

#### Disposition

| Option | Decision |
|--------|---------|
| ✅ **Approved** | [No conditions — proceed] |
| ⚠️ **Approved with Conditions** | [Proceed after addressing critical/major findings] |
| ❌ **Rejected** | [Major rework required — re-review needed] |

#### Sign-Off

| Role | Name | Signature | Date |
|------|------|-----------|------|
| [Author] | | | |
| [Reviewer 1] | | | |
| [Reviewer 2] | | | |
| [Facilitator] | | | |

## 5. Review Statistics

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Total reviews conducted] | [5] | — | — |
| [Findings per review (avg)] | [3.4] | [<5] | 🟢 |
| [Critical findings] | [2] | [0] | 🟡 |
| [Actions completed on time] | [90%] | [100%] | 🟡 |
| [Reviews with rework] | [0] | [0] | 🟢 |

## 6. Review Checklist Template

| # | Check | Status |
|---|-------|--------|
| 1 | [All requirements addressed in design] | ✅❌ |
| 2 | [No single points of failure] | ✅❌ |
| 3 | [Security controls adequate] | ✅❌ |
| 4 | [Performance targets achievable] | ✅❌ |
| 5 | [Error handling comprehensive] | ✅❌ |
| 6 | [Interfaces well-defined] | ✅❌ |
| 7 | [Data model normalized] | ✅❌ |
| 8 | [Design patterns appropriate] | ✅❌ |
| 9 | [Technical debt identified] | ✅❌ |
| 10 | [Testability considered] | ✅❌ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[High-Level Design (HLD)]] | HLD being reviewed |
| [[Low-Level Design (LLD)]] | LLD being reviewed |
| [[Software Architecture Document]] | Architecture being reviewed |
| [[Requirements (Verified)]] | Requirements verified through design |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 20246
> **Usage:** Record every formal design review. Track findings to closure. Reviews catch defects early — when they're cheap to fix.
