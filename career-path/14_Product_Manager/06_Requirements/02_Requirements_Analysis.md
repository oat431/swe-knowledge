---
title: Requirements Analysis
parent: Requirements
summary: Understanding and refining requirements
tags:
  - requirements
  - analysis
  - refinement
  - quality
---

# Requirements Analysis

> Elicitation discovers raw requirements. Analysis refines them into clear, complete, and consistent specifications that teams can build from.

## Why Analysis Matters

**Without analysis:**
- Vague, ambiguous requirements
- Conflicts and gaps
- Misunderstandings
- Rework and delays

**With analysis:**
- Clear, specific requirements
- Resolved conflicts
- Shared understanding
- Efficient delivery

## Analysis Activities

### 1. Organize Requirements

**Structure raw requirements:**

**By type:**
- Business requirements
- User requirements
- Functional requirements
- Non-functional requirements

**By feature:**
- Group related requirements
- Identify feature boundaries
- Map dependencies

**By priority:**
- Must have
- Should have
- Could have
- Won't have (this time)

### 2. Refine Requirements

**Improve clarity and specificity:**

**Vague requirement:**
```
"System should be fast"
```

**Refined requirement:**
```
"Search results must return within 2 seconds for 95% 
of queries with up to 10,000 records"
```

**Refinement techniques:**
- Add specific metrics
- Define boundaries
- Clarify conditions
- Remove ambiguity

### 3. Resolve Conflicts

**Address competing needs:**

**Conflict identification:**
- Stakeholder A wants X
- Stakeholder B wants Y
- X and Y are incompatible

**Resolution approaches:**
- Negotiation and compromise
- Escalation to decision-maker
- Phased delivery (X now, Y later)
- Alternative solutions

**Example:**
```
Conflict:
Sales wants simple pricing (3 tiers)
Finance wants flexible pricing (custom quotes)

Resolution:
Phase 1: 3 standard tiers (simple)
Phase 2: Custom pricing for enterprise (flexible)
```

### 4. Identify Gaps

**Find missing requirements:**

**Gap types:**
- Missing scenarios
- Undefined edge cases
- Unspecified integrations
- Missing non-functional requirements

**Gap discovery:**
```
Requirement: "Users can create projects"

Gaps:
- Can users edit projects?
- Can users delete projects?
- Can users archive projects?
- What happens to project data when deleted?
- Are there project limits?
```

### 5. Ensure Quality

**Check requirements quality:**

**Quality attributes:**
- **Complete:** All necessary information
- **Consistent:** No conflicts
- **Unambiguous:** One interpretation
- **Testable:** Clear how to verify
- **Traceable:** Linked to source and goal
- **Feasible:** Technically possible
- **Prioritized:** Ranked by importance

**Quality checklist:**
```
✓ Is the requirement clear and specific?
✓ Can it be tested/verified?
✓ Is it feasible to implement?
✓ Does it align with business goals?
✓ Are conflicts resolved?
✓ Are dependencies identified?
✓ Is it prioritized?
```

## Analysis Techniques

### 1. Process Modeling

**Visualize workflows:**

**Tools:**
- Flowcharts
- Swim lane diagrams
- State diagrams
- Activity diagrams

**Example:**
```
Order Process:
Customer places order → System validates → 
Payment processed → Order confirmed → 
Inventory checked → Order shipped → 
Customer notified
```

### 2. Use Case Modeling

**Describe system interactions:**

**Components:**
- Actors (who interacts)
- Use cases (what they do)
- Scenarios (how it happens)

**Example:**
```
Use Case: Search Customer
Actor: Customer Service Rep
Precondition: User logged in

Main Flow:
1. Rep enters search criteria
2. System displays results
3. Rep selects customer
4. System shows customer details

Alternative Flows:
2a. No results found
    - System shows "No results"
    - Rep refines search
    
2b. Multiple results
    - System shows list
    - Rep selects correct customer
```

### 3. Data Modeling

**Define data structures:**

**Elements:**
- Entities (what data)
- Attributes (details)
- Relationships (connections)

**Example:**
```
Customer
- Customer ID (primary key)
- Name
- Email
- Phone
- Address
- Created Date

Relationships:
- Customer has many Orders
- Customer has many Support Tickets
- Customer belongs to Account
```

