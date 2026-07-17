---
document_type: Coding Standards / Style Guide
version: "1.0"
status: Draft
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [coding-standards, style-guide, linting, swebok]
standard_ref:
  - SWEBOK v4 — Construction
---

# Coding Standards / Style Guide

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Coding standards ensure consistent, readable, maintainable code across the team. Every developer writes code the same way.

## 2. Language & Framework

| Aspect | Standard |
|--------|---------|
| [Language] | [TypeScript (strict mode)] |
| [Runtime] | [Node.js v20 LTS] |
| [Framework] | [Express.js v4] |
| [Frontend] | [React v18 + Next.js] |
| [Package Manager] | [npm] |

## 3. Code Style

### 3.1 General Rules

| Rule | Standard | Example |
|------|---------|---------|
| [Indentation] | [2 spaces] | [No tabs] |
| [Line Length] | [100 characters max] | [Break long lines] |
| [Semicolons] | [Required] | [Always use ;] |
| [Quotes] | [Single quotes] | ['string', not "string"] |
| [Trailing Commas] | [Required] | [Last item has comma] |
| [Naming — Variables] | [camelCase] | [requestCount] |
| [Naming — Classes] | [PascalCase] | [RequestService] |
| [Naming — Constants] | [UPPER_SNAKE] | [MAX_RETRY_COUNT] |
| [Naming — Files] | [kebab-case] | [request-service.ts] |

### 3.2 TypeScript Rules

| Rule | Standard |
|------|---------|
| [Strict Mode] | [Enabled — `strict: true`] |
| [Any] | [Never use `any`] |
| [Return Types] | [Always specify] |
| [Interface vs Type] | [Interface for objects, Type for unions] |
| [Null Handling] | [Use optional chaining, nullish coalescing] |

### 3.3 Example

```typescript
// ✅ Good
interface CreateRequestDTO {
  type: RequestType;
  amount: number;
  description?: string;
}

async function createRequest(dto: CreateRequestDTO): Promise<Request> {
  const validated = validateDTO(dto);
  const request = await requestRepository.save(validated);
  await eventPublisher.publish(new RequestCreatedEvent(request));
  return request;
}

// ❌ Bad
async function createRequest(dto: any): Promise<any> {
  // No type safety, no validation
  return await db.query("INSERT INTO requests...");
}
```

## 4. Linting & Formatting

| Tool | Config | Command |
|------|--------|---------|
| [ESLint] | [.eslintrc.js] | [npm run lint] |
| [Prettier] | [.prettierrc] | [npm run format] |
| [Husky] | [pre-commit hook] | [Auto-runs on commit] |

### ESLint Rules

```json
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],
  "rules": {
    "no-console": "warn",
    "no-unused-vars": "error",
    "@typescript-eslint/explicit-function-return-type": "error",
    "@typescript-eslint/no-explicit-any": "error"
  }
}
```

### Prettier Config

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100,
  "tabWidth": 2
}
```

## 5. File Structure

| Pattern | Example | Purpose |
|---------|--------|---------|
| [Feature-based] | [services/request/] | [Group by feature] |
| [Layer-based] | [app/, domain/, infra/] | [Group by layer] |
| [Test co-location] | [*.test.ts next to *.ts] | [Tests near code] |

```
services/request/
├── app/
│   ├── commands/
│   │   ├── create-request.ts
│   │   └── create-request.test.ts
│   └── queries/
│       ├── get-request.ts
│       └── get-request.test.ts
├── domain/
│   ├── entities/
│   │   └── request.ts
│   └── events/
│       └── request-created.ts
└── infra/
    ├── persistence/
    │   └── request-repository.ts
    └── messaging/
        └── request-event-publisher.ts
```

## 6. Code Review Standards

| Aspect | Standard |
|--------|---------|
| [PR Size] | [< 400 lines changed] |
| [Reviewers] | [Minimum 1 approval] |
| [Tests] | [Required for all changes] |
| [Documentation] | [Required for public APIs] |
| [Linting] | [Must pass before merge] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[README-Developer-Guide]] | Setup and contribution guide |
| [[Code-Review-Records]] | Review records |
| [[Static-Analysis-Reports]] | Linting results |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Standards are *enforced*, not suggested. Use linting in CI — if it doesn't pass, it doesn't merge.
