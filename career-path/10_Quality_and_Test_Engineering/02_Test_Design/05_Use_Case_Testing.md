---
title: Use Case Testing
parent: Test Design
topic: Testing user workflows end-to-end
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - test-design
  - use-case-testing
---

# Use Case Testing

> **Core Principle:** Test complete user workflows from start to finish, including main success path and alternative flows.

## What Use Case Testing Is

Use case testing is a technique that:
- Tests complete user workflows (use cases)
- Covers main success scenario and alternative flows
- Verifies system behavior from user perspective
- Validates business processes end-to-end

## When to Use Use Case Testing

**Use when:**
- Testing complete user workflows
- Validating business processes
- Verifying integration across components
- Testing from user perspective
- Acceptance testing

**Do not use when:**
- Testing individual components in isolation
- Verifying internal logic (use unit tests)
- Testing every possible code path

## Use Case Structure

A use case describes:
- **Actor:** Who interacts with the system
- **Preconditions:** What must be true before the use case starts
- **Main Success Scenario:** Happy path (most common flow)
- **Alternative Flows:** Variations and exceptions
- **Postconditions:** What is true after the use case completes

### Use Case Template

```markdown
Use Case: [Name]

Actor: [Primary actor]
Stakeholders: [Other interested parties]

Preconditions:
- [Condition 1]
- [Condition 2]

Main Success Scenario:
1. [Step 1]
2. [Step 2]
3. [Step 3]
...
n. [Postcondition achieved]

Alternative Flows:
- [Alternative 1]: [Description]
- [Alternative 2]: [Description]

Exceptions:
- [Exception 1]: [How system handles error]
- [Exception 2]: [How system handles error]

Postconditions:
- [Result 1]
- [Result 2]
```

## Examples

### Example 1: User Registration

```markdown
Use Case: Register New User

Actor: New user
Stakeholders: System administrator, marketing team

Preconditions:
- User is not logged in
- Registration page is accessible

Main Success Scenario:
1. User navigates to registration page
2. User enters username, email, and password
3. User clicks "Register"
4. System validates input
5. System creates user account
6. System sends confirmation email
7. System displays success message
8. User clicks confirmation link in email
9. System activates account
10. User can now log in

Alternative Flows:
- A1: User already has account (step 4)
  - System displays "Account already exists"
  - System offers "Log in" link
  - Use case ends

- A2: User chooses social login (step 2)
  - User clicks "Sign up with Google"
  - System redirects to Google OAuth
  - User authorizes application
  - System creates account from Google profile
  - Continue from step 6

Exceptions:
- E1: Invalid email format (step 4)
  - System displays "Invalid email address"
  - User corrects email
  - Resume from step 3

- E2: Username already taken (step 4)
  - System displays "Username not available"
  - System suggests alternatives
  - User chooses different username
  - Resume from step 3

- E3: Email service unavailable (step 6)
  - System queues email for later delivery
  - System logs error
  - Continue with use case

Postconditions:
- User account created and activated
- User can log in with credentials
- Confirmation email sent
```

**Test Cases:**

| Test ID | Flow | Steps | Expected Result |
|---------|------|-------|-----------------|
| TC1 | Main success | 1-10 | Account created, user can log in |
| TC2 | Alternative A1 | 1-4 | "Account exists" message, login link |
| TC3 | Alternative A2 | 1, 2a, 6-10 | Account created via Google |
| TC4 | Exception E1 | 1-4 | Error message, user corrects and continues |
| TC5 | Exception E2 | 1-4 | Error message, suggestions, user continues |
| TC6 | Exception E3 | 1-6 | Account created, email queued |

### Example 2: Online Purchase

```markdown
Use Case: Purchase Product

Actor: Customer
Stakeholders: Inventory system, payment processor, shipping

Preconditions:
- Customer is logged in
- Product is in stock
- Payment method configured

Main Success Scenario:
1. Customer adds product to cart
2. Customer proceeds to checkout
3. Customer enters shipping address
4. Customer selects shipping method
5. Customer enters payment information
6. Customer reviews order
7. Customer clicks "Place Order"
8. System validates payment
9. System reserves inventory
10. System creates order
11. System sends confirmation email
12. System displays order confirmation

Alternative Flows:
- A1: Apply coupon code (step 6)
  - Customer enters coupon code
  - System validates coupon
  - System applies discount
  - Resume from step 6

- A2: Use saved payment method (step 5)
  - Customer selects saved payment method
  - Resume from step 6

Exceptions:
- E1: Payment declined (step 8)
  - System displays "Payment declined"
  - Customer enters different payment method
  - Resume from step 8

- E2: Out of stock during checkout (step 9)
  - System displays "Product no longer available"
  - System removes product from cart
  - Customer can continue shopping or cancel

- E3: Network timeout (step 8)
  - System retries payment validation
  - If still fails, display error and preserve cart
  - Customer can retry

Postconditions:
- Order created
- Payment processed
- Inventory updated
- Confirmation email sent
```

