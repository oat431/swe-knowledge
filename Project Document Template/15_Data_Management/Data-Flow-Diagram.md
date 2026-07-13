---
document_type: Data Flow Diagram
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-flow, dfd, dmbok]
standard_ref:
  - DMBOK v2 — Data Architecture
---

# Data Flow Diagram

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Visualizes how data moves through the system — from sources to destinations, through transformations.

## 2. Context Diagram (Level 0)

```mermaid
flowchart LR
    CUSTOMER([Customer]) -->|Submit Request| SYSTEM[Project<br>System]
    SYSTEM -->|Status Updates| CUSTOMER
    SYSTEM -->|Sync Data| ERP[ERP]
    ERP -->|Customer Data| SYSTEM
    SYSTEM -->|Process Payment| PAY[Payment]
    PAY -->|Payment Result| SYSTEM
    SYSTEM -->|Notifications| EMAIL[Email]
    SYSTEM -->|Reports| MGMT[Management]

    style SYSTEM fill:#2196F3,color:#fff
    style CUSTOMER fill:#4CAF50,color:#fff
    style ERP fill:#FF9800,color:#fff
    style PAY fill:#9C27B0,color:#fff
```

## 3. Level 1 Data Flow

```mermaid
flowchart TD
    subgraph External["External Entities"]
        CUST([Customer])
        ERP[ERP]
        PAY[Payment]
        EMAIL[Email]
    end

    subgraph System["System Processes"]
        P1[1.0<br>Receive Request]
        P2[2.0<br>Validate & Classify]
        P3[3.0<br>Process & Approve]
        P4[4.0<br>Execute & Complete]
        P5[5.0<br>Notify & Report]
    end

    subgraph Data["Data Stores"]
        D1[(Customer<br>Data)]
        D2[(Request<br>Data)]
        D3[(Transaction<br>Data)]
        D4[(Audit<br>Log)]
    end

    CUST -->|Request| P1
    P1 -->|Validated Request| P2
    P2 -->|Classified Request| P3
    P3 -->|Approved Request| P4
    P4 -->|Completed Request| P5

    P1 --> D1
    P1 --> D2
    P2 --> D2
    P3 --> D3
    P4 --> D3
    P5 --> D4

    D1 --> P2
    D2 --> P3

    ERP -->|Customer Data| D1
    P4 -->|Payment| PAY
    P5 -->|Notification| EMAIL

    style External fill:#e3f2fd,color:#000
    style System fill:#e8f5e9,color:#000
    style Data fill:#fff3e0,color:#000
```

## 4. Data Flow Details

| Flow | Source | Destination | Data Elements | Volume | Frequency |
|------|--------|-----------|--------------|--------|----------|
| [Submit Request] | [Customer] | [System] | [Request details, documents] | [X/day] | [Real-time] |
| [Validate Request] | [System] | [System] | [Validated request, risk score] | [X/day] | [Real-time] |
| [Process Request] | [System] | [System] | [Decision, approval] | [X/day] | [Real-time] |
| [Sync Customer] | [ERP] | [System] | [Customer data] | [X records] | [Nightly] |
| [Process Payment] | [System] | [Payment] | [Amount, account] | [X/day] | [Real-time] |
| [Send Notification] | [System] | [Email] | [Recipient, message] | [X/day] | [Real-time] |

## 5. Data Transformation Rules

| Transformation | Input | Output | Rule |
|---------------|-------|--------|------|
| [Request Classification] | [Raw request] | [Classified request] | [Business rules engine] |
| [Risk Scoring] | [Request + Customer] | [Risk score] | [Risk algorithm] |
| [Notification Generation] | [Status change] | [Notification message] | [Template engine] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Architecture-Blueprint]] | Architecture context |
| [[Data-Lineage-Documentation]] | Detailed lineage |
| [[Data-Integration-Architecture]] | Integration details |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Data flows show *where data goes*. If you can't trace the flow, you can't protect or quality-check it.
