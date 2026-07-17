---
document_type: Activity Diagrams (Requirements)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
ba_owner: "[Business Analyst Name]"
classification: "Internal / Confidential"
tags: [activity-diagrams, process-flow, uml, swebok, iso-19501]
standard_ref:
  - SWEBOK v4 — Requirements
  - ISO/IEC 19501 — UML
---

# Activity Diagrams (Requirements)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Business Analyst | [Name / Role] |

---

## 1. Purpose

> This document contains UML-style activity diagrams that model business processes and workflows at the requirements level. These diagrams complement textual requirements with visual process flows.

## 2. Diagram Index

| # | Diagram | Process Modeled | Related Requirements | Status |
|---|---------|----------------|---------------------|--------|
| AD-01 | [Request Submission Flow] | [Customer submitting a request] | FR-001, FR-002, FR-003 | Draft |
| AD-02 | [Request Processing Flow] | [Operations processing a request] | FR-101 to FR-107 | Draft |
| AD-03 | [Auto-Approval Flow] | [Automated approval logic] | FR-103 | Draft |
| AD-04 | [Notification Flow] | [When and to whom notifications are sent] | FR-201 to FR-205 | Draft |
| AD-05 | [Exception Handling Flow] | [Error and exception scenarios] | FR-002, FR-104 | Draft |

## 3. Activity Diagrams

### AD-01: Request Submission Flow

```mermaid
flowchart TD
    START([Customer Opens Portal]) --> LOGIN{Authenticated?}
    LOGIN -->|No| AUTH[Login/Register]
    AUTH --> LOGIN
    LOGIN -->|Yes| FORM[Display Request Form]
    FORM --> FILL[Customer Fills Form]
    FILL --> VALIDATE{Real-time<br>Validation}
    VALIDATE -->|Errors| SHOW_ERR[Show Field Errors]
    SHOW_ERR --> FILL
    VALIDATE -->|Valid| UPLOAD{Upload<br>Documents?}
    UPLOAD -->|Yes| ATTACH[Attach Files]
    ATTACH --> SUBMIT
    UPLOAD -->|No| SUBMIT{Click Submit?}
    SUBMIT -->|Save Draft| SAVE[Save Draft]
    SAVE --> FORM
    SUBMIT -->|Submit| FINAL[Final Validation]
    FINAL -->|Fail| SHOW_ERR
    FINAL -->|Pass| CREATE[Create Request]
    CREATE --> REF[Generate Reference Number]
    REF --> EMAIL[Send Confirmation Email]
    EMAIL --> SUCCESS[Display Success]
    SUCCESS --> END([Complete])

    style START fill:#4CAF50,color:#fff
    style END fill:#4CAF50,color:#fff
    style LOGIN fill:#FF9800,color:#fff
    style VALIDATE fill:#2196F3,color:#fff
    style FINAL fill:#2196F3,color:#fff
    style SHOW_ERR fill:#f44336,color:#fff
```

### AD-02: Request Processing Flow

```mermaid
flowchart TD
    START([Request Received]) --> CLASSIFY[Auto-Classify<br>Request Type]
    CLASSIFY --> ROUTE[Route to<br>Appropriate Queue]
    ROUTE --> ASSIGN[Assign to<br>Available Staff]
    ASSIGN --> REVIEW[Staff Reviews<br>Request]
    REVIEW --> RULES{Business Rules<br>Check}
    RULES -->|Pass| AMOUNT{Amount<br>> $10K?}
    RULES -->|Fail| REJECT[Reject with<br>Reason]
    AMOUNT -->|No| AUTO{Auto-Approve<br>Eligible?}
    AMOUNT -->|Yes| MGR[Route to<br>Manager]
    AUTO -->|Yes| APPROVE[Auto-Approve]
    AUTO -->|No| MANUAL[Manual Review]
    MANUAL --> DECIDE{Staff<br>Decision}
    DECIDE -->|Approve| APPROVE
    DECIDE -->|Reject| REJECT
    DECIDE -->|Escalate| MGR
    MGR --> MGR_DECIDE{Manager<br>Decision}
    MGR_DECIDE -->|Approve| APPROVE
    MGR_DECIDE -->|Reject| REJECT
    APPROVE --> NOTIFY_A[Notify Customer<br>Approved]
    REJECT --> NOTIFY_R[Notify Customer<br>Rejected]
    NOTIFY_A --> END([Complete])
    NOTIFY_R --> END

    style START fill:#4CAF50,color:#fff
    style END fill:#4CAF50,color:#fff
    style APPROVE fill:#4CAF50,color:#fff
    style REJECT fill:#f44336,color:#fff
    style MGR fill:#FF9800,color:#fff
    style AUTO fill:#2196F3,color:#fff
```

