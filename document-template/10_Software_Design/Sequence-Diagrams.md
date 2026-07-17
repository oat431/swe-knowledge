---
document_type: Sequence Diagrams
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [sequence-diagrams, uml, interactions, swebok, iso-19501]
standard_ref:
  - SWEBOK v4 — Design
  - ISO/IEC 19501 — UML
---

# Sequence Diagrams

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document contains UML sequence diagrams showing object interactions over time for key scenarios.

## 2. Sequence Diagram Index

| # | Scenario | Participants | Status |
|---|---------|-------------|--------|
| SD-001 | [Request Submission] | [Customer, Portal, API, Request Svc, DB] | ✅ Approved |
| SD-002 | [Auto-Processing] | [Request Svc, Processing Svc, Rules, DB, Notification] | ✅ Approved |
| SD-003 | [Manual Review] | [Admin, API, Processing Svc, DB, Notification] | ✅ Approved |
| SD-004 | [Authentication] | [User, API, Auth Svc, DB, Cache] | ✅ Approved |
| SD-005 | [Report Generation] | [Manager, API, Report Svc, DW] | ✅ Approved |
| SD-006 | [ERP Sync] | [Request Svc, Integration Svc, ERP] | ✅ Approved |

## 3. Sequence Diagrams

### SD-001: Request Submission

```mermaid
sequenceDiagram
    actor C as Customer
    participant P as Portal
    participant GW as API Gateway
    participant RS as Request Service
    participant DB as Database
    participant Q as Event Queue

    C->>P: Fill form & submit
    P->>P: Client-side validation
    P->>GW: POST /api/v1/requests
    GW->>GW: Authenticate (JWT)
    GW->>GW: Rate limit check
    GW->>RS: Forward request
    RS->>RS: Validate DTO
    RS->>RS: Check business rules
    RS->>DB: BEGIN TRANSACTION
    RS->>DB: INSERT request
    RS->>DB: INSERT audit_log
    RS->>DB: COMMIT
    RS->>Q: Publish RequestCreated event
    RS-->>GW: 201 Created
    GW-->>P: 201 Created
    P-->>C: Success + reference number

    Note over RS,Q: Async processing starts
    Q-->>RS: RequestCreated consumed
    RS->>RS: Trigger auto-processing
```

### SD-002: Auto-Processing (Auto-Approval)

```mermaid
sequenceDiagram
    participant Q as Event Queue
    participant PS as Processing Service
    participant RE as Rules Engine
    participant DB as Database
    participant NS as Notification Service
    participant EMAIL as Email Service

    Q->>PS: RequestCreated event
    PS->>PS: Load request from DB
    PS->>RE: Evaluate rules
    RE->>RE: Check amount threshold
    RE->>RE: Check duplicate
    RE->>RE: Check risk score
    RE->>RE: Check VIP status
    RE-->>PS: Result: AUTO_APPROVE

    PS->>DB: BEGIN TRANSACTION
    PS->>DB: UPDATE status → APPROVED
    PS->>DB: INSERT audit_log
    PS->>DB: COMMIT

    PS->>Q: Publish RequestApproved event
    Q->>NS: RequestApproved event
    NS->>NS: Load template
    NS->>NS: Render email
    NS->>EMAIL: Send approval email
    EMAIL-->>NS: Sent
    NS->>DB: Log notification
```

### SD-003: Manual Review

```mermaid
sequenceDiagram
    actor A as Admin
    participant AP as Admin Portal
    participant GW as API Gateway
    participant PS as Processing Service
    participant DB as Database
    participant NS as Notification Service

    A->>AP: Open work queue
    AP->>GW: GET /api/v1/processing/queue
    GW->>PS: Forward
    PS->>DB: Query pending requests
    DB-->>PS: Request list
    PS-->>GW: 200 OK
    GW-->>AP: 200 OK
    AP-->>A: Display queue

    A->>AP: Select request
    AP->>GW: GET /api/v1/requests/:id
    GW->>PS: Forward
    PS->>DB: Query request + history
    DB-->>PS: Request details
    PS-->>GW: 200 OK
    GW-->>AP: 200 OK
    AP-->>A: Display request detail

    A->>AP: Approve request
    AP->>GW: POST /api/v1/processing/:id/approve
    GW->>PS: Forward
    PS->>DB: BEGIN TRANSACTION
    PS->>DB: UPDATE status → APPROVED
    PS->>DB: INSERT audit_log
    PS->>DB: COMMIT
    PS-->>GW: 200 OK
    GW-->>AP: 200 OK
    AP-->>A: Success message

    PS->>NS: Event: RequestApproved
    NS->>NS: Send notification to customer
```

