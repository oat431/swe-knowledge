---
document_type: Architecture Views (4+1)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
architect: "[Solution Architect]"
classification: "Internal / Confidential"
tags: [architecture-views, 4+1, swebok, iso-42010]
standard_ref:
  - SWEBOK v4 — Architecture
  - ISO/IEC/IEEE 42010 — Architecture Description
  - Philippe Kruchten — 4+1 View Model
---

# Architecture Views (4+1)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document presents the architecture using Kruchten's 4+1 View Model — five complementary views that address different stakeholder concerns.

## 2. View Model Overview

| View | Stakeholder | Concern | Diagram |
|------|-----------|---------|---------|
| **Logical** | End Users, Developers | Functionality, structure | [[Logical-Architecture]] |
| **Process** | System Integrators | Concurrency, performance | Sequence diagrams |
| **Development** | Developers, Managers | Organization, reuse | Module diagrams |
| **Physical** | System Engineers | Deployment, infrastructure | [[Physical-Architecture]] |
| **Scenarios (+1)** | All | Validation, use cases | Use case diagrams |

## 3. Logical View

> **Stakeholder:** End Users, Developers
> **Concern:** What does the system do? How is the functionality structured?

```mermaid
flowchart TB
    subgraph Presentation["Presentation"]
        PORTAL[Customer Portal]
        ADMIN[Admin Portal]
        DASH[Dashboard]
    end

    subgraph Services["Services"]
        REQ[Request Service]
        PROC[Processing Service]
        AUTH[Auth Service]
        NOTIFY[Notification Service]
        RPT[Reporting Service]
        INT[Integration Service]
    end

    subgraph Data["Data"]
        DB[(Application DB)]
        AUDIT[(Audit Log)]
        DW[(Data Warehouse)]
    end

    Presentation --> Services
    Services --> Data

    style Presentation fill:#4CAF50,color:#fff
    style Services fill:#2196F3,color:#fff
    style Data fill:#9C27B0,color:#fff
```

> **Detail:** See [[Logical-Architecture]]

## 4. Process View

> **Stakeholder:** System Integrators
> **Concern:** How does the system handle concurrency, performance, and runtime behavior?

### 4.1 Key Process: Request Submission

```mermaid
sequenceDiagram
    participant C as Customer
    participant P as Portal
    participant GW as API Gateway
    participant RS as Request Service
    participant PS as Processing Service
    participant NS as Notification Service
    participant DB as Database

    C->>P: Submit Request
    P->>GW: POST /api/v1/requests
    GW->>GW: Authenticate (JWT)
    GW->>RS: Forward request
    RS->>DB: Store request
    RS-->>GW: 201 Created
    GW-->>P: 201 Created
    P-->>C: Success + Reference

    RS->>PS: Event: RequestCreated
    PS->>PS: Validate against rules
    PS->>PS: Classify & Route
    PS->>DB: Update status
    PS->>NS: Event: StatusChanged
    NS->>C: Email notification
```

### 4.2 Process Characteristics

| Process | Concurrency | Performance | Error Handling |
|---------|-----------|-------------|---------------|
| [Request Submission] | [Async processing after sync response] | [<5s end-to-end] | [Retry 3x, dead letter queue] |
| [Auto-Approval] | [Single-threaded per request] | [<1 minute] | [Fallback to manual review] |
| [Notification] | [Async, batched] | [<5 minutes] | [Retry 3x, alert on failure] |

## 5. Development View

> **Stakeholder:** Developers, Managers
> **Concern:** How is the system organized for development?

### 5.1 Repository Structure

```mermaid
flowchart TD
    REPO[monorepo] --> FE[frontend/]
    FE --> PORTAL[portal/]
    FE --> ADMIN[admin/]
    FE --> DASH[dashboard/]
    REPO --> BE[backend/]
    BE --> SVC[services/]
    SVC --> REQ[request/]
    SVC --> PROC[processing/]
    SVC --> AUTH_SVC[auth/]
    SVC --> NOTIFY_SVC[notification/]
    REPO --> INFRA[infra/]
    INFRA --> TF[terraform/]
    INFRA --> K8S[k8s/]
    REPO --> DOCS[docs/]

    style REPO fill:#1a237e,color:#fff
    style FE fill:#4CAF50,color:#fff
    style BE fill:#2196F3,color:#fff
    style INFRA fill:#FF9800,color:#fff
    style DOCS fill:#9C27B0,color:#fff
```

### 5.2 Module Dependencies

| Module | Depends On | Depended By |
|--------|-----------|------------|
| [Request Service] | [Auth, DB, Queue] | [Portal, Admin] |
| [Processing Service] | [Auth, DB, Queue] | [Request Service (events)] |
| [Auth Service] | [DB, Cache] | [All services] |
| [Notification Service] | [Queue, Email, SMS] | [Processing Service (events)] |

## 6. Physical View

> **Stakeholder:** System Engineers
> **Concern:** How is the system deployed?

> **Detail:** See [[Physical-Architecture]]

```mermaid
flowchart TB
    subgraph Cloud["Cloud — ap-southeast-1"]
        subgraph Pub["Public"]
            LB[Load Balancer]
        end
        subgraph App["Application"]
            CONTAINERS[Containers<br>Web × 2, API × 2, Worker × 2]
        end
        subgraph Data_Zone["Data"]
            DB_HA[(DB Primary + Replica)]
            CACHE_HA[(Redis Cluster)]
        end
    end

    LB --> CONTAINERS
    CONTAINERS --> DB_HA
    CONTAINERS --> CACHE_HA

    style Cloud fill:#e8f5e9,color:#000
    style Pub fill:#c8e6c9,color:#000
    style App fill:#a5d6a7,color:#000
    style Data_Zone fill:#81c784,color:#000
```

## 7. Scenarios View (+1)

> **Stakeholder:** All
> **Concern:** How do the views work together for key use cases?

### 7.1 Key Scenarios

| # | Scenario | Components | Views Validated |
|---|---------|-----------|----------------|
| 1 | [Customer submits request] | [Portal, API, Request Svc, DB, Notification] | Logical, Process, Physical |
| 2 | [Operations processes request] | [Admin, API, Processing Svc, DB, Notification] | Logical, Process |
| 3 | [Management views dashboard] | [Dashboard, API, Reporting Svc, DW] | Logical, Process |
| 4 | [System handles peak load] | [All — auto-scaled] | Process, Physical |
| 5 | [Service failure recovery] | [Health checks, failover, retry] | Process, Physical |

### 7.2 Scenario Validation Matrix

| Scenario | Logical | Process | Development | Physical | Status |
|----------|---------|---------|------------|---------|--------|
| [Submit Request] | ✅ | ✅ | ✅ | ✅ | ✅ Validated |
| [Process Request] | ✅ | ✅ | ✅ | ✅ | ✅ Validated |
| [View Dashboard] | ✅ | ✅ | ✅ | ✅ | ✅ Validated |
| [Peak Load] | N/A | ✅ | N/A | ✅ | ✅ Validated |
| [Service Failure] | N/A | ✅ | N/A | ✅ | ✅ Validated |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Logical-Architecture]] | Logical view detail |
| [[Physical-Architecture]] | Physical view detail |
| [[Software-Architecture-Document]] | Comprehensive architecture |
| [[System-Architecture-Description]] | System-level architecture |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC/IEEE 42010, Kruchten 4+1
> **Usage:** The 4+1 model ensures all stakeholder concerns are addressed. Use it for architecture reviews — each view can be reviewed independently by the relevant stakeholders.
