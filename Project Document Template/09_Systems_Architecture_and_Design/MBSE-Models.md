---
document_type: MBSE Models (SysML)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
architect: "[Solution Architect]"
classification: "Internal / Confidential"
tags: [mbse, sysml, model-based, sebok, iso-24641]
standard_ref:
  - SEBoK v2 — System Architecture
  - ISO/IEC/IEEE 24641 — Model-Based Systems Engineering
---

# MBSE Models (SysML)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document captures Model-Based Systems Engineering (MBSE) artifacts using SysML notation. MBSE uses models (not just documents) as the primary means of capturing system requirements, design, and analysis.

## 2. MBSE Approach

| Aspect | Approach |
|--------|---------|
| [Modeling Language] | [SysML (OMG Systems Modeling Language)] |
| [Modeling Tool] | [Draw.io / PlantUML / Cameo] |
| [Model Scope] | [Requirements, structure, behavior, parametrics] |
| [Model Purpose] | [Communication, analysis, verification] |

## 3. SysML Diagram Types Applied

| Diagram Type | Purpose | Applied | Status |
|-------------|---------|---------|--------|
| [Requirements Diagram] | [Requirements structure and relationships] | ✅ | Complete |
| [Block Definition Diagram (BDD)] | [System structure — blocks, components] | ✅ | Complete |
| [Internal Block Diagram (IBD)] | [Internal structure — ports, flows] | ✅ | Complete |
| [Activity Diagram] | [Behavioral — process flows] | ✅ | Complete |
| [Sequence Diagram] | [Behavioral — interactions] | ✅ | Complete |
| [State Machine Diagram] | [Behavioral — state transitions] | ✅ | Complete |
| [Use Case Diagram] | [Functional — actor-goal modeling] | ✅ | Complete |
| [Parametric Diagram] | [Analysis — constraints and equations] | ⬜ | Not Started |

## 4. Requirements Diagram

```mermaid
flowchart TD
    BIZ[Business<br>Requirements] --> SYS[System<br>Requirements]
    SYS --> SW[Software<br>Requirements]
    SW --> TEST[Test<br>Cases]

    BIZ --> BR01[BR-01: Online<br>Submission]
    BIZ --> BR02[BR-02: Auto<br>Validation]
    SYS --> SYRS01[SYRS-001: Request<br>Management]
    SYS --> SYRS02[SYRS-002: Processing<br>Engine]
    SW --> FR01[FR-001: Submit<br>Request]
    SW --> FR02[FR-101: Auto<br>Classify]

    BR01 -.->|satisfies| SYRS01
    BR02 -.->|satisfies| SYRS02
    SYRS01 -.->|refines| FR01
    SYRS02 -.->|refines| FR02

    style BIZ fill:#4CAF50,color:#fff
    style SYS fill:#2196F3,color:#fff
    style SW fill:#FF9800,color:#fff
    style TEST fill:#9C27B0,color:#fff
```

## 5. Block Definition Diagram (BDD)

```mermaid
flowchart TD
    subgraph System["System"]
        PORTAL[Customer Portal]
        ADMIN[Admin Portal]
        GATEWAY[API Gateway]
        SERVICES[Microservices]
        DATA[Data Layer]
        EXT[External Systems]
    end

    subgraph Services_Detail["Microservices"]
        REQ[Request Service]
        PROC[Processing Service]
        AUTH[Auth Service]
        NOTIFY[Notification Service]
    end

    SERVICES --> REQ
    SERVICES --> PROC
    SERVICES --> AUTH
    SERVICES --> NOTIFY

    style System fill:#1a237e,color:#fff
    style Services_Detail fill:#2196F3,color:#fff
```

## 6. Sequence Diagram: Request Processing

```mermaid
sequenceDiagram
    actor Customer
    participant Portal
    participant Gateway
    participant RequestSvc
    participant ProcessingSvc
    participant DB
    participant NotificationSvc

    Customer->>Portal: Submit Request
    Portal->>Gateway: POST /requests
    Gateway->>Gateway: Authenticate
    Gateway->>RequestSvc: Create Request
    RequestSvc->>DB: Store
    RequestSvc-->>Gateway: 201 Created
    Gateway-->>Portal: 201 Created
    Portal-->>Customer: Success

    RequestSvc->>ProcessingSvc: Event: RequestCreated
    ProcessingSvc->>ProcessingSvc: Validate
    ProcessingSvc->>ProcessingSvc: Classify & Route
    ProcessingSvc->>DB: Update Status
    ProcessingSvc->>NotificationSvc: Event: StatusChanged
    NotificationSvc->>Customer: Email Notification
```

## 7. State Machine Diagram: Request Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Draft: Customer starts
    Draft --> Submitted: Submit
    Submitted --> Validating: Auto-validate
    Validating --> Valid: Pass
    Validating --> Invalid: Fail
    Invalid --> Draft: Return to customer
    Valid --> Classifying: Auto-classify
    Classifying --> Routed: Route to queue
    Routed --> UnderReview: Staff picks up
    UnderReview --> Approved: Approve
    UnderReview --> Rejected: Reject
    UnderReview --> Escalated: Escalate
    Escalated --> Approved: Manager approves
    Escalated --> Rejected: Manager rejects
    Approved --> [*]
    Rejected --> [*]
```

## 8. Activity Diagram: Auto-Approval Logic

```mermaid
flowchart TD
    START([Request Validated]) --> AMT{Amount ≤<br>Threshold?}
    AMT -->|Yes| FIELDS{All Fields<br>Valid?}
    AMT -->|No| VIP{VIP<br>Customer?}
    FIELDS -->|Yes| DUP{No Duplicate?}
    FIELDS -->|No| MANUAL[Manual Review]
    VIP -->|Yes| VIP_AMT{Amount ≤<br>$25K?}
    VIP -->|No| MANUAL
    VIP_AMT -->|Yes| APPROVE[Auto-Approve]
    VIP_AMT -->|No| MANUAL
    DUP -->|Yes| RISK{Risk Score<br>OK?}
    DUP -->|No| FLAG[Flag Duplicate]
    RISK -->|Yes| APPROVE
    RISK -->|No| MANUAL

    style START fill:#4CAF50,color:#fff
    style APPROVE fill:#4CAF50,color:#fff
    style MANUAL fill:#FF9800,color:#fff
    style FLAG fill:#f44336,color:#fff
```

## 9. Model Traceability

| Model Element | Requirement | Design | Test |
|--------------|-----------|--------|------|
| [Request block] | [FR-001] | [Request Service] | [TC-001] |
| [Processing block] | [FR-101 to FR-107] | [Processing Service] | [TC-005] |
| [Auto-approval activity] | [FR-103] | [Rules Engine] | [TC-010] |
| [Request state machine] | [FR-006] | [Status tracking] | [TC-009] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[System Architecture Description]] | Architecture modeled here |
| [[SRS]] | Requirements modeled here |
| [[Functional Architecture]] | Functions modeled here |
| [[Architecture Views (4+1)]] | Views using MBSE models |

---

> **Template Standard:** Based on SEBoK v2, ISO/IEC/IEEE 24641
> **Usage:** MBSE models are *executable* and *analyzable* — unlike documents. Use them for complex systems where visual models reduce ambiguity. Keep models in sync with requirements and code.
