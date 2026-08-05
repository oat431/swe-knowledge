---
title: Decision Table Testing
parent: Test Design
topic: Testing complex business rules with decision tables
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - test-design
  - decision-tables
---

# Decision Table Testing

> **Core Principle:** When multiple conditions interact to produce different outcomes, decision tables make all combinations explicit and testable.

## What Decision Table Testing Is

Decision table testing is a systematic technique for:
- Identifying all combinations of conditions
- Determining the expected action for each combination
- Ensuring complete coverage of business rules
- Finding gaps, contradictions, and redundancies in requirements

## When to Use Decision Tables

**Use when:**
- Multiple conditions affect an outcome
- Conditions interact (not independent)
- Business rules are complex
- Requirements use "if...then...else" logic
- You need to verify completeness

**Do not use when:**
- Single input with simple range (use equivalence partitioning)
- Sequential behavior (use state transition testing)
- Conditions are truly independent (use pairwise testing)

## The Technique

### Step 1: Identify Conditions and Actions

**Conditions:** Input variables or business rules that can be true or false
**Actions:** Outcomes or behaviors that result from condition combinations

### Step 2: Create the Decision Table

A decision table has four quadrants:

```
┌─────────────────┬─────────────────┐
│   Conditions    │    Actions      │
├─────────────────┼─────────────────┤
│ Condition stub  │  Action stub    │
│ (list of        │  (list of       │
│  conditions)    │   actions)      │
├─────────────────┼─────────────────┤
│ Condition entry │  Action entry   │
│ (T/F for each   │  (X for execute,│
│  rule)          │   - for skip)   │
└─────────────────┴─────────────────┘
```

### Step 3: Generate Rules

For n binary conditions, there are 2^n possible combinations:
- 2 conditions = 4 rules
- 3 conditions = 8 rules
- 4 conditions = 16 rules

### Step 4: Determine Actions

For each rule (column), determine which actions should occur.

### Step 5: Simplify (Optional)

Combine rules that produce identical actions when one condition does not matter.

## Examples

### Example 1: Login Validation

**Conditions:**
- C1: Username valid (T/F)
- C2: Password valid (T/F)

**Actions:**
- A1: Grant access
- A2: Show error message
- A3: Lock account (after 3 failures)

**Decision Table:**

| Rule | 1 | 2 | 3 | 4 |
|------|---|---|---|---|
| **C1: Username valid** | T | T | F | F |
| **C2: Password valid** | T | F | T | F |
| **A1: Grant access** | X | - | - | - |
| **A2: Show error** | - | X | X | X |
| **A3: Lock account** | - | - | - | X (if 3rd failure) |

**Test Cases:**

| Test ID | Username | Password | Expected Result | Rule |
|---------|----------|----------|-----------------|------|
| TC1 | valid_user | correct_pass | Access granted | 1 |
| TC2 | valid_user | wrong_pass | Error: "Invalid password" | 2 |
| TC3 | invalid_user | correct_pass | Error: "Invalid username" | 3 |
| TC4 | invalid_user | wrong_pass | Error: "Invalid credentials" | 4 |

### Example 2: Loan Approval

**Conditions:**
- C1: Credit score >= 700 (T/F)
- C2: Income >= $50K (T/F)
- C3: Debt-to-income ratio < 0.4 (T/F)

**Actions:**
- A1: Approve loan
- A2: Require co-signer
- A3: Reject application

**Decision Table (8 rules):**

| Rule | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|------|---|---|---|---|---|---|---|---|
| **C1: Credit >= 700** | T | T | T | T | F | F | F | F |
| **C2: Income >= $50K** | T | T | F | F | T | T | F | F |
| **C3: DTI < 0.4** | T | F | T | F | T | F | T | F |
| **A1: Approve** | X | - | - | - | - | - | - | - |
| **A2: Co-signer** | - | X | X | - | X | - | - | - |
| **A3: Reject** | - | - | - | X | - | X | X | X |

**Simplified Table:**

After analysis, we can combine rules with identical outcomes:

| Rule | 1 | 2 | 3 | 4 | 5 |
|------|---|---|---|---|---|
| **C1: Credit >= 700** | T | T | F | F | F |
| **C2: Income >= $50K** | T | - | T | F | - |
| **C3: DTI < 0.4** | T | F | - | T | F |
| **A1: Approve** | X | - | - | - | - |
| **A2: Co-signer** | - | X | X | - | - |
| **A3: Reject** | - | - | - | X | X |

Note: "-" in conditions means "don't care" (any value produces same outcome).

### Example 3: E-commerce Discount Rules

**Business rules:**
- Premium customers get 10% discount
- Orders over $200 get free shipping
- Premium customers with orders over $200 get 15% discount and free shipping
- No discount for regular customers with orders under $200

**Conditions:**
- C1: Premium customer (T/F)
- C2: Order > $200 (T/F)

**Actions:**
- A1: Apply 10% discount
- A2: Apply 15% discount
- A3: Free shipping
- A4: No discount

**Decision Table:**

| Rule | 1 | 2 | 3 | 4 |
|------|---|---|---|---|
| **C1: Premium** | T | T | F | F |
| **C2: Order > $200** | T | F | T | F |
| **A1: 10% discount** | - | X | - | - |
| **A2: 15% discount** | X | - | - | - |
| **A3: Free shipping** | X | - | X | - |
| **A4: No discount** | - | - | - | X |

