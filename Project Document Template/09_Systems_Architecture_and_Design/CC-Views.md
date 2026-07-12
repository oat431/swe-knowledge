---
document_type: Component-and-Connector (C&C) Views
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
architect: "[Solution Architect]"
classification: "Internal / Confidential"
tags: [c-and-c, component-connector, runtime, swebok, iso-42010]
standard_ref:
  - SWEBOK v4 — Architecture
  - ISO/IEC/IEEE 42010 — Architecture Description
---

# Component-and-Connector (C&C) Views

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> C&C views show the runtime structure of the system — components (processing units) and connectors (communication mechanisms). Unlike module views (development-time), C&C views show how the system behaves when running.

## 2. C&C Legend

| Element | Representation | Description |
|---------|---------------|-------------|
| **Component** | [Rectangle] | [Processing unit — service, module, database] |
| **Connector** | [Arrow] | [Communication — REST, event, shared data] |
| **Port** | [Small square on component] | [Interface point] |
| **Process** | [Circle] | [Runtime process] |
| **Data Store** | [Cylinder] | [Persistent storage] |

## 3. C&C View: Request Processing

```mermaid
flowchart TB
    subgraph External["External Actors"]
        CUST([Customer])
        OPS([Operations])
    end

    subgraph System["System Components"]
        PORTAL[Customer Portal<br>Component]
        ADMIN[Admin Portal<br>Component]
        GW[API Gateway<br>Component]
        REQ[Request Service<br>Component]
        PROC[Processing Service<br>Component]
        AUTH[Auth Service<br>Component]
        NOTIFY[Notification Service<br>Component]
    end

    subgraph Data_Stores["Data Stores"]
        DB[(Application DB)]
        CACHE[(Redis Cache)]
        AUDIT[(Audit Log)]
        QUEUE{{Message Queue}}
    end

    subgraph External_Sys["External Systems"]
        ERP[(ERP)]
        EMAIL[Email Service]
    end

    CUST -->|HTTPS| PORTAL
    OPS -->|HTTPS| ADMIN
    PORTAL -->|REST| GW
    ADMIN -->|REST| GW
    GW -->|REST| REQ
    GW -->|REST| PROC
    GW -->|REST| AUTH
    REQ -->|TCP| DB
    REQ -->|TCP| CACHE
    REQ -->|Event| QUEUE
    PROC -->|TCP| DB
    PROC -->|Event| QUEUE
    AUTH -->|TCP| DB
    AUTH -->|TCP| CACHE
    QUEUE -->|Event| NOTIFY
    NOTIFY -->|SMTP| EMAIL
    REQ -->|REST| ERP

    style External fill:#607D8B,color:#fff
    style System fill:#2196F3,color:#fff
    style Data_Stores fill:#FF9800,color:#fff
    style External_Sys fill:#9C27B0,color:#fff
```

## 4. C&C View: Data Flow

```mermaid
flowchart LR
    subgraph Ingest["Data Ingestion"]
        PORTAL[Portal] -->|Request| API[API]
        API -->|Validate| VAL[Validator]
    end

    subgraph Process["Data Processing"]
        VAL -->|Store| DB[(DB)]
        VAL -->|Event| PROC[Processor]
        PROC -->|Approve| DB
        PROC -->|Notify| NOTIFY[Notifier]
    end

    subgraph Output["Data Output"]
        NOTIFY -->|Email| EMAIL[Email]
        NOTIFY -->|SMS| SMS[SMS]
        DB -->|Sync| ERP[(ERP)]
        DB -->|Report| DW[(DW)]
    end

    style Ingest fill:#4CAF50,color:#fff
    style Process fill:#2196F3,color:#fff
    style Output fill:#FF9800,color:#fff
```

## 5. Component Specification

| Component | Type | Ports | Connectors | Threading | State |
|-----------|------|-------|-----------|-----------|-------|
| [Customer Portal] | [Web App] | [HTTPS in] | [REST to Gateway] | [Stateless] | [Stateless] |
| [API Gateway] | [Gateway] | [HTTPS in, REST out] | [REST to Services] | [Stateless] | [Stateless] |
| [Request Service] | [Service] | [REST in, Event out] | [TCP to DB, Event to Queue] | [Multi-threaded] | [Stateless] |
| [Processing Service] | [Service] | [REST in, Event in/out] | [TCP to DB, Event to Queue] | [Multi-threaded] | [Stateless] |
| [Auth Service] | [Service] | [REST in] | [TCP to DB, TCP to Cache] | [Multi-threaded] | [Stateful (sessions)] |
| [Notification Service] | [Service] | [Event in] | [SMTP to Email, REST to SMS] | [Multi-threaded] | [Stateless] |
| [Application DB] | [Data Store] | [TCP in] | — | [Connection pool] | [Stateful] |
| [Message Queue] | [Middleware] | [AMQP in/out] | — | [Async] | [Stateful] |

## 6. Connector Specification

| Connector | Type | Protocol | Reliability | Ordering | Capacity |
|-----------|------|---------|------------|---------|----------|
| [REST API] | [Request-Response] | [HTTPS] | [At-most-once] | [N/A] | [100 req/s] |
| [Event Queue] | [Publish-Subscribe] | [AMQP] | [At-least-once] | [FIFO per queue] | [1000 msg/s] |
| [Database] | [Shared Data] | [TCP/5432] | [ACID] | [N/A] | [1000 conn] |
| [Cache] | [Shared Data] | [TCP/6379] | [Best effort] | [N/A] | [10000 ops/s] |
| [Email] | [Send] | [SMTP] | [At-most-once] | [N/A] | [100/min] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Logical-Architecture]] | Logical components |
| [[Physical-Architecture]] | Deployment of C&C |
| [[Interface-Control-Document]] | Connector specifications |
| [[Architecture-Views-4-1]] | Process view |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC/IEEE 42010
> **Usage:** C&C views show *runtime* behavior — how components interact when the system is running. Use them for performance analysis, failure analysis, and understanding data flow.
