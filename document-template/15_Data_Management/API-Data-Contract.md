---
document_type: API Data Contract
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [api-data-contract, schema, dmbok]
standard_ref:
  - DMBOK v2 — Data Integration
---

# API Data Contract

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines data schemas and contracts for API interfaces — ensuring consistent data exchange between systems.

## 2. API Schema Definitions

### Customer Response Schema

```json
{
  "type": "object",
  "properties": {
    "id": { "type": "string", "format": "uuid" },
    "name": { "type": "string", "maxLength": 255 },
    "email": { "type": "string", "format": "email" },
    "phone": { "type": "string", "pattern": "^\\+[1-9]\\d{1,14}$" },
    "type": { "type": "string", "enum": ["STANDARD", "VIP", "CORPORATE"] },
    "created_at": { "type": "string", "format": "date-time" },
    "updated_at": { "type": "string", "format": "date-time" }
  },
  "required": ["id", "name", "email", "type", "created_at"]
}
```

### Request Schema

```json
{
  "type": "object",
  "properties": {
    "id": { "type": "string", "format": "uuid" },
    "customer_id": { "type": "string", "format": "uuid" },
    "category": { "type": "string", "enum": ["STANDARD", "VIP", "CORPORATE"] },
    "description": { "type": "string", "maxLength": 5000 },
    "amount": { "type": "number", "minimum": 0.01 },
    "currency": { "type": "string", "pattern": "^[A-Z]{3}$" },
    "status": { "type": "string", "enum": ["PENDING", "IN_PROGRESS", "APPROVED", "REJECTED", "COMPLETED"] },
    "priority": { "type": "string", "enum": ["LOW", "NORMAL", "HIGH", "URGENT"] },
    "submitted_at": { "type": "string", "format": "date-time" }
  },
  "required": ["id", "customer_id", "category", "description", "amount", "currency", "status", "submitted_at"]
}
```

### Error Response Schema

```json
{
  "type": "object",
  "properties": {
    "error": {
      "type": "object",
      "properties": {
        "code": { "type": "string" },
        "message": { "type": "string" },
        "details": { "type": "array", "items": { "type": "string" } }
      },
      "required": ["code", "message"]
    }
  }
}
```

## 3. Data Validation Rules

| Field | Rule | Error Message |
|-------|------|-------------|
| [email] | [Valid email format] | [Invalid email address] |
| [phone] | [E.164 format] | [Invalid phone number] |
| [amount] | [> 0] | [Amount must be positive] |
| [currency] | [ISO 4217] | [Invalid currency code] |
| [date] | [ISO 8601] | [Invalid date format] |

## 4. Versioning

| Version | Changes | Compatibility |
|---------|---------|-------------|
| [v1.0] | [Initial release] | — |
| [v1.1] | [Added phone field] | [Backward compatible] |
| [v2.0] | [Changed status enum] | [Breaking change] |

## 5. Deprecation Policy

| Phase | Duration | Action |
|-------|---------|--------|
| [Announce] | [90 days before] | [Notify consumers] |
| [Deprecate] | [30 days] | [Sunset header, docs updated] |
| [Remove] | [After 30 days] | [Endpoint removed] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[API-Specification]] | Full API spec |
| [[Data-Interface-Agreement-DIA]] | Interface agreements |
| [[Data-Standards]] | Data standards |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** API contracts are *promises*. Breaking them breaks consumers. Version everything. Deprecate gracefully.
