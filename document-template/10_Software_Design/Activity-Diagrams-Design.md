---
document_type: Activity Diagrams (Design)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [activity-diagrams, uml, workflows, swebok, iso-19501]
standard_ref:
  - SWEBOK v4 — Design
  - ISO/IEC 19501 — UML
---

# Activity Diagrams (Design)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document contains activity diagrams showing workflow logic — parallel processing, decision points, and swimlanes at the design level.

## 2. Activity Diagram Index

| # | Activity | Swimlanes | Decision Points | Status |
|---|---------|----------|----------------|--------|
| AD-001 | [Request Processing Pipeline] | [3] | [5] | ✅ Approved |
| AD-002 | [Auto-Approval Logic] | [1] | [4] | ✅ Approved |
| AD-003 | [Notification Dispatch] | [2] | [3] | ✅ Approved |
| AD-004 | [Error Handling Flow] | [2] | [2] | ✅ Approved |

### AD-001: Request Processing Pipeline

```mermaid
flowchart TD
    START([Request Submitted]) --> VALIDATE[Validate Input]
    VALIDATE --> CHECK{Validation<br>Passed?}
    CHECK -->|No| RETURN[Return Errors<br>to Customer]
    CHECK -->|Yes| SAVE[Save to DB]
    SAVE --> FORK{Fork}

    FORK --> CLASSIFY[Classify Request]
    FORK --> AUDIT[Log Audit Event]

    CLASSIFY --> ROUTE[Route to Queue]
    ROUTE --> ELIGIBLE{Auto-Approve<br>Eligible?}
    ELIGIBLE -->|Yes| AUTO[Auto-Approve]
    ELIGIBLE -->|No| QUEUE[Add to Review Queue]

    AUTO --> JOIN{Join}
    AUDIT --> JOIN
    QUEUE --> JOIN

    JOIN --> NOTIFY[Send Notification]
    NOTIFY --> END_NODE([Complete])

    style START fill:#4CAF50,color:#fff
    style END_NODE fill:#4CAF50,color:#fff
    style CHECK fill:#FF9800,color:#fff
    style ELIGIBLE fill:#FF9800,color:#fff
    style RETURN fill:#f44336,color:#fff
```

### AD-002: Auto-Approval Logic

```mermaid
flowchart TD
    START([Request Validated]) --> AMT{Amount ≤<br>$10K?}
    AMT -->|Yes| DUP{Duplicate<br>Check}
    AMT -->|No| VIP{VIP<br>Customer?}

    VIP -->|Yes| VIP_AMT{Amount ≤<br>$25K?}
    VIP -->|No| MANUAL[Manual Review]

    VIP_AMT -->|Yes| DUP
    VIP_AMT -->|No| MANUAL

    DUP -->|No Duplicate| RISK{Risk Score<br>≤ 0.3?}
    DUP -->|Duplicate| FLAG[Flag for Review]

    RISK -->|Yes| APPROVE[Auto-Approve]
    RISK -->|No| MANUAL

    APPROVE --> LOG[Log Decision]
    MANUAL --> LOG
    FLAG --> LOG

    LOG --> END_NODE([Decision Made])

    style START fill:#4CAF50,color:#fff
    style END_NODE fill:#4CAF50,color:#fff
    style APPROVE fill:#4CAF50,color:#fff
    style MANUAL fill:#FF9800,color:#fff
    style FLAG fill:#f44336,color:#fff
```

### AD-003: Notification Dispatch

```mermaid
flowchart TD
    START([Event Received]) --> LOAD[Load Template]
    LOAD --> RENDER[Render Content]
    RENDER --> FORK{Fork}

    FORK --> EMAIL[Send Email]
    FORK --> SMS{SMS<br>Required?}
    FORK --> INAPP{In-App<br>Required?}

    SMS -->|Yes| SEND_SMS[Send SMS]
    SMS -->|No| JOIN{Join}

    INAPP -->|Yes| SEND_INAPP[Create In-App]
    INAPP -->|No| JOIN

    EMAIL --> LOG_EMAIL[Log Email Status]
    SEND_SMS --> LOG_SMS[Log SMS Status]
    SEND_INAPP --> LOG_INAPP[Log In-App Status]

    LOG_EMAIL --> JOIN
    LOG_SMS --> JOIN
    LOG_INAPP --> JOIN

    JOIN --> UPDATE[Update Notification Record]
    UPDATE --> END_NODE([Complete])

    style START fill:#2196F3,color:#fff
    style END_NODE fill:#2196F3,color:#fff
```

### AD-004: Error Handling Flow

```mermaid
flowchart TD
    START([API Request]) --> AUTH[Authenticate]
    AUTH --> AUTH_OK{Auth<br>Valid?}
    AUTH_OK -->|No| UNAUTH[401 Unauthorized]

    AUTH_OK -->|Yes| VALIDATE[Validate Input]
    VALIDATE --> VAL_OK{Validation<br>Passed?}
    VAL_OK -->|No| VAL_ERR[400 Validation Error]

    VAL_OK -->|Yes| PROCESS[Process Request]
    PROCESS --> PROC_OK{Success?}
    PROC_OK -->|Yes| RESPOND[200 OK]

    PROC_OK -->|No| ERR_TYPE{Error<br>Type?}
    ERR_TYPE -->|Not Found| NOT_FOUND[404 Not Found]
    ERR_TYPE -->|Conflict| CONFLICT[409 Conflict]
    ERR_TYPE -->|Internal| RETRY{Retry<br>Count < 3?}

    RETRY -->|Yes| PROCESS
    RETRY -->|No| INTERNAL[500 Internal Error]

    UNAUTH --> LOG[Log Error]
    VAL_ERR --> LOG
    NOT_FOUND --> LOG
    CONFLICT --> LOG
    INTERNAL --> LOG
    RESPOND --> END_NODE([Response Sent])
    LOG --> END_NODE

    style START fill:#f44336,color:#fff
    style END_NODE fill:#f44336,color:#fff
    style RETRY fill:#FF9800,color:#fff
```

## 3. Parallel Processing Rules

| Diagram | Fork Point | Parallel Activities | Join Point | Sync Type |
|---------|-----------|-------------------|-----------|----------|
| [AD-001] | [After save] | [Classify + Audit] | [Before notify] | [Join — wait for all] |
| [AD-003] | [After render] | [Email + SMS + In-App] | [After all logs] | [Join — wait for all] |

## 4. Decision Criteria

| Decision | Criteria | True Path | False Path |
|----------|---------|----------|-----------|
| [Validation Passed] | [All required fields valid, business rules pass] | [Classify] | [Return errors] |
| [Auto-Approve Eligible] | [Amount threshold, no duplicate, low risk] | [Auto-approve] | [Manual review] |
| [SMS Required] | [Customer preference + critical status] | [Send SMS] | [Skip] |
| [Retry Count < 3] | [Transient error, count < max] | [Retry] | [500 error] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Sequence-Diagrams]] | Interactions within activities |
| [[State-Diagrams]] | State changes triggered by activities |
| [[Low-Level-Design]] | Implementation of workflows |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 19501 (UML)
> **Usage:** Activity diagrams show *workflow* — the order of operations, parallel processing, and decision logic. Use them to verify completeness and identify edge cases.