### 4. Impact Analysis

**Understand change effects:**

**Analyze:**
- What changes?
- What's affected?
- What breaks?
- What needs updating?

**Example:**
```
Change: Add customer segmentation

Impact:
- Database: Add segment field
- API: Return segment in responses
- UI: Show segment in customer view
- Reports: Filter by segment
- Analytics: Track by segment
- Training: Teach about segments
```

## Requirements Documentation

### 1. Requirements Specification

**Formal document:**

**Structure:**
```
1. Introduction
   - Purpose
   - Scope
   - Definitions

2. Business Requirements
   - Goals
   - Success metrics

3. User Requirements
   - User types
   - User goals
   - User scenarios

4. Functional Requirements
   - Features
   - System behaviors

5. Non-Functional Requirements
   - Performance
   - Security
   - Usability

6. Constraints
   - Technical
   - Business
   - Regulatory

7. Assumptions and Dependencies
```

### 2. User Stories

**Agile format:**
```
As a [user type]
I want [goal]
So that [benefit]

Acceptance Criteria:
- Given [context]
- When [action]
- Then [outcome]
```

### 3. Use Cases

**Detailed scenarios:**
```
Use Case: [Name]
Actor: [Who]
Precondition: [What must be true]

Main Flow:
1. [Step]
2. [Step]
3. [Step]

Alternative Flows:
[Variations]

Postcondition: [What's true after]
```

## Analysis Best Practices

### 1. Start with Problems

**Don't jump to solutions:**
```
Problem: "Reps spend 10 minutes searching for info"
Not: "Build a search feature"

Understand problem first, then explore solutions
```

### 2. Use Examples

**Make requirements concrete:**
```
Abstract: "System should validate email addresses"
Concrete: "System accepts user@example.com, 
          rejects user@invalid (no domain)"
```

### 3. Prototype Early

**Show, don't just tell:**
- Low-fidelity mockups
- Interactive prototypes
- User flow diagrams

### 4. Review with Stakeholders

**Validate understanding:**
- Walk through requirements
- Get feedback
- Resolve questions
- Confirm agreement

### 5. Iterate

**Refine progressively:**
- Start rough, get specific
- Multiple review cycles
- Incorporate feedback
- Improve over time

## Common Analysis Mistakes

### 1. Analysis Paralysis

**Mistake:** Endless analysis, never enough detail
**Fix:** Good enough analysis, timely decisions

### 2. Solution Bias

**Mistake:** Analyzing solutions, not problems
**Fix:** Focus on what and why before how

### 3. Ignoring Non-Functional

**Mistake:** Only functional requirements
**Fix:** Include performance, security, usability

### 4. Not Resolving Conflicts

**Mistake:** Documenting conflicts, not resolving
**Fix:** Address conflicts explicitly

### 5. Over-Specification

**Mistake:** Too much detail, constrains design
**Fix:** Specify what, not how

## Senior-Level Requirements Analysis

1. **Strategic analysis**
   - Not just feature analysis
   - Business impact analysis
   - Strategic alignment

2. **Complex analysis**
   - Handle ambiguity
   - Navigate conflicts
   - Balance competing needs

3. **Analysis leadership**
   - Establish analysis processes
   - Train teams
   - Build quality culture

4. **Advanced techniques**
   - Domain modeling
   - Business rules
   - Decision modeling

## Metrics

- Requirements quality score
- Requirements stability (changes after analysis)
- Analysis efficiency (time to analyze)
- Conflict resolution rate
- Stakeholder satisfaction with requirements

## Resources

- [[body-of-knowledge/BABOK/05_Requirements_Analysis_and_Design]] - Requirements analysis
- Software Requirements by Karl Wiegers
- User Story Mapping by Jeff Patton

## Checklist

Before analysis:
- [ ] Raw requirements gathered
- [ ] Stakeholders available
- [ ] Analysis techniques selected
- [ ] Tools ready

After analysis:
- [ ] Requirements organized
- [ ] Conflicts resolved
- [ ] Gaps identified and filled
- [ ] Quality checked
- [ ] Documented clearly
- [ ] Reviewed with stakeholders
- [ ] Approved

