---
document_type: Test Scripts (Automated)
version: "1.0"
status: Active
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [test-scripts, automation, playwright, jest, swebok]
standard_ref:
  - SWEBOK v4 — Testing
---

# Test Scripts (Automated)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Documents automated test scripts — framework, structure, and execution commands.

## 2. Automation Stack

| Layer | Framework | Language | Runner |
|-------|----------|---------|--------|
| [Unit] | [Jest] | [TypeScript] | [npm run test:unit] |
| [Integration] | [Jest + Supertest] | [TypeScript] | [npm run test:integration] |
| [E2E] | [Playwright] | [TypeScript] | [npm run test:e2e] |
| [Visual] | [Playwright screenshots] | [TypeScript] | [npm run test:visual] |

## 3. Test Script Structure

```
tests/
├── unit/
│   ├── services/
│   │   ├── request-service.test.ts
│   │   ├── processing-service.test.ts
│   │   └── auth-service.test.ts
│   └── utils/
│       └── validation.test.ts
├── integration/
│   ├── api/
│   │   ├── requests-api.test.ts
│   │   └── auth-api.test.ts
│   └── services/
│       └── notification-service.test.ts
├── e2e/
│   ├── request-submission.spec.ts
│   ├── request-processing.spec.ts
│   └── user-login.spec.ts
├── fixtures/
│   ├── customers.json
│   └── requests.json
└── helpers/
    ├── test-utils.ts
    └── page-objects/
```

## 4. Example Test Scripts

### Unit Test

```typescript
// tests/unit/services/request-service.test.ts
import { RequestService } from '@services/request/request-service';
import { InMemoryRequestRepository } from '@test-doubles/in-memory-repository';
import { MockEventPublisher } from '@test-doubles/mock-event-publisher';

describe('RequestService', () => {
  let service: RequestService;
  let repository: InMemoryRequestRepository;
  let eventPublisher: MockEventPublisher;

  beforeEach(() => {
    repository = new InMemoryRequestRepository();
    eventPublisher = new MockEventPublisher();
    service = new RequestService(repository, eventPublisher);
  });

  describe('create', () => {
    it('should create a request with valid data', async () => {
      const dto = { type: 'STANDARD', amount: 5000 };
      const request = await service.create(dto);
      expect(request.id).toBeDefined();
      expect(request.status).toBe('DRAFT');
    });

    it('should reject amount of 0', async () => {
      const dto = { type: 'STANDARD', amount: 0 };
      await expect(service.create(dto)).rejects.toThrow('Amount must be positive');
    });
  });
});
```

### E2E Test

```typescript
// tests/e2e/request-submission.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Request Submission', () => {
  test('should submit valid request', async ({ page }) => {
    await page.goto('/login');
    await page.fill('#email', 'customer@test.com');
    await page.fill('#password', 'password123');
    await page.click('#login-button');
    
    await page.click('text=New Request');
    await page.fill('#amount', '5000');
    await page.fill('#description', 'Test request');
    await page.click('text=Next');
    await page.click('text=Submit');
    
    await expect(page.locator('.success-message')).toBeVisible();
    await expect(page.locator('.reference-number')).not.toBeEmpty();
  });
});
```

## 5. Execution Commands

| Command | Purpose | Duration |
|---------|---------|---------|
| [npm run test:unit] | [Run all unit tests] | [< 30 seconds] |
| [npm run test:integration] | [Run integration tests] | [< 2 minutes] |
| [npm run test:e2e] | [Run E2E tests] | [< 10 minutes] |
| [npm run test:all] | [Run all tests] | [< 15 minutes] |
| [npm run test:coverage] | [Run with coverage report] | [< 2 minutes] |
| [npm run test:watch] | [Watch mode for development] | [Continuous] |

## 6. CI/CD Integration

```yaml
# GitHub Actions
- name: Run Unit Tests
  run: npm run test:unit -- --ci --coverage

- name: Run Integration Tests
  run: npm run test:integration -- --ci

- name: Run E2E Tests
  run: npx playwright install && npm run test:e2e -- --ci
```

## 7. Automation Metrics

| Metric | Target | Current | Status |
|--------|--------|---------|--------|
| [Total automated tests] | [≥ 60] | [X] | 🟢🟡🔴 |
| [Automation rate] | [≥ 80%] | [X%] | 🟢🟡🔴 |
| [Test execution time] | [< 15 min] | [X min] | 🟢🟡🔴 |
| [Flaky test rate] | [< 5%] | [X%] | 🟢🟡🔴 |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Test-Cases]] | Cases implemented as scripts |
| [[Test-Suite]] | Suite organization |
| [[Mock-Stub-Driver-Specifications]] | Test doubles used |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Automated tests are *safety nets*. Run them often. Fix flaky tests immediately — they erode trust.
