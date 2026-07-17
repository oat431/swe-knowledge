---
document_type: State Diagrams
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [state-diagrams, statecharts, uml, behavior, swebok, iso-19501]
standard_ref:
  - SWEBOK v4 — Design
  - ISO/IEC 19501 — UML
---

# State Diagrams

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document contains UML state diagrams showing the lifecycle of key entities — states, transitions, events, and guards.

## 2. State Diagram Index

| # | Entity | States | Transitions | Status |
|---|--------|--------|-----------|--------|
| ST-001 | [Request] | [8] | [10] | ✅ Approved |
| ST-002 | [User Session] | [4] | [5] | ✅ Approved |
| ST-003 | [Notification] | [4] | [5] | ✅ Approved |
| ST-004 | [Document] | [3] | [3] | ✅ Approved |

## 3. State Diagrams

### ST-001: Request Lifecycle

```mermaid
stateDiagram-v2
    [*] --> DRAFT: Customer creates

    DRAFT --> SUBMITTED: submit()
    DRAFT --> [*]: abandon (timeout 7d)

    SUBMITTED --> VALIDATING: auto-validate
    VALIDATING --> CLASSIFYING: valid
    VALIDATING --> DRAFT: invalid (return to customer)

    CLASSIFYING --> ROUTED: classified
    ROUTED --> UNDER_REVIEW: staff picks up
    ROUTED --> AUTO_APPROVED: auto-approve eligible

    UNDER_REVIEW --> APPROVED: approve()
    UNDER_REVIEW --> REJECTED: reject()
    UNDER_REVIEW --> ESCALATED: escalate()

    ESCALATED --> APPROVED: manager approve()
    ESCALATED --> REJECTED: manager reject()

    AUTO_APPROVED --> [*]
    APPROVED --> [*]
    REJECTED --> [*]

    state VALIDATING {
        [*] --> CHECK_FIELDS
        CHECK_FIELDS --> CHECK_RULES
        CHECK_RULES --> CHECK_DUPLICATE
        CHECK_DUPLICATE --> [*]
    }

    state UNDER_REVIEW {
        [*] --> REVIEWING
        REVIEWING --> DECIDING
        DECIDING --> [*]
    }
```

**State Descriptions:**

| State | Description | Entry Action | Exit Action |
|-------|-----------|-------------|------------|
| [DRAFT] | [Customer is filling the form] | [Create request record] | [Set submitted_at] |
| [SUBMITTED] | [Request submitted, awaiting processing] | [Notify customer] | — |
| [VALIDATING] | [System validating inputs] | [Run validation rules] | [Log validation result] |
| [CLASSIFYING] | [System classifying request type] | [Run classification rules] | [Set classification] |
| [ROUTED] | [Assigned to queue/staff] | [Assign to queue] | — |
| [UNDER_REVIEW] | [Staff reviewing request] | [Notify assigned staff] | — |
| [ESCALATED] | [Escalated to manager] | [Notify manager] | — |
| [AUTO_APPROVED] | [Auto-approved by system] | [Set completed_at, notify] | — |
| [APPROVED] | [Manually approved] | [Set completed_at, notify] | — |
| [REJECTED] | [Rejected] | [Set completed_at, notify with reason] | — |

**Transition Table:**

| From | To | Event | Guard | Action |
|------|-----|-------|-------|--------|
| [DRAFT] | [SUBMITTED] | [submit()] | [All required fields filled] | [Set submitted_at] |
| [SUBMITTED] | [VALIDATING] | [auto-trigger] | — | [Run validation] |
| [VALIDATING] | [CLASSIFYING] | [validation.passed] | [All rules pass] | [Log success] |
| [VALIDATING] | [DRAFT] | [validation.failed] | [Any rule fails] | [Return errors to customer] |
| [CLASSIFYING] | [ROUTED] | [classification.complete] | — | [Assign queue] |
| [ROUTED] | [UNDER_REVIEW] | [staff.open()] | [Staff available] | [Assign to staff] |
| [ROUTED] | [AUTO_APPROVED] | [auto-trigger] | [Eligible for auto-approve] | [Approve, notify] |
| [UNDER_REVIEW] | [APPROVED] | [approve()] | [Staff has permission] | [Set completed_at] |
| [UNDER_REVIEW] | [REJECTED] | [reject(reason)] | [Staff has permission] | [Set completed_at] |
| [UNDER_REVIEW] | [ESCALATED] | [escalate(reason)] | [Staff has permission] | [Notify manager] |
| [ESCALATED] | [APPROVED] | [manager.approve()] | [Manager has permission] | [Set completed_at] |
| [ESCALATED] | [REJECTED] | [manager.reject(reason)] | [Manager has permission] | [Set completed_at] |

### ST-002: User Session

```mermaid
stateDiagram-v2
    [*] --> UNAUTHENTICATED

    UNAUTHENTICATED --> AUTHENTICATED: login(email, password)
    UNAUTHENTICATED --> LOCKED: 5 failed attempts

    AUTHENTICATED --> EXPIRED: token expires (30 min)
    AUTHENTICATED --> UNAUTHENTICATED: logout()

    EXPIRED --> AUTHENTICATED: refresh_token()
    EXPIRED --> UNAUTHENTICATED: refresh expired

    LOCKED --> UNAUTHENTICATED: unlock (15 min timeout)
    LOCKED --> UNAUTHENTICATED: admin unlock()
```

### ST-003: Notification

```mermaid
stateDiagram-v2
    [*] --> QUEUED: event triggered

    QUEUED --> SENDING: processor picks up
    SENDING --> SENT: delivery confirmed
    SENDING --> FAILED: delivery failed

    FAILED --> QUEUED: retry (max 3)
    FAILED --> DEAD_LETTER: max retries exceeded

    SENT --> [*]
    DEAD_LETTER --> [*]
```

### ST-004: Document

```mermaid
stateDiagram-v2
    [*] --> UPLOADING: customer selects file

    UPLOADING --> VALIDATED: upload complete + validation pass
    UPLOADING --> REJECTED: validation fail (size, type)

    VALIDATED --> STORED: saved to storage
    REJECTED --> [*]

    STORED --> [*]
```

## 4. State Invariants

| Entity | State | Invariant | Violation Action |
|--------|-------|----------|-----------------|
| [Request] | [APPROVED] | [completed_at must be set] | [System error] |
| [Request] | [REJECTED] | [rejection_reason must not be empty] | [Prevent transition] |
| [Request] | [SUBMITTED] | [amount > 0] | [Prevent submission] |
| [User] | [AUTHENTICATED] | [token must be valid] | [Force re-auth] |
| [Notification] | [SENT] | [sent_at must be set] | [System error] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Sequence-Diagrams]] | Interactions between entities |
| [[Class-Diagrams]] | Entity structure |
| [[Low-Level-Design]] | Implementation of state logic |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 19501 (UML)
> **Usage:** State diagrams show *lifecycle* — how an entity changes over time. Use them to verify all states are handled and transitions are valid. They're essential for testing — every state and transition needs a test case.
