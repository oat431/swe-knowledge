---
document_type: Communication Diagrams
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [communication-diagrams, uml, collaboration, swebok, iso-19501]
standard_ref:
  - SWEBOK v4 — Design
  - ISO/IEC 19501 — UML
---

# Communication Diagrams

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Communication diagrams (formerly collaboration diagrams) show object interactions emphasizing *relationships* rather than time sequence. Messages are numbered to show order.

## 2. Communication Diagram Index

| # | Scenario | Objects | Messages | Status |
|---|---------|---------|---------|--------|
| CD-001 | [Request Submission] | [6] | [8] | ✅ Approved |
| CD-002 | [Auto-Processing] | [5] | [7] | ✅ Approved |
| CD-003 | [Authentication] | [4] | [6] | ✅ Approved |

## 3. Communication Diagrams

### CD-001: Request Submission

```mermaid
flowchart LR
    CUST([Customer]) -->|"1: submit()"| PORTAL[Portal]
    PORTAL -->|"2: POST /requests"| GW[API Gateway]
    GW -->|"3: authenticate()"| AUTH[Auth Service]
    AUTH -->|"4: token valid"| GW
    GW -->|"5: create request"| REQ[Request Service]
    REQ -->|"6: save()"| DB[(Database)]
    REQ -->|"7: publish event"| QUEUE[Event Queue]
    REQ -->|"8: response"| GW
    GW -->|"9: response"| PORTAL
    PORTAL -->|"10: confirmation"| CUST

    style CUST fill:#4CAF50,color:#fff
    style PORTAL fill:#2196F3,color:#fff
    style GW fill:#FF9800,color:#fff
    style AUTH fill:#f44336,color:#fff
    style REQ fill:#9C27B0,color:#fff
    style DB fill:#607D8B,color:#fff
    style QUEUE fill:#795548,color:#fff
```

### CD-002: Auto-Processing

```mermaid
flowchart LR
    QUEUE([Event Queue]) -->|"1: RequestCreated"| PROC[Processing Service]
    PROC -->|"2: load request"| DB[(Database)]
    DB -->|"3: request data"| PROC
    PROC -->|"4: evaluate()"| RULES[Rules Engine]
    RULES -->|"5: result APPROVE"| PROC
    PROC -->|"6: update status"| DB
    PROC -->|"7: publish event"| QUEUE
    QUEUE -->|"8: RequestApproved"| NOTIFY[Notification Service]

    style QUEUE fill:#795548,color:#fff
    style PROC fill:#2196F3,color:#fff
    style DB fill:#607D8B,color:#fff
    style RULES fill:#FF9800,color:#fff
    style NOTIFY fill:#4CAF50,color:#fff
```

### CD-003: Authentication

```mermaid
flowchart LR
    USER([User]) -->|"1: login"| GW[API Gateway]
    GW -->|"2: authenticate()"| AUTH[Auth Service]
    AUTH -->|"3: find user()"| DB[(Database)]
    DB -->|"4: user record"| AUTH
    AUTH -->|"5: verify password"| AUTH
    AUTH -->|"6: generate jwt"| AUTH
    AUTH -->|"7: store session"| CACHE[(Redis)]
    AUTH -->|"8: tokens"| GW
    GW -->|"9: response"| USER

    style USER fill:#4CAF50,color:#fff
    style GW fill:#FF9800,color:#fff
    style AUTH fill:#2196F3,color:#fff
    style DB fill:#607D8B,color:#fff
    style CACHE fill:#f44336,color:#fff
```

## 4. Message Sequence Summary

| Diagram | Message # | From | To | Message | Type |
|---------|----------|------|-----|---------|------|
| CD-001 | 1 | Customer | Portal | submit() | Synchronous |
| CD-001 | 2 | Portal | Gateway | POST /requests | Synchronous |
| CD-001 | 3 | Gateway | Auth | authenticate() | Synchronous |
| CD-001 | 5 | Gateway | RequestSvc | create_request() | Synchronous |
| CD-001 | 6 | RequestSvc | Database | save() | Synchronous |
| CD-001 | 7 | RequestSvc | Queue | publish(event) | Asynchronous |
| CD-002 | 1 | Queue | ProcessingSvc | RequestCreated | Asynchronous |
| CD-002 | 4 | ProcessingSvc | RulesEngine | evaluate() | Synchronous |
| CD-002 | 7 | ProcessingSvc | Queue | publish(event) | Asynchronous |
| CD-003 | 2 | Gateway | AuthSvc | authenticate() | Synchronous |
| CD-003 | 6 | AuthSvc | AuthSvc | generate_jwt() | Internal |

## 5. Communication Patterns

| Pattern | Description | Applied To |
|---------|-----------|-----------|
| [Synchronous Request-Response] | [Caller waits for response] | [API calls, DB queries] |
| [Asynchronous Fire-and-Forget] | [Caller doesn't wait] | [Event publishing, notifications] |
| [Publish-Subscribe] | [One-to-many messaging] | [Status change events] |
| [Point-to-Point] | [Direct communication] | [Service-to-service calls] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Sequence Diagrams]] | Time-ordered version of same interactions |
| [[Class Diagrams]] | Object structure |
| [[Interface Control Document (ICD)]] | Interface specifications |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 19501 (UML)
> **Usage:** Communication diagrams emphasize *relationships* — which objects know about each other. Use them when object relationships matter more than timing. For timing-focused analysis, use Sequence Diagrams instead.
