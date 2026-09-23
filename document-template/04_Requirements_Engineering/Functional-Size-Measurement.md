---
document_type: Functional Size Measurement (FSM)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
ba_owner: "[Business Analyst Name]"
classification: "Internal / Confidential"
tags: [fsm, function-points, story-points, estimation, swebok, iso-20926]
standard_ref:
  - SWEBOK v4 — Requirements
  - ISO/IEC 20926 — Function Point Measurement (IFPUG)
  - ISO/IEC 24570 — Functional Size Measurement (NESMA)
---

# Functional Size Measurement (FSM)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Business Analyst | [Name / Role] |
| Project Manager | [Name / Role] |

---

## 1. Purpose

> This document measures the functional size of the system using industry-standard methods. Functional size provides an objective basis for effort estimation, productivity measurement, and project benchmarking.

## 2. Measurement Method

### 2.1 Method Selection

| Method | Standard | When to Use | Selected |
|--------|---------|------------|---------|
| [IFPUG Function Points] | [ISO/IEC 20926] | [Formal contracts, benchmarking] | ⬜ |
| [NESMA Function Points] | [ISO/IEC 24570] | [Early estimation, simplified] | ⬜ |
| [COSMIC] | [ISO/IEC 19761] | [Real-time, embedded systems] | ⬜ |
| [Story Points] | [Agile/Custom] | [Sprint planning, relative sizing] | ✅ |
| [Use Case Points] | [Karner] | [Use case-driven projects] | ⬜ |

### 2.2 Chosen Method: [Story Points / Function Points]

> [Description of why this method was chosen and how it will be applied]

## 3. Functional Size — Story Points

### 3.1 Story Point Estimation

| Epic | User Story | Complexity | Uncertainty | Effort | Story Points |
|------|-----------|-----------|------------|--------|-------------|
| E-[XX] | US-[XXX] Submit Request | Medium | Low | Medium | 5 |
| E-[XX] | US-[XXX] Track Status | Low | Low | Low | 3 |
| E-[XX] | US-[XXX] Upload Documents | Medium | Low | Medium | 3 |
| E-[XX] | US-[XXX] Manage Profile | Low | Low | Low | 2 |
| E-[XX] | US-[XXX] View History | Low | Low | Low | 2 |
| E-[XX] | US-[XXX] Save Draft | Medium | Low | Medium | 3 |
| E-[XX] | US-[XXX] Auto-Validate | High | Medium | High | 8 |
| E-[XX] | US-[XXX] Auto-Route | Medium | Medium | Medium | 5 |
| E-[XX] | US-[XXX] Auto-Approve | High | High | High | 8 |
| E-[XX] | US-[XXX] Manual Review | Medium | Low | Medium | 5 |
| E-[XX] | US-[XXX] Escalation | Medium | Low | Medium | 5 |
| E-[XX] | US-[XXX] Reassignment | Low | Low | Low | 2 |
| E-[XX] | US-[XXX] Email Notifications | Low | Low | Low | 3 |
| E-[XX] | US-[XXX] In-App Notifications | Low | Low | Low | 2 |
| E-[XX] | US-[XXX] Operational Dashboard | High | Medium | High | 8 |
| E-[XX] | US-[XXX] Standard Reports | Medium | Low | Medium | 5 |
| E-[XX] | US-[XXX] Ad-hoc Reports | High | Medium | High | 8 |
| E-[XX] | US-[XXX] User Management | Medium | Low | Medium | 5 |
| E-[XX] | US-[XXX] Role Management | Medium | Low | Medium | 3 |
| **Total** | | | | | **[N]** |

### 3.2 Velocity-Based Estimation

| Metric | Value |
|--------|-------|
| Total Story Points | [N] |
| Team Velocity (per sprint) | [N points] |
| Sprint Duration | [N weeks] |
| Estimated Sprints | [Total Points / Velocity = N sprints] |
| Estimated Duration | [N sprints × N weeks = N weeks] |
| Contingency ([X]%) | [+N weeks] |
| **Total Estimated Duration** | **[N weeks]** |

