---
document_type: Acceptance Criteria (ATDD/BDD)
schema_version: 2
canonical_name: Acceptance Criteria
doc_form: heavy
applicability: universal
min_project_tier: 3  # Internal
minimum_form: Lightweight markdown doc (frontmatter + required sections only)
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
tags: [acceptance-criteria, bdd, atdd, given-when-then, swebok, iso-29119]
standard_ref:
  - SWEBOK v4 — Requirements
  - ISO/IEC/IEEE 29119 — Software Testing
  - ISO/IEC/IEEE 29148 — Requirements Engineering
---

# Acceptance Criteria (ATDD/BDD)

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
| 1.0 | [YYYY-MM-DD] | [Name] | Approved version |

---

## 1. Purpose

> This document defines acceptance criteria for each requirement using the **Given/When/Then** (GWT) format. Acceptance criteria are the *testable conditions* that must be met for a requirement to be accepted.

## 2. Acceptance Criteria Standards

### 2.1 Format — Given/When/Then

```gherkin
Given [precondition / initial context]
When [action / trigger]
Then [expected outcome / result]
```

### 2.2 Quality Criteria

| Criterion | Description | Example |
|-----------|-------------|---------|
| **Testable** | [Can be verified by test] | ✅ "Response time <2s" vs ❌ "Fast response" |
| **Unambiguous** | [One interpretation only] | ✅ "Email sent within 5 minutes" vs ❌ "Email sent promptly" |
| **Complete** | [Covers happy path + edge cases] | [Multiple scenarios per requirement] |
| **Independent** | [Each criterion testable standalone] | [No dependencies between criteria] |
| **Negotiable** | [Agreed by BA, Dev, QA, PO] | [Reviewed in 3 amigos session] |

## 3. Acceptance Criteria by Requirement

### 3.1 [Requirement Group]

#### FR-[XXX]: [Requirement Title]

| AC ID | Scenario | Given | When | Then | Priority |
|-------|---------|-------|------|------|----------|
| AC-[XXX]a | Happy path | [Precondition] | [Action] | [Expected outcome] | 🔴 |
| AC-[XXX]b | [Edge case] | [Precondition] | [Action] | [Expected outcome] | 🟡 |

### 3.2 [Requirement Group]

#### NFR-[XXX]: [Requirement Title]

| AC ID | Scenario | Given | When | Then | Priority |
|-------|---------|-------|------|------|----------|
| AC-[XXX]a | [Normal condition] | [System state] | [Stimulus] | [Measured outcome] | 🔴 |

### 3.3 [Requirement Group]

#### SEC-[XXX]: [Requirement Title]

| AC ID | Scenario | Given | When | Then | Priority |
|-------|---------|-------|------|------|----------|
| AC-[XXX]a | [Security scenario] | [System state] | [Stimulus] | [Expected outcome] | 🔴 |

## 4. Acceptance Criteria Summary

| Requirement | Total ACs | 🔴 Must Have | 🟡 Should Have | Status |
|------------|----------|-------------|---------------|--------|
| FR-[XXX] | [N] | [N] | [N] | Draft |
| FR-[XXX] | [N] | [N] | [N] | Draft |
| FR-[XXX] | [N] | [N] | [N] | Draft |
| FR-[XXX] | [N] | [N] | [N] | Draft |
| NFR-[XXX] | [N] | [N] | [N] | Draft |
| SEC-[XXX] | [N] | [N] | [N] | Draft |
| USA-[XXX] | [N] | [N] | [N] | Draft |
| **Total** | **[N]** | **[N]** | **[N]** | |

## 5. Acceptance Criteria Traceability

| AC ID | Requirement | Test Case | Test Status |
|-------|------------|-----------|------------|
| AC-[XXX]a | FR-[XXX] | TC-[XXX] | ⬜ Not Run |
| AC-[XXX]b | FR-[XXX] | TC-[XXX] | ⬜ Not Run |
| AC-[XXX]a | FR-[XXX] | TC-[XXX] | ⬜ Not Run |
| AC-[XXX]a | FR-[XXX] | TC-[XXX] | ⬜ Not Run |
| AC-[XXX]a | NFR-[XXX] | PERF-TC-[XXX] | ⬜ Not Run |
| AC-[XXX]a | SEC-[XXX] | SEC-TC-[XXX] | ⬜ Not Run |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Software-Requirements-Specification]] | Requirements these criteria verify |
| [[User-Stories]] | ACs in Given/When/Then format for user stories |
| [[Requirements-Traceability-Matrix]] | ACs linked in traceability |
| [[Acceptance-Criteria]] | ACs elaborated into test cases |
| [[Acceptance-Criteria]] | ACs used for UAT acceptance |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC/IEEE 29119, ISO/IEC/IEEE 29148
> **Usage:** Every requirement MUST have ≥1 acceptance criterion. Use Given/When/Then format for clarity. Write ACs *before* development (ATDD — Acceptance Test-Driven Development). Review in "3 amigos" sessions (BA + Dev + QA).
