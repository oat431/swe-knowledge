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
| [Request Management] | [25] | [20] | [5] | ✅ |
| [Processing] | [18] | [15] | [3] | ✅ |
| [Authentication] | [12] | [10] | [2] | ✅ |
| [Notifications] | [8] | [6] | [2] | ✅ |
| [Reporting] | [10] | [8] | [2] | ✅ |
| **Total** | **[73]** | **[59]** | **[14]** | |

## 3. Test Case Template

| Field | Value |
|-------|-------|
| **Test Case ID** | [TC-XXX] |
| **Title** | [Descriptive title] |
| **Module** | [Request Management] |
| **Priority** | [🔴 Critical / 🟡 High / 🟢 Medium] |
| **Type** | [Functional / Integration / E2E] |
| **Automated** | [Yes / No] |
| **Requirement** | [[FR-001](../04_Requirements_Engineering/Software-Requirements-Specification.md)] |

### Preconditions

| # | Condition |
|---|----------|
| 1 | [User is logged in as Customer] |
| 2 | [At least one request exists] |

### Test Steps

| Step | Action | Expected Result | Actual Result | Status |
|------|--------|----------------|--------------|--------|
| 1 | [Navigate to My Requests] | [Request list displayed] | | ☐ |
| 2 | [Click on request REQ-001] | [Request detail displayed] | | ☐ |
| 3 | [Verify status shows "Submitted"] | [Status badge shows Submitted] | | ☐ |

### Post-conditions

| # | Condition |
|---|----------|
| 1 | [No data modified] |

## 4. Test Cases — Request Module

### TC-001: Submit Valid Request

| Field | Value |
|-------|-------|
| **ID** | [TC-001] |
| **Title** | [Submit valid standard request] |
| **Priority** | [🔴 Critical] |
| **Requirement** | [FR-001] |

| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | [Login as customer] | [Dashboard displayed] |
| 2 | [Click New Request] | [Form Step 1 displayed] |
| 3 | [Fill personal info] | [Fields populated] |
| 4 | [Click Next] | [Form Step 2 displayed] |
| 5 | [Select type: Standard] | [Type selected] |
| 6 | [Enter amount: 5000] | [Amount entered] |
| 7 | [Enter description] | [Description entered] |
| 8 | [Click Next] | [Form Step 3 displayed] |
| 9 | [Upload document] | [Document uploaded] |
| 10 | [Click Next] | [Review page displayed] |
| 11 | [Click Submit] | [Success page with reference #] |

### TC-002: Submit Request with Invalid Amount

| Field | Value |
|-------|-------|
| **ID** | [TC-002] |
| **Title** | [Reject request with amount = 0] |
| **Priority** | [🔴 Critical] |
| **Requirement** | [FR-001] |

| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | [Login as customer] | [Dashboard displayed] |
| 2 | [Navigate to request form] | [Form displayed] |
| 3 | [Enter amount: 0] | [Amount entered] |
| 4 | [Click Submit] | [Error: "Amount must be greater than 0"] |

### TC-003: Auto-Approve Eligible Request

| Field | Value |
|-------|-------|
| **ID** | [TC-003] |
| **Title** | [Auto-approve standard request ≤ $10K] |
| **Priority** | [🔴 Critical] |
| **Requirement** | [FR-103] |

| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | [Submit standard request, amount $5000] | [Request submitted] |
| 2 | [Wait for processing] | [Status changes to Validating] |
| 3 | [Wait for auto-approval] | [Status changes to Approved] |
| 4 | [Check email notification] | [Approval email received] |

## 5. Test Execution Summary

| Sprint | Executed | Passed | Failed | Blocked | Pass Rate |
|--------|---------|--------|--------|---------|----------|
| [Sprint 1] | [20] | [18] | [2] | [0] | [90%] |
| [Sprint 2] | [25] | [24] | [1] | [0] | [96%] |
| [Sprint 3] | [28] | [27] | [1] | [0] | [96%] |
| **Total** | **[73]** | **[69]** | **[4]** | **[0]** | **[95%]** |

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
