---
document_type: API Specification
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [api-specification, openapi, rest, swebok]
standard_ref:
  - SWEBOK v4 — Design
  - OpenAPI Specification 3.0
---

# API Specification

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document defines the API contracts for all REST endpoints. It serves as the contract between frontend and backend teams.

## 2. API Overview

| Field | Detail |
|-------|--------|
| [Base URL] | [https://api.project.com/v1] |
| [Protocol] | [HTTPS] |
| [Format] | [JSON] |
| [Authentication] | [Bearer JWT token] |
| [Rate Limiting] | [100 requests/minute per user] |
| [Versioning] | [URL path — /v1/] |

## 3. Authentication

| Method | Implementation | Token Lifetime | Refresh |
|--------|---------------|---------------|---------|
| [OAuth2 + JWT] | [Auth Service] | [30 minutes] | [Refresh token — 7 days] |

### Headers

```
Authorization: Bearer <jwt_token>
Content-Type: application/json
X-Request-ID: <uuid>
X-Correlation-ID: <uuid>
```

## 4. Common Response Formats

### Success Response

```json
{
  "data": { ... },
  "meta": {
    "requestId": "uuid",
    "timestamp": "ISO8601"
  }
}
```

### Error Response

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": [
      { "field": "fieldName", "message": "Validation error" }
    ]
  },
  "meta": {
    "requestId": "uuid",
    "timestamp": "ISO8601"
  }
}
```

### Error Codes

| Code | HTTP Status | Description |
|------|-----------|-------------|
| [VALIDATION_ERROR] | [400] | [Input validation failed] |
| [UNAUTHORIZED] | [401] | [Missing or invalid token] |
| [FORBIDDEN] | [403] | [Insufficient permissions] |
| [NOT_FOUND] | [404] | [Resource not found] |
| [CONFLICT] | [409] | [Resource conflict] |
| [RATE_LIMITED] | [429] | [Too many requests] |
| [INTERNAL_ERROR] | [500] | [Server error] |
| [SERVICE_UNAVAILABLE] | [503] | [Dependency unavailable] |

## 5. API Endpoints

### 5.1 Requests

#### POST /requests — Create Request

| Field | Detail |
|-------|--------|
| [Description] | [Create a new request] |
| [Auth] | [JWT — Customer role] |
| [Rate Limit] | [100/min] |

**Request Body:**

```json
{
  "type": "STANDARD",
  "amount": 5000.00,
  "description": "Request description",
  "documents": [
    {
      "filename": "support.pdf",
      "contentType": "application/pdf",
      "base64": "..."
    }
  ]
}
```

**Response (201):**

```json
{
  "data": {
    "id": "uuid",
    "customerId": "uuid",
    "type": "STANDARD",
    "amount": 5000.00,
    "status": "SUBMITTED",
    "description": "Request description",
    "documents": [...],
    "createdAt": "ISO8601",
    "updatedAt": "ISO8601"
  }
}
```

**Validation Rules:**

| Field | Rule | Error |
|-------|------|-------|
| [type] | [Required, enum: STANDARD, VIP, CORPORATE] | [VALIDATION_ERROR] |
| [amount] | [Required, > 0, ≤ 999999.99] | [VALIDATION_ERROR] |
| [description] | [Optional, max 1000 chars] | [VALIDATION_ERROR] |
| [documents] | [Optional, max 5 files, each ≤ 10MB] | [VALIDATION_ERROR] |

#### GET /requests — List Requests

| Field | Detail |
|-------|--------|
| [Description] | [List requests with filters and pagination] |
| [Auth] | [JWT — Customer or Admin role] |

**Query Parameters:**

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| [status] | [string] | [all] | [Filter by status] |
| [type] | [string] | [all] | [Filter by type] |
| [from] | [date] | [30 days ago] | [Start date filter] |
| [to] | [date] | [now] | [End date filter] |
| [page] | [number] | [1] | [Page number] |
| [limit] | [number] | [20] | [Items per page, max 100] |
| [sort] | [string] | [createdAt] | [Sort field] |
| [order] | [string] | [desc] | [Sort direction] |

**Response (200):**

```json
{
  "data": [ ... ],
  "meta": {
    "total": 100,
    "page": 1,
    "limit": 20,
    "pages": 5
  }
}
```

#### GET /requests/:id — Get Request

| Field | Detail |
|-------|--------|
| [Description] | [Get a single request by ID] |
| [Auth] | [JWT — Owner or Admin] |

**Response (200):** [Same as create response]

**Error (404):**

```json
{
  "error": {
    "code": "NOT_FOUND",
    "message": "Request not found"
  }
}
```

#### PUT /requests/:id — Update Request

| Field | Detail |
|-------|--------|
| [Description] | [Update a request — only in DRAFT status] |
| [Auth] | [JWT — Owner] |

**Request Body:** [Same as create, all fields optional]

#### GET /requests/:id/status — Get Status

| Field | Detail |
|-------|--------|
| [Description] | [Get current status and history] |
| [Auth] | [JWT — Owner or Admin] |

**Response (200):**

```json
{
  "data": {
    "currentStatus": "APPROVED",
    "history": [
      { "status": "SUBMITTED", "at": "ISO8601", "by": "system" },
      { "status": "VALIDATING", "at": "ISO8601", "by": "system" },
      { "status": "APPROVED", "at": "ISO8601", "by": "admin-uuid" }
    ]
  }
}
```

### 5.2 Processing (Admin)

#### POST /processing/:id/approve — Approve Request

| Field | Detail |
|-------|--------|
| [Auth] | [JWT — Admin role] |
| [Request] | `{ "comment": "Approved" }` |
| [Response] | [200 + Updated request] |

#### POST /processing/:id/reject — Reject Request

| Field | Detail |
|-------|--------|
| [Auth] | [JWT — Admin role] |
| [Request] | `{ "reason": "Missing documentation" }` |
| [Response] | [200 + Updated request] |

#### POST /processing/:id/escalate — Escalate Request

| Field | Detail |
|-------|--------|
| [Auth] | [JWT — Admin role] |
| [Request] | `{ "reason": "Requires manager approval" }` |
| [Response] | [200 + Updated request] |

### 5.3 Reports

#### GET /reports/dashboard — Dashboard Data

| Field | Detail |
|-------|--------|
| [Auth] | [JWT — Admin or Manager role] |
| [Response] | [Dashboard metrics] |

**Response (200):**

```json
{
  "data": {
    "requestsToday": 45,
    "avgProcessingTime": 2.5,
    "queueDepth": 12,
    "slaCompliance": 98.5,
    "statusBreakdown": {
      "pending": 12,
      "processing": 8,
      "approved": 20,
      "rejected": 5
    }
  }
}
```

## 6. Pagination

| Param | Description | Default | Max |
|-------|-----------|---------|-----|
| [page] | [Page number, 1-based] | [1] | — |
| [limit] | [Items per page] | [20] | [100] |

**Pagination Meta:**

```json
{
  "meta": {
    "total": 100,
    "page": 1,
    "limit": 20,
    "pages": 5,
    "hasNext": true,
    "hasPrev": false
  }
}
```

## 7. Filtering & Sorting

| Feature | Implementation | Example |
|---------|---------------|---------|
| [Filtering] | [Query params] | [?status=APPROVED&type=VIP] |
| [Sorting] | [sort + order params] | [?sort=createdAt&order=desc] |
| [Date Range] | [from + to params] | [?from=2026-01-01&to=2026-12-31] |
| [Search] | [q param] | [?q=search+term] |

## 8. Rate Limiting

| Tier | Limit | Window | Response |
|------|-------|--------|---------|
| [Standard] | [100 requests] | [1 minute] | [429 when exceeded] |
| [Admin] | [500 requests] | [1 minute] | [429 when exceeded] |

**Rate Limit Headers:**

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1640000000
```

## 9. Versioning Strategy

| Strategy | Implementation | Migration |
|----------|---------------|----------|
| [URL Path] | [/v1/, /v2/] | [Support N and N-1 versions] |
| [Deprecation] | [Sunset header] | [6 months notice] |
| [Breaking Changes] | [New version required] | [Major version bump] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Low-Level-Design]] | Implementation details |
| [[Interface-Control-Document]] | System-level interfaces |
| [[API-Specification]] | Auto-generated API docs |

---

> **Template Standard:** Based on SWEBOK v4, OpenAPI Specification 3.0
> **Usage:** This is the *contract* between frontend and backend. Both teams code against this spec. Generate OpenAPI YAML from this document for tooling integration.
