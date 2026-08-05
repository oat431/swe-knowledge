---
title: Acceptance Criteria
parent: Requirements
summary: Defining success conditions for requirements
tags:
  - requirements
  - acceptance-criteria
  - testing
  - verification
---

# Acceptance Criteria

> Acceptance criteria define when a requirement is done. They make success objective and testable, eliminating ambiguity about what "done" means.

## Why Acceptance Criteria Matter

**Without acceptance criteria:**
- "Done" means different things to different people
- Endless debates about completeness
- Quality varies
- Rework and disagreements

**With acceptance criteria:**
- Clear definition of done
- Objective verification
- Consistent quality
- Shared understanding

## Acceptance Criteria Formats

### 1. Given-When-Then (Gherkin)

**Behavior-driven format:**

```
Given [initial context]
When [action occurs]
Then [expected outcome]
```

**Example:**
```
Story: Customer search

Acceptance Criteria:

Given a customer named "John Smith" exists
When I search for "John"
Then I see John Smith in the results

Given no customer named "Jane" exists
When I search for "Jane"
Then I see "No results found"

Given 100 customers in the system
When I search for "Smith"
Then I see results in under 2 seconds
```

### 2. Checklist Format

**Simple list:**

```
Story: User login

Acceptance Criteria:
- User can enter email and password
- System validates email format
- System validates password (min 8 chars)
- Successful login redirects to dashboard
- Failed login shows error message
- After 3 failed attempts, account locked for 15 minutes
- Password field masked (shows dots)
- "Remember me" option available
```

### 3. Scenario Format

**Detailed scenarios:**

```
Story: Order placement

Acceptance Criteria:

Scenario 1: Successful order
- User has items in cart
- User enters valid shipping address
- User selects payment method
- User clicks "Place Order"
- Order created with unique ID
- Confirmation email sent
- Inventory updated

Scenario 2: Payment failure
- User enters invalid payment
- System shows error message
- Order not created
- Cart preserved
- User can retry

Scenario 3: Out of stock
- Item goes out of stock during checkout
- System notifies user
- Item removed from cart
- User can continue with remaining items
```

## Writing Good Acceptance Criteria

### Characteristics

**Good criteria are:**

**Specific:**
```
Bad: "Search should be fast"
Good: "Search returns results in under 2 seconds"
```

**Measurable:**
```
Bad: "System should handle many users"
Good: "System supports 500 concurrent users"
```

**Testable:**
```
Bad: "User experience should be good"
Good: "User can complete task in under 3 clicks"
```

**Unambiguous:**
```
Bad: "System should validate input"
Good: "System rejects email without @ symbol"
```

### Coverage

**Include:**

**Happy path:**
- Normal, expected behavior
- Success scenarios

**Edge cases:**
- Boundary conditions
- Unusual inputs

**Error cases:**
- Invalid inputs
- System failures
- Network issues

**Performance:**
- Response times
- Capacity
- Scalability

**Example:**
```
Story: File upload

Acceptance Criteria:

Happy path:
- User can upload files up to 10MB
- Supported formats: PDF, DOC, JPG
- Upload shows progress bar
- Successful upload shows confirmation

Edge cases:
- File exactly 10MB accepted
- File 10.1MB rejected with clear message
- Multiple files can be uploaded simultaneously

Error cases:
- Network interruption: Upload resumes or restarts
- Unsupported format: Clear error message
- Disk full: System error handled gracefully

Performance:
- 5MB file uploads in under 10 seconds
- Progress bar updates every second
```

## Acceptance Criteria Best Practices

### 1. Write Before Development

**Define success early:**
- Write criteria with story
- Review with team before starting
- Clarify before coding

### 2. Collaborate on Criteria

**Involve the team:**
- Product owner writes initial criteria
- Developers add technical considerations
- QA adds test scenarios
- Everyone agrees on "done"

### 3. Keep Criteria Focused

**Test one thing per criterion:**
```
Bad: "User can login and see dashboard and navigate to profile"
Good: 
- "Successful login redirects to dashboard"
- "Dashboard shows user name"
- "User can navigate to profile from dashboard"
```

### 4. Make Criteria Independent

**Each criterion stands alone:**
```
Bad: "After completing step 1, step 2 works"
Good:
- "Step 1 completes successfully"
- "Step 2 works when step 1 is complete"
```

