---
document_type: Component Diagrams
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [component-diagrams, uml, interfaces, swebok, iso-19501]
standard_ref:
  - SWEBOK v4 — Design
  - ISO/IEC 19501 — UML
---

# Component Diagrams

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document shows UML component diagrams — the system's physical components, their interfaces, and dependencies.

## 2. Component Diagram: System Overview

```mermaid
flowchart TB
    subgraph Frontend["Frontend Components"]
        PORTAL[Customer Portal<br><<component>>]
        ADMIN[Admin Portal<br><<component>>]
        DASH[Dashboard<br><<component>>]
    end

    subgraph Backend["Backend Components"]
        subgraph RequestSvc["Request Service"]
            REQ_API[Request API<br><<component>>]
            REQ_DOMAIN[Request Domain<br><<component>>]
            REQ_INFRA[Request Infra<br><<component>>]
        end

        subgraph ProcessingSvc["Processing Service"]
            PROC_API[Processing API<br><<component>>]
            PROC_DOMAIN[Processing Domain<br><<component>>]
            PROC_RULES[Rules Engine<br><<component>>]
        end

        subgraph AuthSvc["Auth Service"]
            AUTH_API[Auth API<br><<component>>]
            AUTH_DOMAIN[Auth Domain<br><<component>>]
        end

        subgraph NotificationSvc["Notification Service"]
            NOTIF_HANDLER[Notification Handler<br><<component>>]
            NOTIF_TEMPLATES[Template Engine<br><<component>>]
        end
    end

    subgraph Infrastructure["Infrastructure Components"]
        GATEWAY[API Gateway<br><<component>>]
        DB_ADAPTER[Database Adapter<br><<component>>]
        CACHE_ADAPTER[Cache Adapter<br><<component>>]
        QUEUE_ADAPTER[Queue Adapter<br><<component>>]
        EXT_ADAPTER[External Adapter<br><<component>>]
    end

    PORTAL -->|<<uses>>| GATEWAY
    ADMIN -->|<<uses>>| GATEWAY
    DASH -->|<<uses>>| GATEWAY
    GATEWAY -->|<<routes>>| REQ_API
    GATEWAY -->|<<routes>>| PROC_API
    GATEWAY -->|<<routes>>| AUTH_API

    REQ_API -->|<<uses>>| REQ_DOMAIN
    REQ_DOMAIN -->|<<uses>>| REQ_INFRA
    REQ_INFRA -->|<<uses>>| DB_ADAPTER
    REQ_INFRA -->|<<uses>>| QUEUE_ADAPTER

    PROC_API -->|<<uses>>| PROC_DOMAIN
    PROC_DOMAIN -->|<<uses>>| PROC_RULES
    PROC_DOMAIN -->|<<uses>>| DB_ADAPTER

    AUTH_API -->|<<uses>>| AUTH_DOMAIN
    AUTH_DOMAIN -->|<<uses>>| DB_ADAPTER
    AUTH_DOMAIN -->|<<uses>>| CACHE_ADAPTER

    NOTIF_HANDLER -->|<<uses>>| NOTIF_TEMPLATES
    NOTIF_HANDLER -->|<<uses>>| EXT_ADAPTER

    style Frontend fill:#4CAF50,color:#fff
    style Backend fill:#2196F3,color:#fff
    style Infrastructure fill:#FF9800,color:#fff
```

## 3. Component Specifications

### 3.1 Request Service Components

| Component | Provided Interfaces | Required Interfaces | Dependencies |
|-----------|-------------------|-------------------|-------------|
| [Request API] | [IRequestAPI] | [IRequestDomain] | [Request Domain] |
| [Request Domain] | [IRequestDomain] | [IRequestRepository, IEventPublisher] | [Request Infra] |
| [Request Infra] | [IRequestRepository, IEventPublisher] | [IDatabase, IQueue] | [DB Adapter, Queue Adapter] |

### 3.2 Interface Definitions

| Interface | Component | Methods | Description |
|-----------|----------|---------|-------------|
| [IRequestAPI] | [Request API] | [create(), getById(), list(), update(), getStatus()] | [REST API for requests] |
| [IRequestDomain] | [Request Domain] | [create(), submit(), approve(), reject()] | [Domain logic for requests] |
| [IRequestRepository] | [Request Infra] | [save(), findById(), findByFilters(), update()] | [Data access for requests] |
| [IEventPublisher] | [Request Infra] | [publish(event)] | [Event publishing] |
| [IRuleEngine] | [Rules Engine] | [evaluate(request)] | [Business rule evaluation] |

### 3.3 Provided/Required Interfaces

```mermaid
flowchart LR
    REQ_API[Request API] -->|<<provides>>| I_API[IRequestAPI<br><<interface>>]
    I_API -->|<<requires>>| I_DOMAIN[IRequestDomain<br><<interface>>]
    I_DOMAIN -->|<<provided by>>| REQ_DOMAIN[Request Domain]
    I_DOMAIN -->|<<requires>>| I_REPO[IRequestRepository<br><<interface>>]
    I_REPO -->|<<provided by>>| REQ_INFRA[Request Infra]
```

## 4. Component Dependency Matrix

| Component | Request Domain | Processing Domain | Auth Domain | Notification | DB Adapter | Queue Adapter | Cache Adapter | External Adapter |
|-----------|---------------|------------------|------------|-------------|-----------|--------------|--------------|-----------------|
| [Request API] | ✅ | — | ✅ | — | — | — | — | — |
| [Request Domain] | — | — | — | — | ✅ | ✅ | — | — |
| [Processing API] | — | ✅ | ✅ | — | — | — | — | — |
| [Processing Domain] | ✅ | — | — | ✅ | ✅ | ✅ | — | — |
| [Auth API] | — | — | ✅ | — | — | — | — | — |
| [Auth Domain] | — | — | — | — | ✅ | — | ✅ | — |
| [Notification Handler] | — | — | — | — | ✅ | ✅ | — | ✅ |

## 5. Component Packaging

| Package | Components | Version | Repository |
|---------|-----------|---------|-----------|
| [frontend-portal] | [Customer Portal] | [v1.0] | [github.com/org/frontend-portal] |
| [frontend-admin] | [Admin Portal, Dashboard] | [v1.0] | [github.com/org/frontend-admin] |
| [service-request] | [Request API, Domain, Infra] | [v1.0] | [github.com/org/service-request] |
| [service-processing] | [Processing API, Domain, Rules] | [v1.0] | [github.com/org/service-processing] |
| [service-auth] | [Auth API, Domain] | [v1.0] | [github.com/org/service-auth] |
| [service-notification] | [Notification Handler, Templates] | [v1.0] | [github.com/org/service-notification] |
| [shared-infra] | [DB, Cache, Queue, External Adapters] | [v1.0] | [github.com/org/shared-infra] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Class Diagrams]] | Internal structure of components |
| [[Interface Control Document (ICD)]] | Interface specifications |
| [[Deployment Diagrams]] | Physical deployment of components |
| [[Architecture Views (4+1)]] | Development view |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 19501 (UML)
> **Usage:** Component diagrams show *physical structure* — what components exist and how they connect via interfaces. Use them for system integration, dependency management, and release planning.