**Test Cases:**

| Test ID | Flow | Expected Result |
|---------|------|-----------------|
| TC1 | Main success | Order placed, payment processed, email sent |
| TC2 | Alternative A1 | Discount applied, order placed |
| TC3 | Alternative A2 | Saved payment used, order placed |
| TC4 | Exception E1 | Payment error, customer can retry |
| TC5 | Exception E2 | Out of stock message, cart updated |
| TC6 | Exception E3 | Network error handled, cart preserved |

### Example 3: Password Reset

```markdown
Use Case: Reset Password

Actor: User who forgot password

Preconditions:
- User has registered account
- User has access to registered email

Main Success Scenario:
1. User clicks "Forgot Password"
2. User enters email address
3. System sends reset email with token
4. User clicks reset link in email
5. User enters new password twice
6. User clicks "Reset Password"
7. System validates password
8. System updates password
9. System invalidates reset token
10. System displays success message
11. User can log in with new password

Alternative Flows:
- A1: User remembers password (step 4)
  - User navigates to login page
  - Use case ends

Exceptions:
- E1: Email not found (step 3)
  - System displays generic message "If account exists, email sent"
  - Use case ends (security: don't reveal if email exists)

- E2: Token expired (step 4)
  - System displays "Reset link expired"
  - User must request new reset email
  - Resume from step 2

- E3: Password too weak (step 7)
  - System displays password requirements
  - User enters stronger password
  - Resume from step 6

Postconditions:
- Password updated
- Reset token invalidated
- User can log in with new password
- Old password no longer works
```

## Test Case Design from Use Cases

### Step 1: Identify Test Scenarios

From each use case, identify:
- Main success scenario (1 test case)
- Each alternative flow (1 test case each)
- Each exception (1 test case each)
- Combinations of alternatives and exceptions (for high-risk paths)

### Step 2: Define Test Data

For each test case:
- Valid data for main success
- Specific data to trigger alternatives
- Invalid data to trigger exceptions
- Boundary values for critical inputs

### Step 3: Define Expected Results

For each test case:
- Final state of system
- Output messages or UI changes
- Side effects (emails sent, logs created)
- Data changes (database updates)

### Step 4: Prioritize Test Cases

| Priority | Criteria |
|----------|----------|
| **Critical** | Main success scenario, critical exceptions |
| **High** | Common alternative flows, security-related exceptions |
| **Medium** | Rare alternatives, minor exceptions |
| **Low** | Edge cases, cosmetic issues |

## Use Case Testing vs Other Techniques

| Technique | Focus | When to Use |
|-----------|-------|-------------|
| **Use case testing** | Complete workflows | Acceptance testing, integration testing |
| **State transition testing** | State changes | When behavior depends on state |
| **Decision table testing** | Business rules | When multiple conditions interact |
| **Equivalence partitioning** | Input validation | Testing data validation |

**Combination:** Use case testing often incorporates other techniques:
- Use equivalence partitioning for input validation within use case steps
- Use state transition testing when use case involves state changes
- Use decision tables for complex business rules within use case

## Benefits and Limitations

### Benefits

| Benefit | Description |
|---------|-------------|
| **User perspective** | Tests system as users experience it |
| **End-to-end coverage** | Validates complete workflows |
| **Business alignment** | Tests business processes |
| **Integration testing** | Reveals integration issues |
| **Stakeholder communication** | Use cases are understandable by non-technical stakeholders |

### Limitations

| Limitation | Mitigation |
|-----------|-----------|
| **May miss edge cases** | Combine with other techniques |
| **Does not test internal logic** | Use unit tests for internal logic |
| **Can be slow** | Automate where possible |
| **May not cover all paths** | Use code coverage to identify gaps |

## Practical Tips

1. **Start with main success:** Test happy path first
2. **Add alternatives systematically:** Cover each alternative flow
3. **Test exceptions:** Verify error handling
4. **Use realistic data:** Test with production-like data
5. **Automate regression:** Automate main success scenarios
6. **Review with users:** Ensure use cases match real workflows
7. **Keep use cases updated:** Update as system evolves

## Key Takeaways

1. **Use case testing validates complete workflows:** From user perspective, start to finish
2. **Cover main success and alternatives:** Don't just test happy path
3. **Test exceptions:** Verify error handling and recovery
4. **Combine with other techniques:** Use case testing + equivalence partitioning + boundary analysis
5. **Prioritize based on risk:** Focus on critical workflows and common alternatives

## Related Topics

- [[04_State_Transition_Testing]]: When use cases involve state changes
- [[06_Exploratory_Testing]]: Finding defects use cases miss
- [[07_Test_Design_Strategy]]: Integrating use case testing into overall strategy

## Existing Vault Connections

- [[software-engineering-note/05_Software_Testing/04_Use_Case_Testing]]: Use case testing fundamentals