### 5. Include Negative Cases

**Test what shouldn't happen:**
```
- System rejects invalid input
- Unauthorized users cannot access
- System handles errors gracefully
```

## Acceptance Criteria Examples

### Feature Story

```
Story: Shopping cart

Acceptance Criteria:

Adding items:
- User can add product to cart
- Cart shows item name, price, quantity
- Cart total updates automatically
- User can add multiple items

Updating quantities:
- User can increase quantity
- User can decrease quantity
- Quantity cannot be negative
- Cart total recalculates

Removing items:
- User can remove item from cart
- Cart total recalculates
- Confirmation shown before removal

Persistence:
- Cart persists across sessions
- Cart expires after 30 days of inactivity
- Cart available on all devices (logged in user)
```

### Technical Story

```
Story: API rate limiting

Acceptance Criteria:

Rate limits:
- Anonymous users: 100 requests per hour
- Authenticated users: 1000 requests per hour
- Premium users: 10000 requests per hour

Enforcement:
- Exceeded limit returns 429 status code
- Response includes retry-after header
- Rate limit resets at start of each hour

Monitoring:
- Rate limit usage logged
- Alerts when 80% of limit used
- Dashboard shows rate limit metrics
```

### Non-Functional Story

```
Story: Page load performance

Acceptance Criteria:

Load times:
- Homepage loads in under 2 seconds (95th percentile)
- Product page loads in under 3 seconds (95th percentile)
- Checkout page loads in under 4 seconds (95th percentile)

Conditions:
- Measured on 3G connection
- First-time visitor (no cache)
- Includes all assets (images, scripts, styles)

Monitoring:
- Performance metrics tracked
- Alerts when degradation > 10%
- Weekly performance report
```

## Common Acceptance Criteria Mistakes

### 1. Too Vague

**Mistake:**
```
- System should work correctly
- User experience should be good
```

**Fix:**
```
- System returns search results in under 2 seconds
- User can complete task in under 5 minutes
```

### 2. Too Detailed

**Mistake:**
```
- Click button at coordinates (150, 200)
- System executes SQL query SELECT * FROM users
```

**Fix:**
```
- User clicks search button
- System retrieves matching users
```

### 3. Implementation Focus

**Mistake:**
```
- System uses JWT tokens for authentication
- Database query optimized with index
```

**Fix:**
```
- User remains logged in for 24 hours
- Search returns results in under 2 seconds
```

### 4. Missing Edge Cases

**Mistake:**
```
- User can upload file
```

**Fix:**
```
- User can upload files up to 10MB
- System rejects files over 10MB with clear message
- System handles network interruption during upload
```

### 5. Not Testable

**Mistake:**
```
- System should be intuitive
- Code should be clean
```

**Fix:**
```
- New user can complete task without training
- Code passes static analysis with no critical issues
```

## Senior-Level Acceptance Criteria

1. **Strategic criteria**
   - Not just feature criteria
   - Business outcome criteria
   - Success metrics

2. **Criteria quality**
   - Establish criteria standards
   - Train teams in good criteria
   - Review and improve

3. **Criteria management**
   - Build criteria processes
   - Ensure criteria coverage
   - Maintain criteria quality

4. **Advanced patterns**
   - Complex acceptance patterns
   - Cross-system criteria
   - Integration criteria

## Metrics

- Criteria coverage (% of stories with criteria)
- Criteria quality (testable, specific, measurable)
- Acceptance rate (% criteria met on first attempt)
- Criteria clarity (team questions per story)
- Rework rate (stories reopened due to unclear criteria)

## Resources

- Writing Effective Use Cases by Alistair Cockburn
- The Cucumber Book by Aslak Hellesøy et al.
- [[body-of-knowledge/BABOK/05_Requirements_Analysis_and_Design]] - Requirements

## Checklist

Before writing criteria:
- [ ] Story clear and complete
- [ ] Stakeholders consulted
- [ ] Success defined
- [ ] Test scenarios identified

After writing criteria:
- [ ] Criteria specific and measurable
- [ ] Happy path covered
- [ ] Edge cases included
- [ ] Error cases defined
- [ ] Performance criteria set
- [ ] Reviewed with team
- [ ] Agreed by all

