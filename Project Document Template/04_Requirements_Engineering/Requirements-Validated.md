---
document_type: Requirements (Validated)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
ba_owner: "[Business Analyst Name]"
sponsor: "[Sponsor Name]"
classification: "Internal / Confidential"
tags: [requirements-validation, stakeholder-agreement, babok, iso-29148]
standard_ref:
  - BABOK v3 — Requirements Analysis & Design Definition
  - ISO/IEC/IEEE 29148 — Requirements Engineering
---

# Requirements (Validated)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Validated | Baselined]
> **Last Updated:** [YYYY-MM-DD]
>
> ✅ This document records stakeholder validation — confirming these are the *right* requirements.

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Business Analyst | [Name / Role] |
| Sponsor | [Name / Role] |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial validation |
| 1.0 | [YYYY-MM-DD] | [Name] | Validation complete |

---

## 1. Purpose

> This document records stakeholder validation of requirements. While verification checks *form* (are requirements well-written?), validation checks *substance* (do requirements deliver the intended business value?).

## 2. Validation Criteria

| Criterion | Definition | Validation Method |
|-----------|-----------|------------------|
| **Business Value** | [Requirements deliver measurable business benefit] | [Stakeholder review against objectives] |
| **Stakeholder Agreement** | [All key stakeholders agree] | [Sign-off from each stakeholder group] |
| **Completeness** | [All stakeholder needs addressed] | [Needs-to-requirements mapping] |
| **Correctness** | [Requirements accurately reflect needs] | [Walkthrough with stakeholders] |
| **Feasibility** | [Stakeholders agree solution is achievable] | [Feasibility study review] |

## 3. Validation Results

### 3.1 Requirements Validation by Stakeholder Group

| Stakeholder Group | Representative | Requirements Reviewed | Agreed | Disagreed | Status |
|-------------------|---------------|---------------------|--------|-----------|--------|
| [Business Owner] | [Name] | BR-01 to BR-08 | [8] | [0] | ✅ Validated |
| [Operations Manager] | [Name] | FR-001 to FR-107 | [12] | [1] | ⚠️ Conditional |
| [End Users] | [Name, Name] | FR-001 to FR-007 | [7] | [0] | ✅ Validated |
| [Compliance Officer] | [Name] | SEC-001 to SEC-008, BR-07 | [9] | [0] | ✅ Validated |
| [IT Director] | [Name] | NFR-001 to NFR-033 | [10] | [1] | ⚠️ Conditional |
| [Sponsor] | [Name] | All | [All] | [0] | ✅ Validated |

### 3.2 Disagreements & Resolutions

| # | Requirement | Stakeholder | Disagreement | Resolution | Decision |
|---|------------|-------------|-------------|-----------|----------|
| 1 | [FR-106] | [Ops Manager] | [Reassignment feature adds complexity] | [Simplified — manager-only reassignment] | [Approved with modification] |
| 2 | [NFR-030] | [IT Director] | [10x scaling is over-engineering for Phase 1] | [Reduce to 5x for Phase 1, 10x for Phase 2] | [Approved with modification] |

### 3.3 Needs Coverage

| Stakeholder Need | Addressed By | Validated | Gap |
|-----------------|-------------|----------|-----|
| SN-01 [Process within 1 hour] | FR-101, FR-102, FR-103 | ✅ | None |
| SN-02 [Reduce data entry 80%] | FR-002, FR-101 | ✅ | None |
| SN-03 [Online submission] | FR-001 | ✅ | None |
| SN-04 [Real-time status] | FR-006 | ✅ | None |
| SN-05 [Real-time dashboards] | FR-301 | ✅ | None |
| SN-06 [Audit trail] | FR-007 | ✅ | None |
| SN-08 [Auto-validate inputs] | FR-002, FR-003 | ✅ | None |
| **Coverage** | | **100%** | |

## 4. Validation Walkthrough Record

| Session | Date | Participants | Requirements Covered | Issues Raised | Resolution |
|---------|------|-------------|---------------------|--------------|-----------|
| [Session 1] | [YYYY-MM-DD] | [Business Owner, Ops Manager, BA] | BR-01 to BR-08 | [1 — BR-08 detail needed] | [Follow-up scheduled] |
| [Session 2] | [YYYY-MM-DD] | [End Users, BA] | FR-001 to FR-007 | [0] | [All agreed] |
| [Session 3] | [YYYY-MM-DD] | [IT Director, Tech Lead, BA] | NFR-001 to NFR-033 | [1 — NFR-030 scaling] | [Reduced scope] |
| [Session 4] | [YYYY-MM-DD] | [Compliance Officer, BA] | SEC-001 to SEC-008 | [0] | [All agreed] |
| [Session 5] | [YYYY-MM-DD] | [Sponsor, All] | All | [0] | [Approved] |

## 5. Validation Statistics

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Stakeholder groups validated] | [6 of 6] | [100%] | ✅ |
| [Requirements agreed] | [X of Y] | [≥95%] | ✅ |
| [Requirements with modifications] | [X] | [<10%] | ✅ |
| [Open disagreements] | [0] | [0] | ✅ |
| [Needs coverage] | [100%] | [100%] | ✅ |

## 6. Validation Sign-Off

| Role | Name | Signature | Date | Verdict |
|------|------|-----------|------|---------|
| Project Sponsor | | | | ✅ Validated |
| Business Owner | | | | ✅ Validated |
| Operations Manager | | | | ✅ Validated / ⚠️ Conditional |
| IT Director | | | | ✅ Validated / ⚠️ Conditional |
| BA Lead | | | | ✅ Validated |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Software-Requirements-Specification]] | Requirements being validated |
| [[Requirements-Verified]] | Verification of form — companion to this validation |
| [[Stakeholder-Needs-Document]] | Needs confirmed here |
| [[Requirements-Traceability-Matrix]] | Traceability confirms needs coverage |

---

> **Template Standard:** Based on BABOK v3, ISO/IEC/IEEE 29148
> **Usage:** Validate requirements with *real stakeholders* — not just peer review. Walkthrough sessions where stakeholders read and confirm requirements are the gold standard. Validation answers: "Are we building the right thing?"
