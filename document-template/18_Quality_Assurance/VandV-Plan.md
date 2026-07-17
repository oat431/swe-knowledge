---
document_type: V&V Plan
version: "1.0"
status: Draft
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [verification, validation, v-and-v, swebok, iso-15288]
standard_ref:
  - SWEBOK v4 — Quality Assurance
  - ISO/IEC/IEEE 15288 — System Life Cycle
---

# V&V Plan (Verification & Validation Plan)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines verification (building right) and validation (building right product) activities.

## 2. V&V Overview

| Aspect | Verification | Validation |
|--------|-------------|-----------|
| **Question** | [Are we building the product right?] | [Are we building the right product?] |
| **Focus** | [Specifications] | [User needs] |
| **Methods** | [Reviews, inspections, testing] | [User testing, acceptance] |
| **When** | [During development] | [After development] |
| **Output** | [Conformance evidence] | [Acceptance evidence] |

## 3. Verification Activities

| Activity | Phase | Method | Criteria | Status |
|---------|-------|--------|---------|--------|
| [Requirements Review] | [Requirements] | [Peer review] | [Complete, consistent, traceable] | ✅ |
| [Design Review] | [Design] | [Formal review] | [Meets requirements] | ✅ |
| [Code Review] | [Construction] | [PR review] | [[Coding-Standards]] compliance] | ✅ |
| [Unit Testing] | [Construction] | [Automated] | [≥ 80% coverage] | ✅ |
| [Integration Testing] | [Testing] | [Automated] | [All interfaces verified] | ✅ |
| [System Testing] | [Testing] | [Automated + Manual] | [All requirements verified] | ✅ |

## 4. Validation Activities

| Activity | Phase | Method | Criteria | Status |
|---------|-------|--------|---------|--------|
| [Prototype Review] | [Design] | [Stakeholder review] | [Design direction approved] | ✅ |
| [UAT] | [Testing] | [User testing] | [Business scenarios pass] | ✅ |
| [Usability Testing] | [Testing] | [User testing] | [SUS ≥ 68] | ✅ |
| [Operational Testing] | [Pre-deployment] | [Operations review] | [Runbook verified] | ✅ |

## 5. V&V Matrix

| Requirement | V: Design | V: Code | V: Test | V: UAT | Status |
|-------------|----------|--------|--------|--------|--------|
| [FR-001] | ✅ | ✅ | ✅ | ✅ | ✅ |
| [FR-002] | ✅ | ✅ | ✅ | ✅ | ✅ |
| [FR-101] | ✅ | ✅ | ✅ | ✅ | ✅ |
| [NFR-001] | ✅ | ✅ | ✅ | — | ✅ |
| [NFR-002] | ✅ | ✅ | ✅ | — | ✅ |

## 6. V&V Schedule

```mermaid
gantt
    title V&V Schedule
    dateFormat YYYY-MM-DD
    section Verification
    Requirements Review  :a1, 2026-08-15, 3d
    Design Review        :a2, 2026-09-15, 3d
    Code Reviews         :a3, 2026-10-01, 60d
    Unit Testing         :b1, 2026-10-01, 60d
    Integration Testing  :b2, 2026-11-15, 15d
    System Testing       :b3, 2026-12-01, 20d
    section Validation
    Prototype Review     :c1, 2026-09-01, 3d
    UAT                  :c2, 2026-12-20, 10d
    Usability Testing    :c3, 2026-12-20, 5d
    Operational Testing  :c4, 2027-01-03, 3d
```

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Verification-Reports]] | Verification results |
| [[Validation-Reports]] | Validation results |
| [[SQAP]] | Quality assurance plan |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC/IEEE 15288
> **Usage:** V&V is *not* just testing. Verification checks specs; validation checks needs. You need both.
