---
document_type: Data Standards
version: "1.0"
status: Draft
author: "[Data Steward]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-standards, naming-conventions, dmbok]
standard_ref:
  - DMBOK v2 — Data Governance
---

# Data Standards

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines technical standards for data — naming conventions, formats, definitions, and quality rules.

## 2. Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| [Database tables] | [snake_case, plural] | [customers, requests] |
| [Columns] | [snake_case, singular] | [customer_id, created_at] |
| [Primary keys] | [id] | [id] |
| [Foreign keys] | [entity_id] | [customer_id] |
| [Indexes] | [idx_table_column] | [idx_requests_status] |
| [Timestamps] | [action_at] | [created_at, updated_at] |
| [Booleans] | [is_xxx / has_xxx] | [is_active, has_documents] |
| [Enums] | [UPPER_CASE] | [STANDARD, VIP, CORPORATE] |

## 3. Data Formats

| Type | Format | Standard | Example |
|------|--------|---------|---------|
| [Date] | [ISO 8601] | [YYYY-MM-DD] | [2026-07-12] |
| [DateTime] | [ISO 8601] | [YYYY-MM-DDTHH:MM:SSZ] | [2026-07-12T14:30:00Z] |
| [Currency] | [ISO 4217] | [3-letter code] | [USD, THB] |
| [Phone] | [E.164] | [+country number] | [+66812345678] |
| [Email] | [RFC 5322] | [Valid email format] | [user@example.com] |
| [UUID] | [RFC 4122] | [UUID v4] | [550e8400-e29b-41d4-a716-446655440000] |

## 4. Data Classification Standards

| Level | Label | Description | Controls |
|-------|-------|-----------|---------|
| [L1] | 🔴 Restricted | [Highly sensitive — PII, financial] | [Encryption, access logging, MFA] |
| [L2] | 🟡 Confidential | [Internal business data] | [Access control, encryption in transit] |
| [L3] | 🟢 Internal | [General internal data] | [Access control] |
| [L4] | ⚪ Public | [Publicly available data] | [None] |

## 5. Data Quality Standards

| Dimension | Definition | Threshold | Measurement |
|----------|-----------|----------|-----------|
| [Accuracy] | [Data correctly represents reality] | [≥ 99%] | [Validation rules] |
| [Completeness] | [All required fields populated] | [≥ 95%] | [Null check] |
| [Consistency] | [Same data across systems] | [≥ 99%] | [Cross-system comparison] |
| [Timeliness] | [Data available when needed] | [< 1 hour lag] | [Latency monitoring] |
| [Uniqueness] | [No duplicate records] | [≥ 99%] | [Deduplication check] |
| [Validity] | [Data conforms to rules] | [≥ 99%] | [Format validation] |

## 6. Data Definition Standards

| Field | Required | Example |
|-------|---------|---------|
| [Business Name] | ✅ | [Customer Email Address] |
| [Technical Name] | ✅ | [customer_email] |
| [Definition] | ✅ | [The email address used for customer communications] |
| [Data Type] | ✅ | [VARCHAR(255)] |
| [Format] | ✅ | [RFC 5322 email] |
| [Valid Values] | ✅ | [Valid email format] |
| [Source] | ✅ | [Customer registration form] |
| [Owner] | ✅ | [Data Steward — Customer Domain] |
| [Classification] | ✅ | [L2 — Confidential] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Policy]] | Policy backing standards |
| [[Business-Glossary]] | Term definitions |
| [[Data-Dictionary]] | Technical definitions |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Standards ensure *consistency*. Without them, every developer invents their own naming, format, and quality rules.
