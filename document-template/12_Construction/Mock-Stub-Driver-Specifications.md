---
document_type: Mock/Stub/Driver Specifications
version: "1.0"
status: Draft
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [mocks, stubs, drivers, testing, swebok]
standard_ref:
  - SWEBOK v4 — Construction
---

# Mock/Stub/Driver Specifications

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines test doubles — mocks, stubs, and drivers — used for isolated testing.

## 2. Test Double Types

| Type | Purpose | When to Use | Example |
|------|---------|-----------|---------|
| [Dummy] | [Passed but not used] | [Required parameter] | [Unused logger] |
| [Stub] | [Returns canned data] | [External dependency] | [Fake database] |
| [Spy] | [Records calls] | [Verify interactions] | [Event publisher spy] |
| [Mock] | [Verifies expectations] | [Behavior verification] | [Mock payment gateway] |
| [Fake] | [Working implementation] | [Expensive dependency] | [In-memory database] |

## 3. Test Doubles Register

| # | Name | Type | Replaces | Used In | Implementation |
|---|------|------|---------|---------|---------------|
| 1 | [InMemoryRequestRepository] | [Fake] | [PostgresRequestRepository] | [Unit tests] | [In-memory Map] |
| 2 | [MockEventPublisher] | [Mock] | [RabbitMQEventPublisher] | [Unit tests] | [Jest mock] |
| 3 | [StubAuthService] | [Stub] | [AuthService] | [Integration tests] | [Returns fixed user] |
| 4 | [SpyNotificationService] | [Spy] | [NotificationService] | [Unit tests] | [Records calls] |
| 5 | [FakePaymentGateway] | [Fake] | [PaymentGateway] | [Integration tests] | [Simulates responses] |

## 4. Mock Specifications

### MockEventPublisher

```typescript
// Mock specification
const mockEventPublisher: EventPublisher = {
  publish: jest.fn().mockResolvedValue(undefined),
};

// Usage in tests
it('should publish RequestCreated event', async () => {
  await requestService.create(dto);
  expect(mockEventPublisher.publish).toHaveBeenCalledWith(
    expect.objectContaining({
      type: 'RequestCreated',
      requestId: expect.any(String),
    })
  );
});
```

### StubAuthService

```typescript
// Stub specification
const stubAuthService: AuthService = {
  authenticate: jest.fn().mockResolvedValue({
    id: 'user-123',
    email: 'test@example.com',
    role: 'admin',
  }),
  authorize: jest.fn().mockResolvedValue(true),
};
```

### FakePaymentGateway

```typescript
// Fake specification
class FakePaymentGateway implements PaymentGateway {
  async processPayment(request: PaymentRequest): Promise<PaymentResult> {
    return {
      success: true,
      transactionId: `fake-${Date.now()}`,
      amount: request.amount,
    };
  }
}
```

## 5. Test Double Guidelines

| Rule | Rationale |
|------|----------|
| [Use fakes for databases] | [Fast, deterministic] |
| [Use mocks for event publishing] | [Verify events sent] |
| [Use stubs for external services] | [Control responses] |
| [Never mock what you own] | [Use real implementation] |
| [Keep test doubles simple] | [Don't test the test double] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[TDD-Test-Cases]] | Tests using these doubles |
| [[Test-Plan]] | Testing strategy |
| [[Coding-Standards]] | Code standards |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Test doubles enable *isolated testing*. Use the simplest double that works. Fakes > Stubs > Mocks.
