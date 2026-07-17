---
document_type: Acceptance Criteria (ATDD/BDD)
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

### 3.1 Request Management

#### FR-001: Online Submission

| AC ID | Scenario | Given | When | Then | Priority |
|-------|---------|-------|------|------|----------|
| AC-001a | Happy path | [Customer is on the portal, logged in] | [Customer fills all required fields and clicks Submit] | [Request is created, reference number generated, confirmation email sent] | 🔴 |
| AC-001b | Save draft | [Customer has partially filled form] | [Customer clicks Save Draft] | [Form data is saved, customer can resume later] | 🟡 |
| AC-001c | Session timeout | [Customer has been inactive for 30 minutes] | [Customer tries to submit] | [Session expired message shown, data preserved in draft] | 🟡 |
| AC-001d | Duplicate submission | [Customer submitted a request within 30 days] | [Customer submits same type of request] | [Warning shown: "Similar request exists. Continue?" with link to existing] | 🟡 |

#### FR-002: Input Validation

| AC ID | Scenario | Given | When | Then | Priority |
|-------|---------|-------|------|------|----------|
| AC-002a | Valid input | [Customer on submission form] | [Customer enters valid data in all fields] | [No errors shown, Submit button enabled] | 🔴 |
| AC-002b | Missing required field | [Customer on submission form] | [Customer leaves required field blank and tabs out] | [Red error message: "[Field name] is required"] | 🔴 |
| AC-002c | Invalid email format | [Customer on submission form] | [Customer enters "abc" in email field] | [Error: "Please enter a valid email address"] | 🔴 |
| AC-002d | Invalid phone format | [Customer on submission form] | [Customer enters "123" in phone field] | [Error: "Please enter a valid phone number"] | 🟡 |
| AC-002e | Amount exceeds limit | [Customer on submission form] | [Customer enters amount > $999,999] | [Error: "Amount cannot exceed $999,999"] | 🔴 |

#### FR-006: Status Tracking

| AC ID | Scenario | Given | When | Then | Priority |
|-------|---------|-------|------|------|----------|
| AC-006a | View status | [Customer has an active request] | [Customer navigates to My Requests] | [All requests shown with current status, dates, and actions taken] | 🔴 |
| AC-006b | Status change notification | [Customer has an active request] | [Status changes (e.g., Approved)] | [Email notification sent within 5 minutes] | 🔴 |
| AC-006c | Status history | [Customer has a request with multiple status changes] | [Customer clicks on a request] | [Full timeline of all status changes with dates and actors shown] | 🟡 |

### 3.2 Processing & Workflow

#### FR-103: Auto-Approval

| AC ID | Scenario | Given | When | Then | Priority |
|-------|---------|-------|------|------|----------|
| AC-103a | Auto-approve eligible | [Request meets ALL auto-approval criteria] | [Request is submitted] | [Request auto-approved within 1 minute, customer notified] | 🔴 |
| AC-103b | Not auto-eligible | [Request does NOT meet auto-approval criteria] | [Request is submitted] | [Request routed to manual review queue] | 🔴 |
| AC-103c | VIP auto-approve | [VIP customer submits request ≤$25K] | [Request is submitted] | [Request auto-approved regardless of standard threshold] | 🔴 |
| AC-103d | VIP exceeds limit | [VIP customer submits request >$25K] | [Request is submitted] | [Request routed to manager queue (not auto-approved)] | 🔴 |

### 3.3 Non-Functional

#### NFR-001: Page Response Time

| AC ID | Scenario | Given | When | Then | Priority |
|-------|---------|-------|------|------|----------|
| AC-N001a | Normal load | [System under normal load (50 users)] | [User loads any page] | [Page renders in <2 seconds (95th percentile)] | 🔴 |
| AC-N001b | Peak load | [System under peak load (100 users)] | [User loads any page] | [Page renders in <5 seconds (95th percentile)] | 🟡 |

#### SEC-001: Multi-Factor Authentication

| AC ID | Scenario | Given | When | Then | Priority |
|-------|---------|-------|------|------|----------|
| AC-S001a | Admin login | [Admin user on login page] | [User enters correct username/password] | [MFA challenge displayed (TOTP or SMS)] | 🔴 |
| AC-S001b | MFA success | [Admin user entered correct password] | [User enters correct MFA code] | [User logged in, session created] | 🔴 |
| AC-S001c | MFA failure | [Admin user entered correct password] | [User enters incorrect MFA code 3 times] | [Account locked for 15 minutes] | 🔴 |

#### USA-007: Accessibility

| AC ID | Scenario | Given | When | Then | Priority |
|-------|---------|-------|------|------|----------|
| AC-U007a | Keyboard navigation | [User on any page] | [User navigates using Tab key only] | [All interactive elements reachable, visible focus indicator] | 🔴 |
| AC-U007b | Screen reader | [User using screen reader] | [User navigates any page] | [All content readable, ARIA labels present, landmarks defined] | 🔴 |
| AC-U007c | Color contrast | [Any text on any page] | [Measured with accessibility tool] | [Contrast ratio ≥4.5:1 for normal text, ≥3:1 for large text] | 🔴 |

## 4. Acceptance Criteria Summary

| Requirement | Total ACs | 🔴 Must Have | 🟡 Should Have | Status |
|------------|----------|-------------|---------------|--------|
| FR-001 | [4] | [1] | [3] | Draft |
| FR-002 | [5] | [3] | [2] | Draft |
| FR-006 | [3] | [2] | [1] | Draft |
| FR-103 | [4] | [4] | [0] | Draft |
| NFR-001 | [2] | [1] | [1] | Draft |
| SEC-001 | [3] | [3] | [0] | Draft |
| USA-007 | [3] | [3] | [0] | Draft |
| **Total** | **[X]** | **[X]** | **[X]** | |

## 5. Acceptance Criteria Traceability

| AC ID | Requirement | Test Case | Test Status |
|-------|------------|-----------|------------|
| AC-001a | FR-001 | TC-001 | ⬜ Not Run |
| AC-001b | FR-001 | TC-002 | ⬜ Not Run |
| AC-002a | FR-002 | TC-003 | ⬜ Not Run |
| AC-103a | FR-103 | TC-010 | ⬜ Not Run |
| AC-N001a | NFR-001 | PERF-TC-001 | ⬜ Not Run |
| AC-S001a | SEC-001 | SEC-TC-001 | ⬜ Not Run |

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
