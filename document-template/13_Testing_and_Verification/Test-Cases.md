---
document_type: Test Cases
version: "1.0"
status: Active
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [test-cases, test-scenarios, swebok, iso-29119]
standard_ref:
  - SWEBOK v4 — Testing
  - ISO/IEC/IEEE 29119 — Software Testing
---

# Test Cases

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Detailed test cases — preconditions, steps, expected results, and actual results for each test scenario.

## 2. Test Case Index

| Module | Total | Automated | Manual | Status |
|--------|-------|----------|--------|--------|
| [Module Name] | [N] | [N] | [N] | [Status] |
| **Total** | **[N]** | **[N]** | **[N]** | |

## 3. Test Case Template

| Field | Value |
|-------|-------|
| **Test Case ID** | [TC-XXX] |
| **Title** | [Descriptive title] |
| **Module** | [Module Name] |
| **Priority** | [🔴 Critical / 🟡 High / 🟢 Medium] |
| **Type** | [Functional / Integration / E2E] |
| **Automated** | [Yes / No] |
| **Requirement** | [[FR-XXX](Software-Requirements-Specification.md)] |

### Preconditions

| # | Condition |
|---|----------|
| 1 | [Precondition] |
| 2 | [Precondition] |

### Test Steps

| Step | Action | Expected Result | Actual Result | Status |
|------|--------|----------------|--------------|--------|
| 1 | [Action] | [Expected result] | | ☐ |
| 2 | [Action] | [Expected result] | | ☐ |
| 3 | [Action] | [Expected result] | | ☐ |

### Post-conditions

| # | Condition |
|---|----------|
| 1 | [Post-condition] |

## 4. Test Cases — [Module Name]

### TC-[XXX]: [Test Case Title]

| Field | Value |
|-------|-------|
| **ID** | [TC-XXX] |
| **Title** | [Descriptive title] |
| **Priority** | [🔴 Critical / 🟡 High / 🟢 Medium] |
| **Requirement** | [FR-XXX] |

| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | [Action] | [Expected result] |
| 2 | [Action] | [Expected result] |
| 3 | [Action] | [Expected result] |

## 5. Test Execution Summary

| Sprint | Executed | Passed | Failed | Blocked | Pass Rate |
|--------|---------|--------|--------|---------|----------|
| [Sprint N] | [N] | [N] | [N] | [N] | [X]% |
| **Total** | **[N]** | **[N]** | **[N]** | **[N]** | **[X]%** |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Test-Plan]] | Plan governing these cases |
| [[Test-Suite]] | Organized test suites |
| [[Traceability-Matrix-Req-Tests]] | Requirement traceability |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC/IEEE 29119
> **Usage:** Every requirement needs at least one test case. Every test case traces to a requirement. Keep them in sync.
