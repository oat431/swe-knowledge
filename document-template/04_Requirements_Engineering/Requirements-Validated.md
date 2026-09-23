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
| [Business Owner] | [Name] | BR-[XX] to BR-[XX] | [N] | [N] | [✅ Validated / ⚠️ Conditional] |
| [Operations Manager] | [Name] | FR-[XXX] to FR-[XXX] | [N] | [N] | [✅ Validated / ⚠️ Conditional] |
| [End Users] | [Name, Name] | FR-[XXX] to FR-[XXX] | [N] | [N] | [✅ Validated / ⚠️ Conditional] |
| [Compliance Officer] | [Name] | SEC-[XXX] to SEC-[XXX], BR-[XX] | [N] | [N] | [✅ Validated / ⚠️ Conditional] |
| [IT Director] | [Name] | NFR-[XXX] to NFR-[XXX] | [N] | [N] | [✅ Validated / ⚠️ Conditional] |
| [Sponsor] | [Name] | All | [N] | [N] | [✅ Validated / ⚠️ Conditional] |

### 3.2 Disagreements & Resolutions

| # | Requirement | Stakeholder | Disagreement | Resolution | Decision |
|---|------------|-------------|-------------|-----------|----------|
| 1 | [FR-XXX] | [Stakeholder] | [Disagreement description] | [Resolution] | [Approved with modification] |
| 2 | [NFR-[XXX]] | [IT Director] | [10x scaling is over-engineering for Phase 1] | [Reduce to 5x for Phase 1, 10x for Phase 2] | [Approved with modification] |

### 3.3 Needs Coverage

| Stakeholder Need | Addressed By | Validated | Gap |
|-----------------|-------------|----------|-----|
| SN-[XX] [Stakeholder need] | FR-[XXX], FR-[XXX] | [✅/❌] | [None / Gap description] |
| SN-[XX] [Stakeholder need] | FR-[XXX], FR-[XXX] | [✅/❌] | [None / Gap description] |
| SN-[XX] [Stakeholder need] | FR-[XXX] | [✅/❌] | [None / Gap description] |
| SN-[XX] [Stakeholder need] | FR-[XXX] | [✅/❌] | [None / Gap description] |
| SN-[XX] [Stakeholder need] | FR-[XXX] | [✅/❌] | [None / Gap description] |
| SN-[XX] [Stakeholder need] | FR-[XXX] | [✅/❌] | [None / Gap description] |
| SN-[XX] [Stakeholder need] | FR-[XXX], FR-[XXX] | [✅/❌] | [None / Gap description] |
| **Coverage** | | **[%]** | |

## 4. Validation Walkthrough Record

| Session | Date | Participants | Requirements Covered | Issues Raised | Resolution |
|---------|------|-------------|---------------------|--------------|-----------|
| [Session N] | [YYYY-MM-DD] | [Participants] | BR-[XX] to BR-[XX] | [N — issue description] | [Resolution] |
| [Session N] | [YYYY-MM-DD] | [Participants] | FR-[XXX] to FR-[XXX] | [N] | [Resolution] |
| [Session N] | [YYYY-MM-DD] | [Participants] | NFR-[XXX] to NFR-[XXX] | [N — issue description] | [Resolution] |
| [Session N] | [YYYY-MM-DD] | [Participants] | SEC-[XXX] to SEC-[XXX] | [N] | [Resolution] |
| [Session N] | [YYYY-MM-DD] | [All] | All | [N] | [Resolution] |

## 5. Validation Statistics

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Stakeholder groups validated] | [X of Y] | [100%] | ✅ |
| [Requirements agreed] | [X of Y] | [≥95%] | ✅ |
| [Requirements with modifications] | [X] | [<10%] | ✅ |
| [Open disagreements] | [0] | [0] | ✅ |
| [Needs coverage] | [%] | [100%] | ✅ |

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
