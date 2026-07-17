---
document_type: Requirements Traceability Matrix (RTM)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
ba_owner: "[Business Analyst Name]"
qa_lead: "[QA Lead Name]"
classification: "Internal / Confidential"
tags: [rtm, traceability, requirements, swebok, sebok, iso-29148]
standard_ref:
  - SWEBOK v4 — Requirements
  - SEBoK v2 — Concept Definition
  - ISO/IEC/IEEE 29148 — Requirements Engineering
---

# Requirements Traceability Matrix (RTM)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Business Analyst | [Name / Role] |
| QA Lead | [Name / Role] |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial draft |
| 1.0 | [YYYY-MM-DD] | [Name] | Baselined version |

---

## 1. Purpose

> This document provides bidirectional traceability from business objectives through requirements to design, code, and test cases. It ensures every requirement has a purpose (upward trace) and every requirement is verified (downward trace).

## 2. Traceability Model

```mermaid
flowchart LR
    OBJ[Business<br>Objectives] --> BR[Business<br>Requirements]
    BR --> SYRS[System<br>Requirements]
    SYRS --> SRS[Software<br>Requirements]
    SRS --> HLD[High-Level<br>Design]
    HLD --> CODE[Source<br>Code]
    CODE --> TC[Test<br>Cases]
    TC --> EXEC[Test<br>Execution]

    style OBJ fill:#1a237e,color:#fff
    style BR fill:#283593,color:#fff
    style SYRS fill:#3949ab,color:#fff
    style SRS fill:#4CAF50,color:#fff
    style HLD fill:#2196F3,color:#fff
    style CODE fill:#FF9800,color:#fff
    style TC fill:#9C27B0,color:#fff
    style EXEC fill:#f44336,color:#fff
```

## 3. Requirements Traceability Matrix

### 3.1 Forward Traceability (Objective → Test)

| Business Objective | Business Req | System Req | Software Req | Design Element | Test Case | Test Status |
|-------------------|-------------|-----------|-------------|---------------|-----------|------------|
| OBJ-01 Reduce Processing Time | BR-01 | SYRS-001 | FR-001 | HLD-001, API-001 | TC-001, TC-002 | ⬜ Not Run |
| OBJ-01 Reduce Processing Time | BR-02 | SYRS-002 | FR-002 | HLD-002, SVC-001 | TC-003, TC-004 | ⬜ Not Run |
| OBJ-01 Reduce Processing Time | BR-04 | SYRS-003 | FR-101, FR-102, FR-103 | HLD-003, WF-001 | TC-005, TC-006, TC-007 | ⬜ Not Run |
| OBJ-02 Customer Experience | BR-01 | SYRS-001 | FR-001 | HLD-001, PORTAL-001 | TC-001, TC-008 | ⬜ Not Run |
| OBJ-02 Customer Experience | BR-03 | SYRS-005 | FR-006 | HLD-005, PORTAL-002 | TC-009, TC-010 | ⬜ Not Run |
| OBJ-03 Compliance | BR-07 | SYRS-006 | FR-007 | HLD-007, AUDIT-001 | TC-011, TC-012 | ⬜ Not Run |
| OBJ-04 Reduce Cost | BR-06 | SYRS-008 | FR-301 | HLD-008, DASH-001 | TC-013 | ⬜ Not Run |

### 3.2 Backward Traceability (Test → Objective)

| Test Case | Software Req | System Req | Business Req | Business Objective | Test Status |
|-----------|-------------|-----------|-------------|-------------------|------------|
| TC-001 | FR-001 | SYRS-001 | BR-01 | OBJ-01, OBJ-02 | ⬜ Not Run |
| TC-002 | FR-001 | SYRS-001 | BR-01 | OBJ-01, OBJ-02 | ⬜ Not Run |
| TC-003 | FR-002 | SYRS-002 | BR-02 | OBJ-01 | ⬜ Not Run |
| TC-004 | FR-002 | SYRS-002 | BR-02 | OBJ-01 | ⬜ Not Run |
| TC-005 | FR-101 | SYRS-003 | BR-04 | OBJ-01 | ⬜ Not Run |
| TC-009 | FR-006 | SYRS-005 | BR-03 | OBJ-02 | ⬜ Not Run |
| TC-011 | FR-007 | SYRS-006 | BR-07 | OBJ-03 | ⬜ Not Run |

## 4. Coverage Analysis

### 4.1 Requirements Coverage

| Level | Total | Covered | Uncovered | Coverage % | Status |
|-------|-------|---------|-----------|-----------|--------|
| Business Objectives | [X] | [Y] | [Z] | [%] | 🟢🟡🔴 |
| Business Requirements | [X] | [Y] | [Z] | [%] | 🟢🟡🔴 |
| System Requirements | [X] | [Y] | [Z] | [%] | 🟢🟡🔴 |
| Software Requirements | [X] | [Y] | [Z] | [%] | 🟢🟡🔴 |
| Design Elements | [X] | [Y] | [Z] | [%] | 🟢🟡🔴 |
| Test Cases | [X] | [Y] | [Z] | [%] | 🟢🟡🔴 |

### 4.2 Uncovered Requirements

| Req ID | Description | Reason | Action | Owner | Due Date |
|--------|-------------|--------|--------|-------|----------|
| [FR-XXX] | [Description] | [No test case yet] | [Create test case] | [QA] | [Date] |
| | | | | | |

## 5. Traceability Rules

| Rule | Description |
|------|-------------|
| **Every BR must trace to ≥1 OBJ** | [No orphan business requirements] |
| **Every SRS req must trace to ≥1 BR** | [No requirements without business justification] |
| **Every SRS req must trace to ≥1 TC** | [No untested requirements] |
| **Every TC must trace to ≥1 SRS req** | [No orphan test cases] |
| **Bidirectional traceability** | [Must be able to trace forward and backward] |

## 6. Traceability Statistics

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Forward coverage — OBJ → TC] | [%] | [100%] | 🟢🟡🔴 |
| [Backward coverage — TC → OBJ] | [%] | [100%] | 🟢🟡🔴 |
| [Orphan requirements] | [Count] | [0] | 🟢🟡🔴 |
| [Orphan test cases] | [Count] | [0] | 🟢🟡🔴 |
| [Requirements with defects] | [Count] | [Decreasing] | 🟢🟡🔴 |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Business-Objectives]] | Top of traceability chain |
| [[Business-Requirements]] | Business requirements in the chain |
| [[Software-Requirements-Specification]] | Software requirements in the chain |
| [[System-Requirements-Specification]] | System requirements in the chain |
| [[Usability-Test-Plan]] | Test cases in the chain |
| [[Requirements-Change-Log]] | Changes tracked against traceability |

---

> **Template Standard:** Based on SWEBOK v4, SEBoK v2, ISO/IEC/IEEE 29148
> **Usage:** This is a *living document* — update whenever requirements, design, or tests change. Use it to assess impact of changes (what tests are affected?) and to verify completeness (are all requirements tested?).
