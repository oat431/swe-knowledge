---
title: User Stories
parent: Requirements
summary: Writing requirements as user stories
tags:
  - requirements
  - user-stories
  - agile
  - format
---

# User Stories

> User stories express requirements from the user's perspective. They capture who needs what and why, in language everyone understands.

## Why User Stories Matter

**Traditional requirements:**
```
The system shall provide search functionality with advanced 
filtering capabilities and result sorting options.
```

**User story:**
```
As a customer service rep
I want to search for customers by name or email
So that I can quickly find the right customer record
```

**Benefits:**
- User-focused (not system-focused)
- Easy to understand (non-technical)
- Encourages conversation
- Flexible (doesn't over-specify)

## User Story Format

### Basic Structure

```
As a [user type]
I want [goal/feature]
So that [benefit/value]
```

**Components:**
- **Role:** Who needs this (user type)
- **Goal:** What they want to do
- **Benefit:** Why they want it

### Example Stories

**Simple:**
```
As a customer
I want to reset my password
So that I can regain access to my account
```

**Complex:**
```
As a product manager
I want to export analytics data to CSV
So that I can create custom reports for stakeholders
```

**Technical:**
```
As a system administrator
I want to configure email templates
So that I can customize notifications for our brand
```

## Writing Good User Stories

### INVEST Criteria

**Good stories are:**

**I - Independent:**
- Can be developed separately
- Minimal dependencies
- Can be prioritized independently

**N - Negotiable:**
- Not a contract
- Details emerge through conversation
- Flexible implementation

**V - Valuable:**
- Delivers value to user
- Worth building
- Clear benefit

**E - Estimable:**
- Team can estimate effort
- Clear enough to size
- Not too vague

**S - Small:**
- Can be completed in one sprint
- Not too large
- Break down if needed

**T - Testable:**
- Clear how to verify
- Objective criteria
- Can be demonstrated

### Story Quality Checklist

```
✓ Is it from user perspective?
✓ Is the benefit clear?
✓ Is it independent?
✓ Is it valuable?
✓ Can we estimate it?
✓ Is it small enough?
✓ Can we test it?
```

## Story Refinement

### Breaking Down Large Stories

**Epic (too large):**
```
As a customer service rep
I want a unified customer view
So that I can see all customer information in one place
```

**Break into stories:**
```
Story 1: Customer profile
As a customer service rep
I want to see customer contact information
So that I can verify who I'm talking to

Story 2: Order history
As a customer service rep
I want to see customer's recent orders
So that I can understand their purchase history

Story 3: Support tickets
As a customer service rep
I want to see customer's support tickets
So that I can understand their issues

Story 4: Search
As a customer service rep
I want to search by name or email
So that I can find customers quickly
```

### Adding Detail

**Initial story:**
```
As a user
I want to search
So that I can find things
```

**Refined story:**
```
As a customer service rep
I want to search customers by name, email, or phone
So that I can find the right customer record in under 30 seconds

Acceptance Criteria:
- Search returns results in <2 seconds
- Search matches partial names
- Results show name, email, and last contact date
- Can click result to view full profile
```

## Story Collaboration

### Three C's

**Card:**
- Written story (placeholder for conversation)
- Brief, not detailed specification

**Conversation:**
- Discussion with team
- Clarify details
- Explore options
- Answer questions

**Confirmation:**
- Acceptance criteria
- How to verify
- Definition of done

### Story Workshop

**Process:**
```
1. Product owner presents story
2. Team asks questions
3. Discuss implementation options
4. Clarify acceptance criteria
5. Estimate effort
6. Confirm understanding
```

**Example:**
```
PO: "Here's the search story..."
Dev: "Should search be case-sensitive?"
PO: "No, case-insensitive"
Dev: "Should it search across all fields or just name?"
PO: "Name, email, and phone for now"
QA: "How should we test performance?"
PO: "Load test with 10,000 records"
```

## Story Patterns

### CRUD Stories

**Create:**
```
As a [user]
I want to create [entity]
So that [benefit]
```

**Read:**
```
As a [user]
I want to view [entity]
So that [benefit]
```

**Update:**
```
As a [user]
I want to edit [entity]
So that [benefit]
```

**Delete:**
```
As a [user]
I want to delete [entity]
So that [benefit]
```

### Workflow Stories

**Start:**
```
As a [user]
I want to start [process]
So that [benefit]
```

**Progress:**
```
As a [user]
I want to [action] in [process]
So that [benefit]
```

**Complete:**
```
As a [user]
I want to complete [process]
So that [benefit]
```

## Common Story Mistakes

### 1. Too Vague

**Bad:**
```
As a user
I want a better experience
So that I'm happier
```

**Good:**
```
As a customer
I want to filter products by price range
So that I can find items within my budget
```

### 2. Technical Focus

**Bad:**
```
As a developer
I want to refactor the database
So that queries are faster
```

**Good:**
```
As a customer
I want search results in under 1 second
So that I don't wait while shopping
```

### 3. No Clear Benefit

**Bad:**
```
As a user
I want a dashboard
So that I have a dashboard
```

**Good:**
```
As a manager
I want a dashboard showing team performance
So that I can identify who needs support
```

### 4. Too Large

**Bad:**
```
As a user
I want a complete e-commerce platform
So that I can sell products online
```

**Good:**
Break into many smaller stories

### 5. Solution in Story

**Bad:**
```
As a user
I want a dropdown menu with 5 options
So that I can select my preference
```

**Good:**
```
As a user
I want to select my preference
So that the system customizes my experience
```

## Story Estimation

### Story Points

**Relative sizing:**
- Compare to known stories
- Consider complexity, effort, uncertainty
- Use Fibonacci (1, 2, 3, 5, 8, 13)

**Example:**
```
Simple search: 2 points
Advanced filters: 5 points
Export to CSV: 3 points
Real-time updates: 8 points
```

### T-Shirt Sizing

**Quick estimation:**
- XS: Trivial (< 1 day)
- S: Small (1-2 days)
- M: Medium (3-5 days)
- L: Large (1-2 weeks)
- XL: Epic (break it down)

## Senior-Level User Stories

1. **Strategic stories**
   - Not just feature stories
   - Business outcome stories
   - Strategic initiative stories

2. **Story quality**
   - Establish story standards
   - Train teams in good stories
   - Review and improve

3. **Story management**
   - Build story processes
   - Manage story backlog
   - Ensure story readiness

4. **Advanced patterns**
   - Complex story types
   - Cross-team stories
   - Integration stories

## Metrics

- Story quality score (INVEST compliance)
- Story completion rate (done vs. planned)
- Story cycle time (start to done)
- Story rework rate (reopened stories)
- Story clarity (team questions per story)

## Resources

- User Stories Applied by Mike Cohn
- User Story Mapping by Jeff Patton
- [[body-of-knowledge/BABOK/05_Requirements_Analysis_and_Design]] - Requirements

## Checklist

Before writing story:
- [ ] User type identified
- [ ] Goal clear
- [ ] Benefit understood
- [ ] Problem defined

After writing story:
- [ ] INVEST criteria met
- [ ] Acceptance criteria defined
- [ ] Story estimated
- [ ] Dependencies identified
- [ ] Reviewed with team
- [ ] Ready for development

