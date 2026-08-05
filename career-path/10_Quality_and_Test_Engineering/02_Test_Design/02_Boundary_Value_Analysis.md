---
title: Boundary Value Analysis
parent: Test Design
topic: Testing at the edges of input ranges
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - test-design
  - boundary-value-analysis
---

# Boundary Value Analysis

> **Core Principle:** Defects cluster at boundaries. Test at, just below, and just above boundary values.

## What Boundary Value Analysis Is

Boundary value analysis (BVA) is a test design technique that:
- Focuses on values at the edges of equivalence partitions
- Tests boundary values and values just inside/outside boundaries
- Catches off-by-one errors and boundary condition defects
- Complements equivalence partitioning

## Why Boundary Value Analysis Matters

Defects cluster at boundaries because:
- Developers use `<` instead of `<=` (or vice versa)
- Array indexing errors (0-based vs 1-based)
- Rounding errors at precision boundaries
- Loop termination conditions
- Comparison operator mistakes

**Example:** Age validation (18-65)

```python
# Bug: uses < instead of <=
if age < 18 or age > 65:  # Should be: age < 18 or age > 65
    return "Invalid age"
```

This bug is only caught by testing the boundary values 18 and 65.

## The Technique

### Basic Boundary Value Analysis

For a range [min, max], test:
- **min - 1** (just below lower boundary)
- **min** (lower boundary)
- **min + 1** (just above lower boundary)
- **max - 1** (just below upper boundary)
- **max** (upper boundary)
- **max + 1** (just above upper boundary)

### Robust Boundary Value Analysis

Adds extreme values:
- **min - 2** (further below)
- **max + 2** (further above)

## Examples

### Example 1: Age Field (18-65)

**Range:** 18 to 65 (inclusive)

**Boundary Values:**

| Value | Position | Expected Result | Rationale |
|-------|----------|-----------------|-----------|
| 17 | Below lower boundary | Rejected | Just outside valid range |
| 18 | Lower boundary | Accepted | Minimum valid value |
| 19 | Above lower boundary | Accepted | Just inside valid range |
| 64 | Below upper boundary | Accepted | Just inside valid range |
| 65 | Upper boundary | Accepted | Maximum valid value |
| 66 | Above upper boundary | Rejected | Just outside valid range |

**Test Cases:**

| Test ID | Input | Expected Result | Boundary |
|---------|-------|-----------------|----------|
| TC1 | 17 | Rejected | Below lower |
| TC2 | 18 | Accepted | Lower boundary |
| TC3 | 19 | Accepted | Above lower |
| TC4 | 64 | Accepted | Below upper |
| TC5 | 65 | Accepted | Upper boundary |
| TC6 | 66 | Rejected | Above upper |

### Example 2: Password Length (8-20 characters)

**Range:** 8 to 20 characters

**Boundary Values:**

| Length | Position | Expected Result |
|--------|----------|-----------------|
| 7 | Below lower | Rejected: "Password must be at least 8 characters" |
| 8 | Lower boundary | Accepted |
| 9 | Above lower | Accepted |
| 19 | Below upper | Accepted |
| 20 | Upper boundary | Accepted |
| 21 | Above upper | Rejected: "Password must be 20 characters or less" |

### Example 3: Date Range (2020-01-01 to 2025-12-31)

**Range:** January 1, 2020 to December 31, 2025

**Boundary Values:**

| Date | Position | Expected Result |
|------|----------|-----------------|
| 2019-12-31 | Below lower | Rejected |
| 2020-01-01 | Lower boundary | Accepted |
| 2020-01-02 | Above lower | Accepted |
| 2025-12-30 | Below upper | Accepted |
| 2025-12-31 | Upper boundary | Accepted |
| 2026-01-01 | Above upper | Rejected |

### Example 4: Array Index (0 to n-1)

**Array size:** 10 elements (indices 0-9)

**Boundary Values:**

| Index | Position | Expected Result |
|-------|----------|-----------------|
| -1 | Below lower | Error: Index out of bounds |
| 0 | Lower boundary | Access first element |
| 1 | Above lower | Access second element |
| 8 | Below upper | Access ninth element |
| 9 | Upper boundary | Access last element |
| 10 | Above upper | Error: Index out of bounds |

## Advanced Boundary Analysis

### Multiple Boundaries

When multiple inputs have boundaries, test combinations:

**Example:** Rectangle dimensions (width: 1-100, height: 1-50)

**Strategy:** Test boundary combinations strategically

| Test ID | Width | Height | Rationale |
|---------|-------|--------|-----------|
| TC1 | 1 | 1 | Both at lower boundary |
| TC2 | 100 | 50 | Both at upper boundary |
| TC3 | 1 | 50 | Width low, height high |
| TC4 | 100 | 1 | Width high, height low |
| TC5 | 50 | 25 | Both in middle (control) |
| TC6 | 0 | 25 | Width below boundary |
| TC7 | 50 | 0 | Height below boundary |
| TC8 | 101 | 25 | Width above boundary |
| TC9 | 50 | 51 | Height above boundary |

