---
document_type: Data Dictionary
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [data-dictionary, data-elements, metadata, swebok, dmbok]
standard_ref:
  - SWEBOK v4 — Design
  - DMBOK v2 — Data Modeling & Design
  - ISO/IEC 11179 — Metadata Registries
---

# Data Dictionary

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document provides standardized definitions of all data elements — names, types, formats, valid values, business rules, and ownership.

## 2. Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| [Table names] | [snake_case, plural] | [customers, requests] |
| [Column names] | [snake_case, singular] | [customer_id, created_at] |
| [Primary keys] | [id] | [id] |
| [Foreign keys] | [entity_id] | [customer_id, role_id] |
| [Timestamps] | [action_at] | [created_at, updated_at, submitted_at] |
| [Booleans] | [is_xxx / has_xxx] | [is_active, has_permission] |
| [Enums] | [UPPER_CASE] | [STANDARD, VIP, CORPORATE] |

## 3. Data Elements

### 3.1 Customer Data Elements

| Element | Type | Format | Length | Required | Default | Valid Values | Business Rule | Owner |
|---------|------|--------|--------|----------|---------|-------------|---------------|-------|
| [id] | [UUID] | [UUID v4] | [36] | [Yes] | [auto] | [Valid UUID] | [Auto-generated, immutable] | [System] |
| [name] | [VARCHAR] | [Text] | [255] | [Yes] | — | [Any] | [Full name, 2-255 chars] | [Customer] |
| [email] | [VARCHAR] | [Email] | [255] | [Yes] | — | [Valid email] | [Unique, used for login] | [Customer] |
| [phone] | [VARCHAR] | [Phone] | [20] | [No] | [NULL] | [Valid phone] | [International format preferred] | [Customer] |
| [address] | [TEXT] | [Text] | [Unlimited] | [No] | [NULL] | [Any] | [Full mailing address] | [Customer] |
| [metadata] | [JSONB] | [JSON] | [Unlimited] | [No] | ['{}'] | [Valid JSON] | [Flexible additional data] | [System] |
| [created_at] | [TIMESTAMP] | [ISO 8601] | — | [Yes] | [NOW()] | [Valid timestamp] | [Immutable after creation] | [System] |
| [updated_at] | [TIMESTAMP] | [ISO 8601] | — | [Yes] | [NOW()] | [Valid timestamp] | [Auto-updated on change] | [System] |

### 3.2 Request Data Elements

| Element | Type | Format | Length | Required | Default | Valid Values | Business Rule | Owner |
|---------|------|--------|--------|----------|---------|-------------|---------------|-------|
| [id] | [UUID] | [UUID v4] | [36] | [Yes] | [auto] | [Valid UUID] | [Auto-generated, immutable] | [System] |
| [customer_id] | [UUID] | [UUID v4] | [36] | [Yes] | — | [Valid customer ID] | [Must reference existing customer] | [System] |
| [type] | [VARCHAR] | [Enum] | [50] | [Yes] | — | [STANDARD, VIP, CORPORATE] | [Determines processing rules] | [Customer] |
| [amount] | [DECIMAL] | [Numeric] | [12,2] | [Yes] | — | [0.01 - 999999.99] | [Must be positive] | [Customer] |
| [status] | [VARCHAR] | [Enum] | [20] | [Yes] | [DRAFT] | [See Status Values] | [Valid transitions only] | [System] |
| [description] | [TEXT] | [Text] | [Unlimited] | [No] | [NULL] | [0-1000 chars] | [Optional description] | [Customer] |
| [assigned_to] | [UUID] | [UUID v4] | [36] | [No] | [NULL] | [Valid user ID] | [Staff member assigned] | [System] |
| [submitted_at] | [TIMESTAMP] | [ISO 8601] | — | [No] | [NULL] | [Valid timestamp] | [Set when status → SUBMITTED] | [System] |
| [completed_at] | [TIMESTAMP] | [ISO 8601] | — | [No] | [NULL] | [Valid timestamp] | [Set when status → APPROVED/REJECTED] | [System] |

### 3.3 Status Values

