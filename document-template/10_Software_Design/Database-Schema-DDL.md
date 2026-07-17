---
document_type: Database Schema (DDL)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [database-schema, ddl, postgresql, swebok]
standard_ref:
  - SWEBOK v4 — Design
---

# Database Schema (DDL)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document provides the physical database schema — DDL statements, indexes, constraints, and stored procedures.

## 2. Database Overview

| Field | Detail |
|-------|--------|
| [RDBMS] | [PostgreSQL 15] |
| [Character Set] | [UTF-8] |
| [Collation] | [en_US.UTF-8] |
| [Schema] | [public] |
| [Naming Convention] | [snake_case, lowercase] |

## 3. DDL Scripts

### 3.1 Customers Table

```sql
CREATE TABLE customers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    phone VARCHAR(20),
    address TEXT,
    metadata JSONB DEFAULT '{}',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    CONSTRAINT uk_customers_email UNIQUE (email),
    CONSTRAINT chk_customers_email CHECK (email ~* '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$')
);

CREATE INDEX idx_customers_name ON customers(name);
CREATE INDEX idx_customers_created ON customers(created_at);
```

### 3.2 Requests Table

```sql
CREATE TABLE requests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    customer_id UUID NOT NULL,
    type VARCHAR(50) NOT NULL,
    amount DECIMAL(12,2) NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'DRAFT',
    description TEXT,
    metadata JSONB DEFAULT '{}',
    assigned_to UUID,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    submitted_at TIMESTAMP WITH TIME ZONE,
    completed_at TIMESTAMP WITH TIME ZONE,

    CONSTRAINT fk_requests_customer FOREIGN KEY (customer_id)
        REFERENCES customers(id) ON DELETE RESTRICT,
    CONSTRAINT fk_requests_assigned FOREIGN KEY (assigned_to)
        REFERENCES users(id) ON DELETE SET NULL,
    CONSTRAINT chk_requests_type CHECK (type IN ('STANDARD', 'VIP', 'CORPORATE')),
    CONSTRAINT chk_requests_status CHECK (status IN (
        'DRAFT', 'SUBMITTED', 'VALIDATING', 'CLASSIFYING',
        'ROUTED', 'UNDER_REVIEW', 'APPROVED', 'REJECTED'
    )),
    CONSTRAINT chk_requests_amount CHECK (amount > 0 AND amount <= 999999.99)
);

CREATE INDEX idx_requests_customer ON requests(customer_id);
CREATE INDEX idx_requests_status ON requests(status);
CREATE INDEX idx_requests_type ON requests(type);
CREATE INDEX idx_requests_assigned ON requests(assigned_to);
CREATE INDEX idx_requests_created ON requests(created_at DESC);
CREATE INDEX idx_requests_submitted ON requests(submitted_at);
CREATE INDEX idx_requests_status_created ON requests(status, created_at DESC);
```

### 3.3 Documents Table

```sql
CREATE TABLE documents (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    request_id UUID NOT NULL,
    filename VARCHAR(255) NOT NULL,
    content_type VARCHAR(100) NOT NULL,
    size_bytes INTEGER NOT NULL,
    storage_url VARCHAR(500) NOT NULL,
    uploaded_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    CONSTRAINT fk_documents_request FOREIGN KEY (request_id)
        REFERENCES requests(id) ON DELETE CASCADE,
    CONSTRAINT chk_documents_size CHECK (size_bytes > 0 AND size_bytes <= 10485760),
    CONSTRAINT chk_documents_content_type CHECK (content_type IN (
        'application/pdf', 'image/jpeg', 'image/png',
        'application/msword', 'application/vnd.openxmlformats-officedocument.wordprocessingml.document'
    ))
);

CREATE INDEX idx_documents_request ON documents(request_id);
```