### 3.3 Epic Breakdown

| Epic | Stories | Story Points | % of Total | Sprints |
|------|---------|-------------|-----------|---------|
| E-[XX] [Epic Name] | [N] | [N] | [%] | [N] |
| E-[XX] [Epic Name] | [N] | [N] | [%] | [N] |
| E-[XX] [Epic Name] | [N] | [N] | [%] | [N] |
| E-[XX] [Epic Name] | [N] | [N] | [%] | [N] |
| E-[XX] [Epic Name] | [N] | [N] | [%] | [N] |
| **Total** | **[N]** | **[N]** | **[%]** | **[N]** |

## 4. Functional Size — Function Points (Optional)

### 4.1 IFPUG Function Point Count

| Function Type | Count | Complexity | Weight | FP |
|--------------|-------|-----------|--------|-----|
| External Inputs (EI) | [X] | [Low/Med/High] | [3/4/6] | [Sum] |
| External Outputs (EO) | [X] | [Low/Med/High] | [4/5/7] | [Sum] |
| External Inquiries (EQ) | [X] | [Low/Med/High] | [3/4/6] | [Sum] |
| Internal Logical Files (ILF) | [X] | [Low/Med/High] | [7/10/15] | [Sum] |
| External Interface Files (EIF) | [X] | [Low/Med/High] | [5/7/10] | [Sum] |
| **Unadjusted FP (UFP)** | | | | **[Sum]** |

### 4.2 Value Adjustment Factor (VAF)

| # | Factor | Rating (0-5) |
|---|--------|-------------|
| 1 | Data Communications | [X] |
| 2 | Distributed Data Processing | [X] |
| 3 | Performance | [X] |
| 4 | Heavily Used Configuration | [X] |
| 5 | Transaction Rate | [X] |
| 6 | Online Data Entry | [X] |
| 7 | End-User Efficiency | [X] |
| 8 | Online Update | [X] |
| 9 | Complex Processing | [X] |
| 10 | Reusability | [X] |
| 11 | Installation Ease | [X] |
| 12 | Operational Ease | [X] |
| 13 | Multiple Sites | [X] |
| 14 | Facilitate Change | [X] |
| **Total VAF** | | **[Sum / 70]** |
| **Adjusted FP** | | **[UFP × VAF]** |

## 5. Effort Estimation

### 5.1 Productivity-Based Estimation

| Method | Size | Productivity Rate | Effort Estimate |
|--------|------|------------------|----------------|
| [Story Points × Velocity] | [N SP] | [N SP/sprint] | [N sprints = N weeks] |
| [FP × Productivity] | [X FP] | [Y FP/person-month] | [Z person-months] |
| [Industry Benchmark] | [X FP] | [Y hours/FP] | [Z hours] |

### 5.2 Effort Breakdown

| Activity | % of Total | Effort (hours) |
|----------|-----------|---------------|
| Requirements | [%] | [X] |
| Design | [%] | [X] |
| Development | [%] | [X] |
| Testing | [%] | [X] |
| Management | [%] | [X] |
| **Total** | **[%]** | **[Sum]** |

## 6. Size Trends & Benchmarks

| Metric | This Project | Industry Average | Status |
|--------|-------------|-----------------|--------|
| [FP per person-month] | [X] | [Y] | 🟢🟡🔴 |
| [Story points per sprint] | [X] | [Y] | 🟢🟡🔴 |
| [Defects per FP] | [X] | [Y] | 🟢🟡🔴 |
| [Cost per FP] | [X] | [Y] | 🟢🟡🔴 |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Software-Requirements-Specification]] | Requirements being sized |
| [[User-Stories]] | Stories being estimated |
| [[Project-Scope-Statement]] | Scope being measured |
| [[WBS-WBS-Dictionary]] | Work breakdown uses these estimates |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 20926 (IFPUG), ISO/IEC 24570 (NESMA)
> **Usage:** Functional size provides an *objective* measure of what the system does, independent of technology or team. Use it for: effort estimation, productivity benchmarking, contract negotiation, and project comparison. Story points are relative; function points are absolute.
