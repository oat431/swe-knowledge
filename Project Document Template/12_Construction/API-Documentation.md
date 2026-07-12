---
document_type: API Documentation
version: "1.0"
status: Draft
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [api-documentation, openapi, swagger, swebok]
standard_ref:
  - SWEBOK v4 — Construction
  - OpenAPI Specification 3.0
---

# API Documentation

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Auto-generated API documentation from OpenAPI/Swagger specifications — keeping docs in sync with code.

## 2. API Documentation Stack

| Tool | Purpose | URL |
|------|---------|-----|
| [Swagger UI] | [Interactive API explorer] | [/api-docs] |
| [Redoc] | [Clean API reference] | [/api-reference] |
| [OpenAPI Generator] | [Client SDK generation] | [CI/CD] |
| [Postman Collection] | [API testing] | [Shared workspace] |

## 3. OpenAPI Specification

```yaml
openapi: 3.0.3
info:
  title: Project API
  version: 1.0.0
  description: API for request management system
  contact:
    name: API Support
    email: api@project.com

servers:
  - url: https://api.project.com/v1
    description: Production
  - url: https://api-staging.project.com/v1
    description: Staging

paths:
  /requests:
    post:
      summary: Create a new request
      tags: [Requests]
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateRequestDTO'
      responses:
        '201':
          description: Request created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Request'
        '400':
          description: Validation error
        '401':
          description: Unauthorized

components:
  schemas:
    CreateRequestDTO:
      type: object
      required: [type, amount]
      properties:
        type:
          type: string
          enum: [STANDARD, VIP, CORPORATE]
        amount:
          type: number
          minimum: 0.01
          maximum: 999999.99
        description:
          type: string
          maxLength: 1000

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

## 4. API Documentation Generation

```bash
# Generate OpenAPI spec from code annotations
npm run generate:openapi

# Generate Swagger UI
npm run generate:swagger

# Generate Redoc
npm run generate:redoc

# Generate client SDK
npm run generate:sdk -- --lang=typescript
```

## 5. Documentation Standards

| Aspect | Standard |
|--------|---------|
| [Every Endpoint] | [Summary, description, parameters, responses] |
| [Every Schema] | [Description, example, validation rules] |
| [Error Responses] | [All possible error codes documented] |
| [Authentication] | [How to authenticate, token format] |
| [Examples] | [Request/response examples for each endpoint] |
| [Changelog] | [API version changes documented] |

## 6. API Versioning

| Version | Status | Base URL | Sunset Date |
|---------|--------|---------|------------|
| [v1] | [Active] | [/v1/] | — |
| [v2] | [Planned] | [/v2/] | — |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[API-Specification]] | Detailed API specs |
| [[Interface-Control-Document]] | System interfaces |
| [[README-Developer-Guide]] | Developer onboarding |

---

> **Template Standard:** Based on SWEBOK v4, OpenAPI 3.0
> **Usage:** API docs should be *auto-generated* from code annotations — never manually maintained. If the code changes, the docs update automatically.
