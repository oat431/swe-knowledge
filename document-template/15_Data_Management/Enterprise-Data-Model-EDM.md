---
document_type: Enterprise Data Model (EDM)
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [enterprise-data-model, edm, data-architecture, dmbok]
standard_ref:
  - DMBOK v2 — Data Architecture
---

# Enterprise Data Model (EDM)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> High-level, enterprise-wide view of data entities and their relationships — the blueprint for all data structures.

## 2. Enterprise Data Model

```mermaid
erDiagram
    CUSTOMER ||--o{ REQUEST : submits
    REQUEST ||--o{ TRANSACTION : contains
    REQUEST }|--|| CATEGORY : classified-as
    REQUEST ||--o{ DOCUMENT : has
    CUSTOMER ||--o{ ACCOUNT : has
    ACCOUNT ||--o{ PAYMENT : makes
    REQUEST ||--|| STATUS : has
    REQUEST ||--o{ AUDIT-LOG : generates

    CUSTOMER {
        uuid id PK
        string name
        string email
        string phone
        string type
        datetime created-at
    }

    REQUEST {
        uuid id PK
        uuid customer-id FK
        uuid category-id FK
        string description
        decimal amount
        uuid status-id FK
        datetime submitted-at
    }

    TRANSACTION {
        uuid id PK
        uuid request-id FK
        string type
        decimal amount
        datetime processed-at
    }

    CATEGORY {
        uuid id PK
        string name
        string description
        int priority
    }

    STATUS {
        uuid id PK
        string name
        string description
    }
```

## 3. Entity Summary

| Entity | Description | Domain | Owner | Classification |
|--------|-----------|--------|-------|---------------|
| [Customer] | [Individual or organization submitting requests] | [Customer] | [Customer Steward] | 🔴 Restricted |
| [Request] | [Formal submission for service or action] | [Operations] | [Operations Steward] | 🟡 Confidential |
| [Transaction] | [Financial or operational transaction] | [Financial] | [Financial Steward] | 🔴 Restricted |
| [Category] | [Classification of requests] | [Reference] | [Reference Steward] | 🟢 Internal |
| [Document] | [Attached files and documents] | [Operations] | [Operations Steward] | 🟡 Confidential |
| [Account] | [Customer account information] | [Customer] | [Customer Steward] | 🔴 Restricted |
| [Payment] | [Payment transactions] | [Financial] | [Financial Steward] | 🔴 Restricted |
| [Status] | [Lifecycle states] | [Reference] | [Reference Steward] | 🟢 Internal |
| [Audit-Log] | [System action records] | [System] | [System Steward] | 🟡 Confidential |

## 4. Business Rules

| # | Rule | Entities Affected |
|---|------|------------------|
| 1 | [Every request must have a customer] | [Request, Customer] |
| 2 | [Every request must have a status] | [Request, Status] |
| 3 | [Transactions link to requests] | [Transaction, Request] |
| 4 | [Documents link to requests] | [Document, Request] |
| 5 | [Payments link to accounts] | [Payment, Account] |

## 5. Model Governance

| Rule | Description |
|------|-----------|
| [Review frequency] | [Quarterly] |
| [Change process] | [Data Architecture Review Board] |
| [Version control] | [Git repository] |
| [Documentation] | [Data catalog + glossary] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Architecture-Blueprint]] | Architecture context |
| [[Logical-Data-Model-LDM]] | Logical implementation |
| [[Physical-Data-Model-PDM]] | Physical implementation |
| [[Business-Glossary]] | Term definitions |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** The EDM is the *master blueprint*. All physical data models derive from it. Changes to the EDM affect everything.
