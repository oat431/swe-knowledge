---
title: Equivalence Partitioning
parent: Test Design
topic: Reducing test cases through equivalence classes
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - test-design
  - equivalence-partitioning
---

# Equivalence Partitioning

> **Core Principle:** Divide input data into partitions where all values in a partition are expected to behave the same way, then test one value from each partition.

## What Equivalence Partitioning Is

Equivalence partitioning is a black-box test design technique that:
- Divides input data into groups (partitions)
- Assumes all values in a partition are processed similarly
- Tests one representative value from each partition
- Reduces test cases while maintaining coverage

## Why Equivalence Partitioning Matters

Testing all possible input values is usually impossible:

| Input | Possible Values | Test Cases Needed |
|-------|----------------|-------------------|
| Age (1-120) | 120 values | 120 tests |
| Username (1-50 chars) | Infinite combinations | Impossible |
| Email address | Infinite combinations | Impossible |

Equivalence partitioning reduces this to a manageable number:

| Input | Partitions | Test Cases Needed |
|-------|-----------|-------------------|
| Age | 3 (invalid low, valid, invalid high) | 3 tests |
| Username | 3 (too short, valid, too long) | 3 tests |
| Email | 2 (valid format, invalid format) | 2 tests |

## The Technique

### Step 1: Identify Input Conditions

List all inputs to the function or feature:
- User inputs (text fields, selections)
- System inputs (file data, API parameters)
- Environmental inputs (time, configuration)

### Step 2: Define Equivalence Partitions

For each input, identify:

**Valid partitions:**
- Values that should be accepted and processed correctly

**Invalid partitions:**
- Values that should be rejected with appropriate error messages

### Step 3: Select Test Values

Choose one representative value from each partition:
- For valid partitions: typical values
- For invalid partitions: clear violations

### Step 4: Create Test Cases

Combine partitions from multiple inputs to create test cases.

## Examples

### Example 1: Age Field (18-65)

**Input:** Age (integer)
**Valid range:** 18 to 65

**Partitions:**

| Partition | Type | Range | Representative Value |
|-----------|------|-------|---------------------|
| P1 | Invalid | < 18 | 10 |
| P2 | Valid | 18-65 | 30 |
| P3 | Invalid | > 65 | 70 |

**Test Cases:**

| Test ID | Input | Expected Result | Partition |
|---------|-------|-----------------|-----------|
| TC1 | 10 | Rejected: "Age must be 18 or older" | P1 |
| TC2 | 30 | Accepted and processed | P2 |
| TC3 | 70 | Rejected: "Age must be 65 or younger" | P3 |

### Example 2: Username Field

**Input:** Username (3-20 alphanumeric characters)

**Partitions:**

| Partition | Type | Condition | Representative Value |
|-----------|------|-----------|---------------------|
| P1 | Invalid | Length < 3 | "ab" |
| P2 | Valid | Length 3-20, alphanumeric | "user123" |
| P3 | Invalid | Length > 20 | "thisusernameiswaytoolong" |
| P4 | Invalid | Contains special chars | "user@123" |
| P5 | Invalid | Contains spaces | "user 123" |
| P6 | Invalid | Empty | "" |

**Test Cases:**

| Test ID | Input | Expected Result | Partition |
|---------|-------|-----------------|-----------|
| TC1 | "ab" | Rejected: "Username must be at least 3 characters" | P1 |
| TC2 | "user123" | Accepted | P2 |
| TC3 | "thisusernameiswaytoolong" | Rejected: "Username must be 20 characters or less" | P3 |
| TC4 | "user@123" | Rejected: "Username must be alphanumeric" | P4 |
| TC5 | "user 123" | Rejected: "Username cannot contain spaces" | P5 |
| TC6 | "" | Rejected: "Username is required" | P6 |

### Example 3: Discount Calculation

**Input:** Purchase amount and customer type
**Business rules:**
- Regular customers: 5% discount on purchases over $100
- Premium customers: 10% discount on all purchases
- No discount for purchases under $50