| Value | Description | Valid Transitions | Terminal |
|-------|-----------|------------------|---------|
| [DRAFT] | [Request being created] | [SUBMITTED] | [No] |
| [SUBMITTED] | [Request submitted by customer] | [VALIDATING] | [No] |
| [VALIDATING] | [System validating inputs] | [CLASSIFYING, DRAFT] | [No] |
| [CLASSIFYING] | [System classifying request] | [ROUTED] | [No] |
| [ROUTED] | [Assigned to queue/staff] | [UNDER_REVIEW] | [No] |
| [UNDER_REVIEW] | [Staff reviewing] | [APPROVED, REJECTED] | [No] |
| [APPROVED] | [Request approved] | — | [Yes] |
| [REJECTED] | [Request rejected] | — | [Yes] |

### 3.4 Document Data Elements

| Element | Type | Format | Length | Required | Default | Valid Values | Business Rule | Owner |
|---------|------|--------|--------|----------|---------|-------------|---------------|-------|
| [id] | [UUID] | [UUID v4] | [36] | [Yes] | [auto] | [Valid UUID] | [Auto-generated] | [System] |
| [request_id] | [UUID] | [UUID v4] | [36] | [Yes] | — | [Valid request ID] | [Must reference existing request] | [System] |
| [filename] | [VARCHAR] | [Text] | [255] | [Yes] | — | [Valid filename] | [Original filename preserved] | [Customer] |
| [content_type] | [VARCHAR] | [MIME] | [100] | [Yes] | — | [Allowed MIME types] | [PDF, JPG, PNG, DOC, DOCX] | [System] |
| [size_bytes] | [INTEGER] | [Numeric] | — | [Yes] | — | [1 - 10485760] | [Max 10MB per file] | [System] |
| [storage_url] | [VARCHAR] | [URL] | [500] | [Yes] | — | [Valid URL] | [S3/storage URL] | [System] |

## 4. Business Rules Reference

| Rule ID | Rule | Elements Affected | Type |
|---------|------|------------------|------|
| [BR-001] | [Email must be unique] | [customers.email] | [Constraint] |
| [BR-002] | [Amount must be positive] | [requests.amount] | [Validation] |
| [BR-003] | [Status transitions must be valid] | [requests.status] | [Business logic] |
| [BR-004] | [Max 5 documents per request] | [documents.request_id] | [Business logic] |
| [BR-005] | [Max 10MB per document] | [documents.size_bytes] | [Validation] |
| [BR-006] | [Duplicate request within 30 days flagged] | [requests] | [Business logic] |
| [BR-007] | [VIP auto-approve ≤ $25K] | [requests.type, requests.amount] | [Business logic] |
| [BR-008] | [Standard auto-approve ≤ $10K] | [requests.type, requests.amount] | [Business logic] |
| [BR-009] | [Data retained 7 years after completion] | [All tables] | [Compliance] |

## 5. Data Quality Rules

| Rule | Element | Dimension | Threshold | Measurement |
|------|---------|-----------|-----------|-------------|
| [Valid email format] | [customers.email] | [Validity] | [100%] | [Regex validation] |
| [No null required fields] | [All required] | [Completeness] | [100%] | [NOT NULL constraint] |
| [Unique emails] | [customers.email] | [Uniqueness] | [100%] | [UNIQUE constraint] |
| [Valid status transitions] | [requests.status] | [Consistency] | [100%] | [Application logic] |
| [Amount precision] | [requests.amount] | [Accuracy] | [100%] | [DECIMAL(12,2)] |

## 6. Data Classification

| Data Element | Classification | Handling | Encryption |
|-------------|---------------|---------|-----------|
| [Customer PII] | 🔴 Confidential | [Need-to-know, audit access] | [At rest + in transit] |
| [Request data] | 🟡 Internal | [Internal only] | [In transit] |
| [User credentials] | 🔴 Restricted | [Never exposed, hashed] | [At rest + in transit] |
| [Audit logs] | 🟡 Internal | [Immutable, append-only] | [At rest] |
| [System metadata] | 🟢 Public | [No restrictions] | [In transit] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[ERD]] | Logical data model |
| [[Database-Schema-DDL]] | Physical schema |
| [[Data-Dictionary]] | Classification levels |

---

> **Template Standard:** Based on SWEBOK v4, DMBOK v2, ISO/IEC 11179
> **Usage:** The data dictionary is the *single source of truth* for data element definitions. When in doubt about a field's meaning, format, or rules — look it up here.
