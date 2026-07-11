---
document_type: Requirements (Verified)
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
tags: [requirements-verification, quality-check, babok, iso-29148]
standard_ref:
  - BABOK v3 — Requirements Analysis & Design Definition
  - ISO/IEC/IEEE 29148 — Requirements Engineering
  - ISO/IEC 20246 — Work Product Reviews
---

# Requirements (Verified)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Verified | Baselined]
> **Last Updated:** [YYYY-MM-DD]
>
> ✅ This document records the verification of requirements against quality characteristics.

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Business Analyst | [Name / Role] |
| QA Lead | [Name / Role] |
| Technical Lead | [Name / Role] |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial verification |
| 1.0 | [YYYY-MM-DD] | [Name] | Verification complete |

---

## 1. Purpose

> This document records the verification of requirements against quality characteristics defined in ISO/IEC/IEEE 29148. Verification ensures requirements are well-formed *before* they are used for design and development.

## 2. Verification Criteria

### 2.1 Quality Characteristics

| Characteristic | Definition | Verification Method |
|---------------|-----------|-------------------|
| **Atomic** | [One requirement per statement] | [Peer review — split compound requirements] |
| **Complete** | [All attributes filled, no gaps] | [Checklist — every field populated] |
| **Consistent** | [No contradictions with other requirements] | [Cross-reference review] |
| **Feasible** | [Technically and economically achievable] | [Technical review, cost analysis] |
| **Unambiguous** | [One interpretation only] | [Peer review, 3 amigos] |
| **Necessary** | [Traces to business objective] | [Traceability check] |
| **Prioritized** | [MoSCoW assigned] | [Stakeholder review] |
| **Testable** | [Acceptance criteria defined] | [AC review, QA sign-off] |
| **Traceable** | [Linked to source and downstream] | [RTM check] |
| **Modifiable** | [Structured for easy updates] | [Document structure review] |

### 2.2 Verification Scoring

| Rating | Score | Description |
|--------|-------|-------------|
| ✅ Pass | [Fully meets criterion] | [No action needed] |
| ⚠️ Partial | [Meets with minor gaps] | [Action item created] |
| ❌ Fail | [Does not meet criterion] | [Must fix before baseline] |

## 3. Verification Results

### 3.1 Requirements Verification Summary

| Req ID | Requirement | Atomic | Complete | Consistent | Feasible | Unambiguous | Testable | Traceable | Overall | Issues |
|--------|-------------|--------|----------|-----------|---------|------------|---------|----------|---------|--------|
| FR-001 | [Online submission] | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ Pass | None |
| FR-002 | [Input validation] | ✅ | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ Partial | [Missing validation rules detail] |
| FR-003 | [Real-time errors] | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ | ✅ | ⚠️ Partial | [Ambiguous "real-time"] |
| FR-101 | [Auto-classification] | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ | ⚠️ Partial | [AC missing edge cases] |
| FR-103 | [Auto-approval] | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ Pass | None |
| NFR-001 | [Response time <2s] | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ Pass | None |
| SEC-001 | [MFA for admin] | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ Pass | None |

### 3.2 Verification Statistics

| Metric | Count | Percentage |
|--------|-------|-----------|
| Total Requirements Verified | [X] | 100% |
| ✅ Pass | [X] | [Y%] |
| ⚠️ Partial | [X] | [Y%] |
| ❌ Fail | [X] | [Y%] |
| **Verification Rate** | | **[Y%]** |

### 3.3 Issues Register

| Issue ID | Req ID | Issue | Severity | Owner | Due Date | Status |
|----------|--------|-------|----------|-------|----------|--------|
| VI-001 | FR-002 | [Missing validation rules detail] | 🟡 Medium | [BA] | [YYYY-MM-DD] | ☐ |
| VI-002 | FR-003 | [Ambiguous "real-time" — define latency] | 🟡 Medium | [BA] | [YYYY-MM-DD] | ☐ |
| VI-003 | FR-101 | [AC missing edge cases for classification] | 🟡 Medium | [QA] | [YYYY-MM-DD] | ☐ |

## 4. Verification Checklist

| # | Check | Status | Notes |
|---|-------|--------|-------|
| 1 | [Every requirement has a unique ID] | ✅❌ | |
| 2 | [Every requirement has acceptance criteria] | ✅❌ | |
| 3 | [Every requirement traces to a business objective] | ✅❌ | |
| 4 | [No conflicting requirements exist] | ✅❌ | |
| 5 | [All requirements are technically feasible] | ✅❌ | |
| 6 | [All requirements are economically feasible] | ✅❌ | |
| 7 | [Priority assigned to every requirement] | ✅❌ | |
| 8 | [No vague terms (fast, easy, user-friendly)] | ✅❌ | |
| 9 | [All requirements reviewed by ≥2 reviewers] | ✅❌ | |
| 10 | [All issues resolved or tracked] | ✅❌ | |

## 5. Verification Sign-Off

| Role | Name | Signature | Date | Verdict |
|------|------|-----------|------|---------|
| BA Lead | | | | ✅ Verified / ⚠️ Conditional / ❌ Not Verified |
| QA Lead | | | | ✅ Verified / ⚠️ Conditional / ❌ Not Verified |
| Technical Lead | | | | ✅ Verified / ⚠️ Conditional / ❌ Not Verified |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SRS]] | Requirements being verified |
| [[Requirements (Validated)]] | Validation confirms stakeholder agreement |
| [[Requirements Traceability Matrix]] | Traceability verified here |
| [[Acceptance Criteria]] | Testability verified here |

---

> **Template Standard:** Based on BABOK v3, ISO/IEC/IEEE 29148, ISO/IEC 20246
> **Usage:** Verify requirements *before* baseline. Verification checks *form* (are they well-written?). Validation checks *substance* (are they the right requirements?). Both must pass before development begins.