**Partitions:**

| Partition | Customer Type | Amount | Expected Discount |
|-----------|--------------|--------|-------------------|
| P1 | Regular | < $50 | 0% |
| P2 | Regular | $50-$100 | 0% |
| P3 | Regular | > $100 | 5% |
| P4 | Premium | < $50 | 10% |
| P5 | Premium | $50-$100 | 10% |
| P6 | Premium | > $100 | 10% |

**Test Cases:**

| Test ID | Customer | Amount | Expected Discount | Partition |
|---------|----------|--------|-------------------|-----------|
| TC1 | Regular | $30 | 0% | P1 |
| TC2 | Regular | $75 | 0% | P2 |
| TC3 | Regular | $150 | 5% ($7.50) | P3 |
| TC4 | Premium | $30 | 10% ($3.00) | P4 |
| TC5 | Premium | $75 | 10% ($7.50) | P5 |
| TC6 | Premium | $150 | 10% ($15.00) | P6 |

## Advanced Techniques

### Combining Partitions

When multiple inputs exist, you can:

**1. Test all combinations (exhaustive):**
- Number of tests = Product of partition counts
- Example: 3 partitions × 4 partitions = 12 tests
- Use when: Small number of partitions, high risk

**2. Test each partition independently:**
- Number of tests = Sum of partition counts
- Example: 3 partitions + 4 partitions = 7 tests
- Use when: Inputs are independent, low risk

**3. Pairwise testing:**
- Test all pairs of partitions
- Reduces tests while maintaining good coverage
- Use when: Many inputs, moderate risk

### Output Partitions

Don't forget to partition outputs:

**Example:** Tax calculation
- Input: Income (partitioned)
- Output partitions:
  - No tax (income < threshold)
  - Low tax rate (income in low bracket)
  - High tax rate (income in high bracket)

Test at least one input that produces each output partition.

## Common Mistakes

| Mistake | Problem | Solution |
|---------|---------|----------|
| **Missing invalid partitions** | Only test valid inputs | Always identify invalid partitions |
| **Too many partitions** | Over-partitioning creates redundant tests | Group similar values |
| **Too few partitions** | Under-partitioning misses defects | Consider edge cases and error conditions |
| **Ignoring output partitions** | Only partition inputs | Partition outputs too |
| **Not documenting partitions** | Can't explain test coverage | Document partitions and rationale |

## When to Use Equivalence Partitioning

**Use when:**
- Input has clear valid/invalid ranges
- Many possible input values
- Need to reduce test cases
- Testing data validation
- Black-box testing (no code access)

**Combine with:**
- Boundary value analysis (test partition boundaries)
- Decision tables (when multiple conditions interact)
- State transition testing (when input depends on state)

## Practical Tips

1. **Start with obvious partitions:** Valid vs invalid is always a good start
2. **Consider data types:** Strings, numbers, dates have different partition strategies
3. **Think about errors:** What invalid inputs should trigger errors?
4. **Don't forget edge cases:** Null, empty, maximum length, etc.
5. **Document your partitions:** Makes test design reviewable and maintainable
6. **Review with developers:** They may know about internal partitions you missed

## Key Takeaways

1. **Equivalence partitioning reduces test cases** by grouping similar inputs
2. **Identify both valid and invalid partitions** to test acceptance and rejection
3. **One test per partition** is usually sufficient (unless high risk)
4. **Combine with boundary value analysis** for more thorough coverage
5. **Document partitions** to explain test coverage to stakeholders

## Related Topics

- [[02_Boundary_Value_Analysis]]: Test the edges of partitions
- [[03_Decision_Table_Testing]]: When partitions interact
- [[07_Test_Design_Strategy]]: Combining techniques effectively

## Existing Vault Connections

- [[software-engineering-note/05_Software_Testing/02_Boundary_and_Equivalence]]: Equivalence partitioning fundamentals
