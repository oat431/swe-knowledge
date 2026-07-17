---
document_type: Test Data
version: "1.0"
status: Active
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [test-data, fixtures, factories, swebok]
standard_ref:
  - SWEBOK v4 — Testing
---

# Test Data

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines test data strategy — how test data is created, managed, and maintained across testing levels.

## 2. Test Data Strategy

| Level | Data Source | Creation | Refresh |
|-------|-----------|---------|---------|
| [Unit] | [Factory functions] | [In-test] | [Per test] |
| [Integration] | [Fixtures + seed scripts] | [Before suite] | [Per suite] |
| [System] | [Anonymized production] | [Weekly refresh] | [Weekly] |
| [UAT] | [Anonymized production] | [Before UAT] | [Per cycle] |

## 3. Test Data Factories

### Customer Factory

```typescript
// Factory for creating test customers
const customerFactory = {
  build: (overrides = {}) => ({
    id: generateUUID(),
    name: `Test Customer ${Date.now()}`,
    email: `test${Date.now()}@example.com`,
    phone: '+1-555-0100',
    ...overrides,
  }),
  
  create: async (overrides = {}) => {
    const customer = customerFactory.build(overrides);
    await db.customers.insert(customer);
    return customer;
  },
};
```

### Request Factory

```typescript
const requestFactory = {
  build: (overrides = {}) => ({
    id: generateUUID(),
    customerId: generateUUID(),
    type: 'STANDARD',
    amount: 5000,
    status: 'DRAFT',
    description: 'Test request',
    ...overrides,
  }),
  
  create: async (overrides = {}) => {
    const request = requestFactory.build(overrides);
    await db.requests.insert(request);
    return request;
  },
};
```

## 4. Test Fixtures

| Fixture | Description | File | Records |
|---------|-----------|------|---------|
| [customers.json] | [Test customers] | [fixtures/customers.json] | [10] |
| [requests.json] | [Test requests] | [fixtures/requests.json] | [25] |
| [users.json] | [Test users] | [fixtures/users.json] | [5] |
| [roles.json] | [Test roles] | [fixtures/roles.json] | [4] |

## 5. Seed Scripts

```bash
# Seed test database
npm run db:seed:dev      # Development data
npm run db:seed:test     # Test data
npm run db:seed:uat      # UAT data

# Reset and re-seed
npm run db:reset:test    # Reset + seed test database
```

## 6. Anonymization Rules

| Field | Method | Example |
|-------|--------|---------|
| [Name] | [Replace with fake] | [John Doe → Test User 123] |
| [Email] | [Replace domain] | [user@real.com → user@test.com] |
| [Phone] | [Replace last 4] | [555-0123 → 555-XXXX] |
| [Amount] | [Keep original] | [Preserved for testing] |
| [Address] | [Replace with fake] | [123 Main St → 456 Test Ave] |

## 7. Test Data Cleanup

| Strategy | When | Method |
|---------|------|--------|
| [Rollback] | [After each test] | [Transaction rollback] |
| [Truncate] | [After each suite] | [TRUNCATE tables] |
| [Reset] | [After each day] | [Re-seed database] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Test-Cases]] | Cases using this data |
| [[Test-Plan]] | Data strategy |
| [[Database-Schema-DDL]] | Schema for test data |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Test data should be *deterministic* — same input, same output. Never rely on production data for tests.