### SD-004: Authentication Flow

```mermaid
sequenceDiagram
    actor U as User
    participant GW as API Gateway
    participant AS as Auth Service
    participant DB as Database
    participant CACHE as Redis Cache

    U->>GW: POST /auth/login (email, password)
    GW->>AS: Forward credentials
    AS->>DB: Query user by email
    DB-->>AS: User record
    AS->>AS: Verify password (bcrypt)
    AS->>AS: Generate JWT (30 min)
    AS->>AS: Generate refresh token (7 days)
    AS->>CACHE: Store refresh token
    AS-->>GW: 200 OK + tokens
    GW-->>U: 200 OK + tokens

    Note over U,GW: Subsequent requests
    U->>GW: GET /api/v1/requests (Bearer JWT)
    GW->>GW: Validate JWT
    GW->>AS: Verify token (cached)
    AS->>CACHE: Check token
    CACHE-->>AS: Valid
    AS-->>GW: Authorized
    GW->>GW: Forward to service
```

### SD-005: Report Generation

```mermaid
sequenceDiagram
    actor M as Manager
    participant DASH as Dashboard
    participant GW as API Gateway
    participant RPT as Reporting Service
    participant DW as Data Warehouse

    M->>DASH: Open dashboard
    DASH->>GW: GET /api/v1/reports/dashboard
    GW->>RPT: Forward
    RPT->>DW: Query aggregated data
    DW-->>RPT: Results
    RPT->>RPT: Format response
    RPT-->>GW: 200 OK + metrics
    GW-->>DASH: 200 OK
    DASH-->>M: Display dashboard

    M->>DASH: Drill down on metric
    DASH->>GW: GET /api/v1/reports/details/:metric
    GW->>RPT: Forward
    RPT->>DW: Query detailed data
    DW-->>RPT: Results
    RPT-->>GW: 200 OK + details
    GW-->>DASH: 200 OK
    DASH-->>M: Display details
```

### SD-006: ERP Synchronization

```mermaid
sequenceDiagram
    participant RS as Request Service
    participant IS as Integration Service
    participant ERP as ERP System
    participant DB as Database
    participant Q as Event Queue

    RS->>Q: Publish RequestApproved event
    Q->>IS: Consume event
    IS->>IS: Map to ERP format
    IS->>ERP: POST /api/customers (create/update)
    ERP-->>IS: 200 OK
    IS->>ERP: POST /api/transactions
    ERP-->>IS: 200 OK
    IS->>DB: Log sync status
    IS->>Q: Publish SyncCompleted event

    Note over IS,ERP: Error handling
    alt ERP unavailable
        IS->>IS: Retry 3x with backoff
        IS->>Q: Move to dead letter queue
        IS->>IS: Alert operations
    end
```

## 4. Interaction Patterns

| Pattern | Applied To | Purpose |
|---------|-----------|---------|
| [Request-Response] | [API calls] | [Synchronous operations] |
| [Publish-Subscribe] | [Event processing] | [Decoupled async operations] |
| [Retry with Backoff] | [External calls] | [Handle transient failures] |
| [Circuit Breaker] | [ERP integration] | [Prevent cascading failures] |
| [Dead Letter Queue] | [Failed events] | [Handle poison messages] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Class-Diagrams]] | Static structure |
| [[State-Diagrams]] | State transitions |
| [[Low-Level-Design]] | Implementation details |
| [[API-Specification]] | API contracts |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 19501 (UML)
> **Usage:** Sequence diagrams show *behavior over time* — how objects interact for specific scenarios. Use them to understand, communicate, and verify complex interactions.