### AD-03: Auto-Approval Flow

```mermaid
flowchart TD
    START([Validated Request]) --> CHECK_1{Amount<br><= $10K?}
    CHECK_1 -->|No| CHECK_VIP{VIP<br>Customer?}
    CHECK_1 -->|Yes| CHECK_2{All Required<br>Fields Valid?}
    CHECK_VIP -->|No| MANUAL[Route to<br>Manual Review]
    CHECK_VIP -->|Yes| CHECK_VIP_AMT{Amount<br><= $25K?}
    CHECK_VIP_AMT -->|Yes| APPROVE[Auto-Approve]
    CHECK_VIP_AMT -->|No| MANUAL
    CHECK_2 -->|No| MANUAL
    CHECK_2 -->|Yes| CHECK_3{No Duplicate<br>in 30 Days?}
    CHECK_3 -->|No| FLAG[Flag for<br>Review]
    CHECK_3 -->|Yes| CHECK_4{No Risk<br>Indicators?}
    CHECK_4 -->|No| MANUAL
    CHECK_4 -->|Yes| APPROVE
    APPROVE --> LOG[Log Approval]
    LOG --> NOTIFY[Notify Customer]
    NOTIFY --> END([Approved])
    MANUAL --> END_M([Queued])
    FLAG --> END_F([Flagged])

    style START fill:#2196F3,color:#fff
    style APPROVE fill:#4CAF50,color:#fff
    style MANUAL fill:#FF9800,color:#fff
    style FLAG fill:#FF9800,color:#fff
```

### AD-04: Notification Flow

```mermaid
flowchart TD
    START([Status Change<br>Event]) --> IDENTIFY[Identify<br>Stakeholders]
    IDENTIFY --> TEMPLATE[Select Notification<br>Template]
    TEMPLATE --> EMAIL{Email<br>Enabled?}
    EMAIL -->|Yes| SEND_EMAIL[Send Email<br>Notification]
    EMAIL -->|No| SMS{SMS<br>Enabled?}
    SEND_EMAIL --> SMS
    SMS -->|Yes| SEND_SMS[Send SMS<br>Notification]
    SMS -->|No| INAPP{In-App<br>Enabled?}
    SEND_SMS --> INAPP
    INAPP -->|Yes| SEND_INAPP[Send In-App<br>Notification]
    INAPP -->|No| LOG
    SEND_INAPP --> LOG[Log Notification<br>Sent]
    LOG --> END([Complete])

    style START fill:#9C27B0,color:#fff
    style END fill:#4CAF50,color:#fff
```

### AD-05: Exception Handling Flow

```mermaid
flowchart TD
    START([Exception<br>Occurs]) --> TYPE{Exception<br>Type?}
    TYPE -->|Validation| VAL_ERR[Return Field-Level<br>Errors]
    TYPE -->|Timeout| TIMEOUT[Retry 3x<br>Then Queue]
    TYPE -->|System Error| SYS_ERR[Log Error<br>Alert IT]
    TYPE -->|Business Rule| BIZ_ERR[Flag for<br>Manual Review]
    VAL_ERR --> CUSTOMER[Display to<br>Customer]
    TIMEOUT --> QUEUE[Add to<br>Retry Queue]
    SYS_ERR --> ALERT[Send Alert<br>to IT Team]
    BIZ_ERR --> OPS[Route to<br>Operations]
    CUSTOMER --> END_C([Customer<br>Corrects])
    QUEUE --> END_Q([Queued for<br>Retry])
    ALERT --> END_IT([IT<br>Investigates])
    OPS --> END_O([Manual<br>Review])

    style START fill:#f44336,color:#fff
    style CUSTOMER fill:#FF9800,color:#fff
    style QUEUE fill:#FF9800,color:#fff
    style ALERT fill:#f44336,color:#fff
    style OPS fill:#2196F3,color:#fff
```

## 4. Diagram Legend

| Symbol | Meaning |
|--------|---------|
| Rounded rectangle | Activity / Action |
| Diamond | Decision / Branch |
| Circle (small) | Fork / Join |
| Circle (large, filled) | Start |
| Circle (large, ring) | End |
| Arrow | Flow / Transition |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Software-Requirements-Specification]] | Requirements modeled here |
| [[Use-Case-Specifications]] | Use case flows visualized here |
| [[User-Stories]] | Story flows visualized here |
| [[Activity-Diagrams]] | Design-level process models |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 19501 (UML)
> **Usage:** Activity diagrams visualize *process flows* — they complement textual requirements. Use them for complex workflows where text alone is unclear. Mermaid format renders in GitHub, Obsidian, and most markdown viewers.