### Non-Numeric Boundaries

Boundaries apply to more than numbers:

**String length:**
- Minimum length
- Maximum length
- Empty string

**Dates:**
- Leap years
- Month boundaries (28/29/30/31 days)
- Year boundaries
- Daylight saving time transitions

**Enumerations:**
- First value
- Last value
- Values not in enumeration

**File sizes:**
- 0 bytes (empty)
- 1 byte (minimum)
- Maximum allowed size
- Just above maximum

### Boundary Analysis for Collections

**List/Set size:**

| Size | Position | Expected Result |
|------|----------|-----------------|
| 0 | Lower boundary | Empty list handled |
| 1 | Above lower | Single item processed |
| max | Upper boundary | Maximum items handled |
| max + 1 | Above upper | Rejected or error |

**Example:** Shopping cart (max 50 items)

| Items | Position | Expected Result |
|-------|----------|-----------------|
| 0 | Lower | "Cart is empty" message |
| 1 | Above lower | Single item displayed |
| 50 | Upper | All 50 items displayed |
| 51 | Above upper | "Cart limit reached" error |

## Common Boundary Types

### Numeric Boundaries

- Integer ranges (1-100)
- Floating-point precision (0.00-99.99)
- Negative numbers (-100 to -1)
- Zero as boundary

### String Boundaries

- Length (min-max characters)
- Empty string vs non-empty
- Whitespace-only strings
- Unicode character limits

### Date/Time Boundaries

- Start/end of day (00:00:00, 23:59:59)
- Month boundaries (28/29/30/31 days)
- Year boundaries (leap years)
- Timezone transitions

### Collection Boundaries

- Empty collection
- Single item
- Maximum capacity
- Overflow

### State Boundaries

- Initial state
- Final state
- State transitions
- Concurrent state changes

## Boundary Analysis Checklist

For each input, check:

- [ ] Minimum value (lower boundary)
- [ ] Value just below minimum
- [ ] Value just above minimum
- [ ] Maximum value (upper boundary)
- [ ] Value just below maximum
- [ ] Value just above maximum
- [ ] Zero (if applicable)
- [ ] Negative values (if applicable)
- [ ] Empty/null (if applicable)
- [ ] Extreme values (very large, very small)

## Common Defects Found by BVA

| Defect Type | Example | How BVA Catches It |
|-------------|---------|-------------------|
| **Off-by-one errors** | Loop runs n times instead of n-1 | Test at boundary n-1, n, n+1 |
| **Wrong comparison operator** | Uses `<` instead of `<=` | Test at exact boundary value |
| **Array index errors** | Access index[n] instead of index[n-1] | Test at last valid index |
| **Rounding errors** | 0.999 rounds to 0 instead of 1 | Test at precision boundaries |
| **Overflow/underflow** | Integer overflow at max value | Test at max and max+1 |
| **Empty collection handling** | Null pointer on empty list | Test with empty collection |

## Practical Tips

1. **Combine with equivalence partitioning:** First partition, then test boundaries
2. **Think about implicit boundaries:** Not all boundaries are documented
3. **Consider data types:** Integer vs float boundaries differ
4. **Test both sides:** Inside and outside the boundary
5. **Don't forget zero:** Zero is often a boundary
6. **Check documentation:** Requirements may specify boundaries
7. **Ask developers:** They know about internal boundaries

## When to Use Boundary Value Analysis

**Use when:**
- Input has numeric ranges
- Input has size limits
- Collections have capacity limits
- Date/time ranges exist
- Off-by-one errors are likely
- High-risk calculations

**Combine with:**
- Equivalence partitioning (partition first, then boundaries)
- Decision tables (when boundaries interact)
- Error guessing (based on experience)

## Key Takeaways

1. **Defects cluster at boundaries:** Test at, just inside, and just outside boundaries
2. **BVA complements equivalence partitioning:** Partition first, then test boundaries
3. **Test both sides:** Values inside and outside the valid range
4. **Consider all boundary types:** Numeric, string, date, collection, state
5. **BVA catches common defects:** Off-by-one errors, wrong operators, index errors

## Related Topics

- [[01_Equivalence_Partitioning]]: Partition before testing boundaries
- [[03_Decision_Table_Testing]]: When multiple boundaries interact
- [[07_Test_Design_Strategy]]: Combining BVA with other techniques

## Existing Vault Connections

- [[software-engineering-note/05_Software_Testing/02_Boundary_and_Equivalence]]: Boundary value analysis fundamentals
