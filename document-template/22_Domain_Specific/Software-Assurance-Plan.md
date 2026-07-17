---
document_type: Software Assurance Plan
version: "1.0"
status: Draft
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [software-assurance, safety-critical, sebok, iso-61508]
standard_ref:
  - SEBoK v2 — Software Engineering
  - ISO/IEC 61508 — Functional Safety
---

# Software Assurance Plan

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]
>
> ⚠️ **Domain-Specific:** This document applies to safety-critical projects.

---

## 1. Purpose

> Defines software assurance activities for safety-critical systems — ensuring the software meets safety requirements.

## 2. Safety Integrity Level (SIL)

| SIL | Description | Failure Rate | Testing Rigor |
|-----|-----------|-------------|-------------|
| [SIL 4] | [Catastrophic] | [< 10^-9 /hr] | [Formal methods, 100% coverage] |
| [SIL 3] | [Critical] | [< 10^-7 /hr] | [Comprehensive testing, >95%] |
| [SIL 2] | [Major] | [< 10^-6 /hr] | [Standard testing, >80%] |
| [SIL 1] | [Minor] | [< 10^-5 /hr] | [Basic testing] |

### Project SIL Assignment

| Component | SIL | Rationale | Testing Requirements |
|---------|-----|----------|-------------------|
| [Authentication] | [SIL 2] | [Security critical] | [95% coverage, security testing] |
| [Data Processing] | [SIL 3] | [Financial impact] | [95% coverage, formal verification] |
| [Notifications] | [SIL 1] | [Non-critical] | [Basic testing] |

## 3. Assurance Activities

| Activity | Phase | Method | SIL Requirement | Status |
|---------|-------|--------|----------------|--------|
| [Requirements verification] | [Requirements] | [Formal review] | [SIL 1-4] | ✅ |
| [Design verification] | [Design] | [Formal review, FMEA] | [SIL 2-4] | ✅ |
| [Code review] | [Construction] | [Formal inspection] | [SIL 1-4] | ✅ |
| [Static analysis] | [Construction] | [Tool-based] | [SIL 1-4] | ✅ |
| [Unit testing] | [Construction] | [Automated, 95%+] | [SIL 1-4] | ✅ |
| [Integration testing] | [Testing] | [Automated + manual] | [SIL 1-4] | ✅ |
| [System testing] | [Testing] | [Comprehensive] | [SIL 1-4] | ✅ |
| [Safety testing] | [Testing] | [Fault injection] | [SIL 2-4] | ✅ |
| [Formal verification] | [Testing] | [Mathematical proof] | [SIL 3-4] | ⬜ |

## 4. Safety Analysis

| Analysis | Method | Scope | Status |
|---------|--------|-------|--------|
| [FMEA] | [Failure Mode Effects Analysis] | [All SIL 2+ components] | ✅ |
| [FTA] | [Fault Tree Analysis] | [Top-level failures] | ✅ |
| [Hazard Analysis] | [Hazard identification] | [System-wide] | ✅ |

## 5. Independence of Assurance

| Activity | Performer | Independent Reviewer |
|---------|----------|-------------------|
| [Requirements] | [BA] | [Independent BA] |
| [Design] | [Architect] | [Independent Architect] |
| [Code] | [Developer] | [Independent Developer] |
| [Testing] | [QA] | [Independent QA] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SQAP]] | Quality assurance plan |
| [[Integrity-Level-Assignments]] | SIL assignments |
| [[FMEA-FTA-Reports]] | Safety analysis |

---

> **Template Standard:** Based on SEBoK v2, ISO/IEC 61508
> **Usage:** Safety-critical systems require *independent assurance*. The person who builds it cannot be the only person who verifies it.
