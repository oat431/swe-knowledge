---
document_type: Physical Data Model (PDM)
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [physical-data-model, pdm, database-design, dmbok]
standard_ref:
  - DMBOK v2 — Data Modeling
---

# Physical Data Model (PDM)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Database-specific implementation of the logical model — tables, indexes, constraints, and storage details.

## 2. Database Configuration

| Aspect | Configuration |
|--------|-------------|
| [RDBMS] | [PostgreSQL 16] |
| [Character Set] | [UTF-8] |
| [Collation] | [en_US.UTF-8] |
| [Schema] | [public] |
| [Extensions] | [uuid-ossp, pgcrypto] |

## 3. Physical Model

```mermaid
erDiagram
    customers ||--o{ requests : submits
    requests ||--o{ transactions : contains
    requests }|--|| categories : belongs-to
    requests ||--o{ documents : has
    customers ||--o{ accounts : has
    accounts ||--o{ payments : receives
    staff ||--o{ requests : processes
    requests ||--|| statuses : has

    customers {
        uuid id PK
        varchar name
        varchar email UK
        varchar phone
        varchar type
        timestamptz created_at
        timestamptz updated_at
    }

    requests {
        uuid id PK
        uuid customer_id FK
        uuid category_id FK
        uuid status_id FK
        uuid assigned_to FK
        text description
        decimal amount
        varchar priority
        timestamptz submitted_at
        timestamptz updated_at
    }

    transactions {
        uuid id PK
        uuid request_id FK
        varchar type
        decimal amount
        varchar currency
        varchar reference
        timestamptz processed_at
    }

    categories {
        uuid id PK
        varchar name UK
        text description
        int sort_order
        boolean is_active
    }

    statuses {
        uuid id PK
        varchar name UK
        text description
        int sort_order
    }

    staff {
        uuid id PK
        varchar name
        varchar email UK
        varchar role
        varchar department
        boolean is_active
    }

    documents {
        uuid id PK
        uuid request_id FK
        varchar filename
        varchar file_type
        int file_size
        varchar storage_path
        timestamptz uploaded_at
    }

    accounts {
        uuid id PK
        uuid customer_id FK
        varchar account_number UK
        varchar type
        varchar status
        timestamptz opened_at
    }

    payments {
        uuid id PK
        uuid account_id FK
        decimal amount
        varchar currency
        varchar method
        varchar status
        timestamptz processed_at
    }
```

## 4. Table Definitions

### customers

```sql
CREATE TABLE customers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    phone VARCHAR(20),
    type VARCHAR(20) NOT NULL DEFAULT 'STANDARD',
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX idx_customers_email ON customers(email);
CREATE INDEX idx_customers_type ON customers(type);
```

### requests

```sql
CREATE TABLE requests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    customer_id UUID NOT NULL REFERENCES customers(id),
    category_id UUID NOT NULL REFERENCES categories(id),
    status_id UUID NOT NULL REFERENCES statuses(id),
    assigned_to UUID REFERENCES staff(id),
    description TEXT NOT NULL,
    amount DECIMAL(12,2) NOT NULL CHECK (amount > 0),
    priority VARCHAR(20) NOT NULL DEFAULT 'NORMAL',
    submitted_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX idx_requests_customer ON requests(customer_id);
CREATE INDEX idx_requests_status ON requests(status_id);
CREATE INDEX idx_requests_assigned ON requests(assigned_to);
CREATE INDEX idx_requests_submitted ON requests(submitted_at);
```

## 5. Indexes Summary

| Table | Index | Columns | Type | Purpose |
|-------|-------|---------|------|---------|
| [customers] | [idx_customers_email] | [email] | [B-tree, unique] | [Email lookup] |
| [requests] | [idx_requests_customer] | [customer_id] | [B-tree] | [Customer requests] |
| [requests] | [idx_requests_status] | [status_id] | [B-tree] | [Status filtering] |
| [requests] | [idx_requests_submitted] | [submitted_at] | [B-tree] | [Date sorting] |

## 6. Triggers

```sql
-- Auto-update updated_at
CREATE OR REPLACE FUNCTION update_updated_at()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = now();
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_customers_updated
    BEFORE UPDATE ON customers
    FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER trg_requests_updated
    BEFORE UPDATE ON requests
    FOR EACH ROW EXECUTE FUNCTION update_updated_at();
```

## 7. Storage Estimates

| Table | Rows (Year 1) | Row Size | Total Size |
|-------|-------------|---------|-----------|
| [customers] | [10,000] | [~500 bytes] | [~5 MB] |
| [requests] | [100,000] | [~800 bytes] | [~80 MB] |
| [transactions] | [200,000] | [~400 bytes] | [~80 MB] |
| [documents] | [150,000] | [~300 bytes] | [~45 MB] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Logical-Data-Model-LDM]] | Logical model |
| [[Database-Schema-DDL]] | DDL scripts |
| [[Data-Dictionary]] | Definitions |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** The PDM is the *physical reality*. Every table, index, and constraint. This is what gets deployed.
