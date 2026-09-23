---
document_type: User Stories
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
ba_owner: "[Business Analyst Name]"
po_owner: "[Product Owner]"
classification: "Internal / Confidential"
tags: [user-stories, agile, backlog, swebok]
standard_ref:
  - SWEBOK v4 — Requirements
  - ISO/IEC/IEEE 29148 — Requirements Engineering
---

# User Stories

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | In Backlog]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Business Analyst | [Name / Role] |
| Product Owner | [Name / Role] |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial draft |
| 1.0 | [YYYY-MM-DD] | [Name] | Approved version |

---

## 1. Purpose

> This document captures requirements as user stories in the format: **"As a [role], I want [feature], so that [benefit]."** Each story includes acceptance criteria and estimation.

## 2. User Story Standards

### 2.1 INVEST Criteria

| Criterion | Description | Check |
|-----------|-------------|-------|
| **I**ndependent | [Story can be developed independently] | ✅ |
| **N**egotiable | [Details can be discussed and refined] | ✅ |
| **V**aluable | [Delivers value to a user or business] | ✅ |
| **E**stimable | [Team can estimate effort] | ✅ |
| **S**mall | [Fits within a sprint] | ✅ |
| **T**estable | [Has clear acceptance criteria] | ✅ |

### 2.2 Story Format

```markdown
**As a** [role/persona]
**I want** [feature/capability]
**So that** [benefit/value]

**Acceptance Criteria:**
Given [precondition]
When [action]
Then [outcome]

**Story Points:** [1, 2, 3, 5, 8, 13]
**Priority:** [🔴 Must Have / 🟡 Should Have / 🟢 Could Have]
**Epic:** [Parent epic]
**Sprint:** [Target sprint]
```

## 3. Epic Overview

| Epic ID | Epic Name | Stories | Total Points | Sprint |
|---------|-----------|---------|-------------|--------|
| E-[XX] | [Epic Name] | [N] | [N] | [Sprint N-N] |
| E-[XX] | [Epic Name] | [N] | [N] | [Sprint N-N] |
| E-[XX] | [Epic Name] | [N] | [N] | [Sprint N-N] |
| E-[XX] | [Epic Name] | [N] | [N] | [Sprint N-N] |
| E-[XX] | [Epic Name] | [N] | [N] | [Sprint N-N] |

## 4. User Stories

### Epic E-[XX]: [Epic Name]

#### US-[XXX]: [Story Title]

**As a** [role/persona]
**I want** [feature/capability]
**So that** [benefit/value]

**Acceptance Criteria:**
- **AC-[X]:** Given [precondition], When [action], Then [outcome]

**Story Points:** [N]
**Priority:** [🔴 Must Have / 🟡 Should Have / 🟢 Could Have]
**Epic:** E-[XX]
**Sprint:** [N]
**Status:** Draft

---

## 5. Story Estimation Summary

| Epic | Stories | Total Points | Sprint Allocation |
|------|---------|-------------|------------------|
| E-[XX] [Epic Name] | [N] | [N] | [Sprint N-N] |
| E-[XX] [Epic Name] | [N] | [N] | [Sprint N-N] |
| E-[XX] [Epic Name] | [N] | [N] | [Sprint N-N] |
| E-[XX] [Epic Name] | [N] | [N] | [Sprint N-N] |
| E-[XX] [Epic Name] | [N] | [N] | [Sprint N-N] |
| **Total** | **[N]** | **[N]** | |

## 6. Story Map

| | Sprint 1 | Sprint 2 | Sprint 3 | Sprint 4 | Sprint 5 |
|--|---------|---------|---------|---------|---------|
| **E-[XX] [Epic Name]** | US-[XXX], US-[XXX] | US-[XXX] | US-[XXX] | US-[XXX] | US-[XXX] |
| **E-[XX] [Epic Name]** | | US-[XXX], US-[XXX] | US-[XXX], US-[XXX] | US-[XXX], US-[XXX] | |
| **E-[XX] [Epic Name]** | | | US-[XXX], US-[XXX] | US-[XXX], US-[XXX] | |
| **E-[XX] [Epic Name]** | | | | US-[XXX], US-[XXX] | US-[XXX], US-[XXX], US-[XXX] |
| **E-[XX] [Epic Name]** | | US-[XXX], US-[XXX] | US-[XXX], US-[XXX] | | |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Software-Requirements-Specification]] | User stories elaborate SRS requirements |
| [[Acceptance-Criteria]] | ACs defined per user story |
| [[Requirements-Traceability-Matrix]] | Stories traced to objectives |
| [[Requirements-Architecture]] | Stories organized by epic |
| [[User-Stories]] | Stories managed in backlog |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC/IEEE 29148
> **Usage:** User stories are the *Agile format* for requirements. Each story must have acceptance criteria, story points, and priority. Stories are refined in backlog grooming sessions and delivered in sprints.