### 3.4 Users Table

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) NOT NULL,
    name VARCHAR(255) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role_id UUID NOT NULL,
    is_active BOOLEAN DEFAULT true,
    last_login TIMESTAMP WITH TIME ZONE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    CONSTRAINT uk_users_email UNIQUE (email),
    CONSTRAINT fk_users_role FOREIGN KEY (role_id)
        REFERENCES roles(id) ON DELETE RESTRICT
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_role ON users(role_id);
CREATE INDEX idx_users_active ON users(is_active);
```

### 3.5 Roles Table

```sql
CREATE TABLE roles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(50) NOT NULL,
    description TEXT,
    permissions JSONB DEFAULT '[]',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    CONSTRAINT uk_roles_name UNIQUE (name)
);

-- Default roles
INSERT INTO roles (name, description, permissions) VALUES
    ('admin', 'System Administrator', '["*"]'),
    ('manager', 'Operations Manager', '["requests:read", "requests:approve", "requests:reject", "reports:read"]'),
    ('staff', 'Operations Staff', '["requests:read", "requests:process"]'),
    ('customer', 'Customer', '["requests:create", "requests:read:own"]');
```

### 3.6 Audit Log Table

```sql
CREATE TABLE audit_log (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    entity_type VARCHAR(50) NOT NULL,
    entity_id UUID NOT NULL,
    action VARCHAR(50) NOT NULL,
    actor_id UUID,
    actor_type VARCHAR(20),
    changes JSONB,
    ip_address INET,
    user_agent TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    CONSTRAINT chk_audit_action CHECK (action IN (
        'CREATE', 'UPDATE', 'DELETE', 'STATUS_CHANGE', 'LOGIN', 'LOGOUT'
    ))
);

CREATE INDEX idx_audit_entity ON audit_log(entity_type, entity_id);
CREATE INDEX idx_audit_actor ON audit_log(actor_id);
CREATE INDEX idx_audit_created ON audit_log(created_at DESC);
CREATE INDEX idx_audit_action ON audit_log(action);
```

## 4. Triggers

### 4.1 Updated At Trigger

```sql
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER trg_customers_updated
    BEFORE UPDATE ON customers
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER trg_requests_updated
    BEFORE UPDATE ON requests
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER trg_users_updated
    BEFORE UPDATE ON users
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
```

### 4.2 Audit Log Trigger

```sql
CREATE OR REPLACE FUNCTION create_audit_log()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO audit_log (entity_type, entity_id, action, actor_id, changes)
    VALUES (
        TG_TABLE_NAME,
        NEW.id,
        CASE
            WHEN TG_OP = 'INSERT' THEN 'CREATE'
            WHEN TG_OP = 'UPDATE' THEN 'UPDATE'
            WHEN TG_OP = 'DELETE' THEN 'DELETE'
        END,
        current_setting('app.current_user_id', true)::UUID,
        CASE
            WHEN TG_OP = 'UPDATE' THEN jsonb_build_object('old', to_jsonb(OLD), 'new', to_jsonb(NEW))
            WHEN TG_OP = 'DELETE' THEN jsonb_build_object('old', to_jsonb(OLD))
            ELSE NULL
        END
    );
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER trg_requests_audit
    AFTER INSERT OR UPDATE OR DELETE ON requests
    FOR EACH ROW EXECUTE FUNCTION create_audit_log();
```

## 5. Database Migrations

| Version | Date | Description | Script |
|---------|------|-------------|--------|
| [v1.0] | [YYYY-MM-DD] | [Initial schema] | [001_initial.sql] |
| [v1.1] | [YYYY-MM-DD] | [Add metadata columns] | [002_add_metadata.sql] |
| [v1.2] | [YYYY-MM-DD] | [Add audit triggers] | [003_audit_triggers.sql] |

## 6. Backup & Recovery

| Strategy | Frequency | Retention | Method |
|----------|-----------|-----------|--------|
| [Full backup] | [Daily] | [30 days] | [pg_dump] |
| [WAL archiving] | [Continuous] | [7 days] | [Archive to S3] |
| [Point-in-time recovery] | [On demand] | [7 days] | [pg_restore] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[ERD]] | Logical model this schema implements |
| [[Data-Dictionary]] | Data element definitions |
| [[Low-Level-Design]] | Design using this schema |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** This is the *physical* database schema. Use migration tools (Flyway, Liquibase, Prisma Migrate) to manage schema changes. Never modify production schema manually.