**Test Cases:**

| Test ID | Customer | Order Amount | Expected Discount | Expected Shipping | Rule |
|---------|----------|--------------|-------------------|-------------------|------|
| TC1 | Premium | $250 | 15% ($37.50) | Free | 1 |
| TC2 | Premium | $150 | 10% ($15.00) | Standard | 2 |
| TC3 | Regular | $250 | 0% | Free | 3 |
| TC4 | Regular | $150 | 0% | Standard | 4 |

## Extended Entry Tables

When conditions have more than two values, use extended entry tables:

**Example:** Shipping calculation

**Conditions:**
- C1: Destination (Domestic, International, Remote)
- C2: Weight (Light < 5kg, Medium 5-20kg, Heavy > 20kg)

**Actions:**
- A1: Standard shipping ($10, $30, $50)
- A2: Express shipping ($20, $60, $100)

**Extended Decision Table:**

| Rule | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|------|---|---|---|---|---|---|---|---|---|
| **C1: Destination** | Dom | Dom | Dom | Int | Int | Int | Rem | Rem | Rem |
| **C2: Weight** | L | M | H | L | M | H | L | M | H |
| **A1: Standard cost** | $10 | $30 | $50 | $30 | $60 | $100 | $50 | $100 | $150 |

## Benefits of Decision Tables

| Benefit | Description |
|---------|-------------|
| **Completeness** | Ensures all condition combinations are considered |
| **Clarity** | Makes complex logic explicit and reviewable |
| **Gap detection** | Reveals missing rules or undefined behavior |
| **Contradiction detection** | Finds conflicting rules |
| **Redundancy detection** | Identifies duplicate rules that can be simplified |
| **Test coverage** | Provides systematic test case generation |

## Common Issues Found

### Missing Rules

**Symptom:** Some condition combinations have no defined action

**Example:**
```
Rule 5: Credit < 700, Income >= $50K, DTI < 0.4 → ???
```

**Impact:** System behavior undefined for this case

**Solution:** Clarify requirements, add rule

### Contradictory Rules

**Symptom:** Same conditions lead to different actions

**Example:**
```
Rule 3: Premium, Order > $200 → 15% discount
Rule 7: Premium, Order > $200 → 10% discount
```

**Impact:** Inconsistent behavior

**Solution:** Resolve contradiction in requirements

### Redundant Rules

**Symptom:** Multiple rules with same outcome where one condition doesn't matter

**Example:**
```
Rule 2: Premium, Order <= $200, Member > 1 year → 10% discount
Rule 6: Premium, Order <= $200, Member <= 1 year → 10% discount
```

**Can simplify to:**
```
Rule 2: Premium, Order <= $200 → 10% discount (membership doesn't matter)
```

## Practical Guidelines

### Building Decision Tables

1. **Start with conditions:** List all input variables or business rules
2. **Define actions:** List all possible outcomes
3. **Generate all combinations:** Use systematic approach (binary counting)
4. **Determine actions:** For each rule, decide which actions occur
5. **Review with stakeholders:** Verify table matches business intent
6. **Simplify:** Combine rules where possible
7. **Generate test cases:** One test per rule (or per simplified rule)

### Managing Complexity

**Problem:** Too many conditions (e.g., 10 conditions = 1024 rules)

**Solutions:**
- **Decompose:** Break into smaller, related decision tables
- **Prioritize:** Focus on high-risk combinations
- **Use pairwise:** Test pairs of conditions instead of all combinations
- **Simplify early:** Combine rules as you build the table

### Reviewing Decision Tables

**Checklist:**
- [ ] All conditions identified
- [ ] All actions identified
- [ ] All combinations covered (no gaps)
- [ ] No contradictions
- [ ] Redundancies identified and simplified
- [ ] Stakeholders have reviewed and approved
- [ ] Test cases generated for each rule

## Decision Tables vs Other Techniques

| Technique | When to Use | Strengths |
|-----------|-------------|-----------|
| **Decision tables** | Multiple interacting conditions | Completeness, clarity |
| **Equivalence partitioning** | Single input with range | Efficiency, simplicity |
| **Boundary value analysis** | Numeric ranges | Catches off-by-one errors |
| **State transition testing** | Sequential behavior | Tests temporal logic |
| **Pairwise testing** | Many independent conditions | Reduces test count |

## Key Takeaways

1. **Decision tables make complex logic explicit:** All condition combinations are visible
2. **Ensure completeness:** Every combination should have a defined outcome
3. **Simplify when possible:** Combine rules with identical outcomes
4. **Review with stakeholders:** Decision tables are excellent for requirements validation
5. **One test per rule:** Ensures systematic coverage

## Related Topics

- [[01_Equivalence_Partitioning]]: Partition individual conditions first
- [[02_Boundary_Value_Analysis]]: Test boundaries within conditions
- [[04_State_Transition_Testing]]: When decisions depend on state
- [[07_Test_Design_Strategy]]: Combining decision tables with other techniques

## Existing Vault Connections

- [[software-engineering-note/05_Software_Testing/03_Decision_Tables_and_State_Based]]: Decision table testing fundamentals
