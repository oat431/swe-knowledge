---
tags:
  - clean-code
  - documentation
  - comments
  - programming
---

# Comments & Documentation

Comments are a necessary evil. Good code is self-documenting, but some things can't be expressed in code alone — intent, warnings, legal requirements. The goal is to write code so clear that comments become rare and valuable.

---

## Bad Comments

### ❌ Redundant — Restates the Obvious

```java
// increment i by 1
i++;

// check if user is null
if (user == null) {
    throw new UserNotFoundException();
}
```

### ❌ Misleading — Outdated or Wrong

```java
// Validates email format — (written 3 years ago, regex was changed twice since)
private boolean isValid(String email) {
    return email.contains("@");  // doesn't actually validate format
}
```

### ❌ Commented-Out Code

```java
// UserService service = new UserService();
// service.process(data);
// List<User> users = service.findAll();
// TODO: maybe use this later
```
This is what git is for. Delete it.

### ❌ Noise — Adds Zero Information

```java
/**
 * Default constructor.
 */
public OrderService() {}

/**
 * The name field.
 */
private String name;
```

### ❌ Journal Comments

```java
// 2023-01-15 John: Added tax calculation
// 2023-02-20 Jane: Fixed rounding issue
// 2023-04-10 Bob: Refactored to use BigDecimal
```
Use `git log` instead.

---

## Good Comments

### ✅ Legal Comments

```java
// Copyright (c) 2024 Acme Corp. All rights reserved.
// Licensed under the MIT License. See LICENSE file.
```

### ✅ Clarification of Intent

```java
// Using bitwise AND to check if the 3rd bit is set
// This determines if the user has the EDIT permission
if ((permissions & EDIT_FLAG) != 0) {
    allowEditing();
}
```

### ✅ Warning of Consequences

```java
// WARNING: This method is called by the nightly batch job.
// Changing the return format will break downstream ETL pipelines.
public List<Report> generateDailyReport() { ... }
```

### ✅ TODO / FIXME

```java
// TODO: Replace with proper rate limiter once Redis is available
// FIXME: Race condition when two users edit the same order
// HACK: Temporary workaround for SDK bug (ticket PROJ-1234)
```

Always include a ticket number or responsible person.

### ✅ Amplification — Making the Unobvious Obvious

```java
// The trim() is critical — upstream system sends trailing spaces
// that cause lookup failures in the database
String normalized = input.trim().toLowerCase();
```

---

## Self-Documenting Code > Comments

The best comment is the one you don't need to write.

```java
// ❌ Needs a comment
int d; // elapsed time in days

// ✅ Self-documenting
int elapsedTimeInDays;

// ❌ Needs explanation
if (employee.flags & 0x0010) { ... }

// ✅ Named constant
if (employee.isRetired()) { ... }
```

---

## Javadoc / JSDoc

### When to Document

| Scenario | Document? | Why |
|----------|-----------|-----|
| Public API / library | ✅ Always | Consumers can't read your source |
| Non-obvious behavior | ✅ Yes | Side effects, thread safety, exceptions |
| Complex algorithm | ✅ Yes | Explain the *why*, not every line |
| Private helper methods | ⚠️ Only if complex | If it needs a comment, maybe it needs a better name |
| Obvious getters/setters | ❌ Skip | `getName()` returns the name — no docs needed |

### Java — Javadoc

```java
/**
 * Calculates the discount for a customer based on their loyalty tier.
 *
 * <p>Customers with 2+ years of history receive a 10% discount.
 * Premium tier customers receive an additional 5% on top.
 *
 * @param customer the customer to calculate discount for
 * @param orderTotal the order subtotal before discount
 * @return discount amount (never negative, capped at 50% of orderTotal)
 * @throws IllegalArgumentException if orderTotal is negative
 */
public BigDecimal calculateDiscount(Customer customer, BigDecimal orderTotal) { ... }
```

### TypeScript — JSDoc

```typescript
/**
 * Sends a notification to the user via their preferred channel.
 *
 * Falls back to email if the preferred channel is unavailable.
 * Throws `NotificationError` if all channels fail.
 *
 * @param userId - Target user ID
 * @param message - Notification content (max 500 chars)
 * @param options - Delivery options
 * @returns The delivery receipt with channel used
 * @throws {NotificationError} When all delivery attempts fail
 */
async function sendNotification(
  userId: string,
  message: string,
  options?: NotificationOptions
): Promise<DeliveryReceipt> { ... }
```

---

## README Template

Every project should have a README with these sections:

```markdown
# Project Name

One-line description of what this project does.

## Quick Start

```bash
# Prerequisites, install, and run in < 5 steps
npm install
npm run dev
```

## Usage

Brief code examples or screenshots.

## Configuration

Environment variables, config files.

## API Reference

Link to detailed docs or auto-generated API docs.

## Contributing

How to set up dev environment, run tests, submit PRs.

## License

MIT / Apache 2.0 / Proprietary
```

---

## API Documentation

For REST APIs, use **OpenAPI / Swagger** specification. See [[01 OpenAPI & Documentation]] for the full workflow: defining specs, generating clients, and publishing docs.

Key rules:
- Document every endpoint with request/response examples
- Include error codes and their meanings
- Keep the spec in version control alongside the code
- Generate docs from the spec, never maintain docs separately

---

## Documentation Checklist

- [ ] No commented-out code blocks
- [ ] Comments explain *why*, not *what*
- [ ] Public APIs have Javadoc/JSDoc
- [ ] README exists with Quick Start section
- [ ] TODOs include ticket numbers
- [ ] No outdated comments that contradict the code

---

## Book Deep Dive

For full chapter-level coverage:
- [[Comment Patterns]] — when to comment, comment types, anti-patterns

---

**Sources:** Robert C. Martin, *Clean Code*, Ch. 4 (2008); Divio documentation system; Google developer documentation style guide
