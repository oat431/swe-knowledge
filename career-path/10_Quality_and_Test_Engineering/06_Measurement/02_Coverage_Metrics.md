---
title: Coverage Metrics
parent: Measurement
topic: How much are we testing?
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - coverage-metrics
  - test-coverage
---

# Coverage Metrics

> **Core Principle:** Coverage metrics measure how much code is exercised by tests. High coverage does not guarantee quality, but low coverage indicates testing gaps.

## What Coverage Metrics Measure

Coverage metrics track:
- **Code coverage:** How much code is executed by tests
- **Branch coverage:** How many decision paths are tested
- **Function coverage:** How many functions are called
- **Requirements coverage:** How many requirements have tests
- **Risk coverage:** How many high-risk areas are tested

## Types of Code Coverage

### 1. Statement Coverage

**Definition:** Percentage of executable statements executed by tests

**Formula:**
```
Statement Coverage = (Statements Executed / Total Statements) × 100%
```

**Example:**
```python
def calculate_discount(amount, customer_type):
    if customer_type == "premium":     # Line 1
        discount = amount * 0.1        # Line 2
    elif customer_type == "regular":   # Line 3
        discount = amount * 0.05       # Line 4
    else:                              # Line 5
        discount = 0                   # Line 6
    return discount                    # Line 7

# Test 1: calculate_discount(100, "premium")
# Executes lines: 1, 2, 7
# Statement coverage: 3/7 = 43%

# Test 2: calculate_discount(100, "regular")
# Executes lines: 1, 3, 4, 7
# Combined coverage: 5/7 = 71%

# Test 3: calculate_discount(100, "new")
# Executes lines: 1, 3, 5, 6, 7
# Combined coverage: 7/7 = 100%
```

**Limitations:**
- Does not verify all paths
- Can miss edge cases
- 100% statement coverage does not mean bug-free

### 2. Branch Coverage

**Definition:** Percentage of decision branches executed

**Formula:**
```
Branch Coverage = (Branches Executed / Total Branches) × 100%
```

**Example:**
```python
def check_age(age):
    if age >= 18:        # Branch 1: True
        return "adult"   # Branch 2
    else:                # Branch 3: False
        return "minor"   # Branch 4

# Test: check_age(25)
# Executes branches: 1 (True), 2
# Branch coverage: 2/4 = 50%

# Test: check_age(15)
# Executes branches: 1 (False), 3, 4
# Combined coverage: 4/4 = 100%
```

**Why branch coverage is better:**
- Tests all decision paths
- Catches more edge cases
- More thorough than statement coverage

### 3. Condition Coverage

**Definition:** Percentage of boolean sub-expressions evaluated to both true and false

**Example:**
```python
def can_access(user, resource):
    if user.is_admin and resource.is_public:
        return True
    return False

# Conditions: user.is_admin, resource.is_public
# Need tests where each is True and False

# Test 1: admin=True, public=True → both True
# Test 2: admin=False, public=False → both False
# Condition coverage: 100%
```

### 4. Path Coverage

**Definition:** Percentage of execution paths through the code

**Example:**
```python
def process(order):
    if order.valid:
        if order.paid:
            ship(order)
        else:
            request_payment(order)
    else:
        reject(order)

# Paths:
# 1. valid=True, paid=True → ship
# 2. valid=True, paid=False → request_payment
# 3. valid=False → reject

# Need 3 tests for 100% path coverage
```

**Challenge:** Path coverage grows exponentially with branches

## Coverage Tools

### Python

**Coverage.py:**
```bash
# Install
pip install coverage

# Run tests with coverage
coverage run -m pytest tests/

# Generate report
coverage report -m

# Generate HTML report
coverage html
```

**pytest-cov:**
```bash
# Install
pip install pytest-cov

# Run with coverage
pytest --cov=src --cov-report=html tests/

# With branch coverage
pytest --cov=src --cov-branch --cov-report=html tests/
```

### JavaScript

**Istanbul/nyc:**
```bash
# Install
npm install --save-dev nyc

# Run tests with coverage
nyc npm test

# Generate report
nyc report --reporter=html
```

**Jest:**
```bash
# Built-in coverage
jest --coverage

# Configuration in package.json
{
  "jest": {
    "collectCoverage": true,
    "coverageThreshold": {
      "global": {
        "branches": 80,
        "functions": 80,
        "lines": 80,
        "statements": 80
      }
    }
  }
}
```

### Java

**JaCoCo:**
```xml
<!-- Maven plugin -->
<plugin>
  <groupId>org.jacoco</groupId>
  <artifactId>jacoco-maven-plugin</artifactId>
  <version>0.8.8</version>
  <executions>
    <execution>
      <goals>
        <goal>prepare-agent</goal>
      </goals>
    </execution>
    <execution>
      <id>report</id>
      <phase>test</phase>
      <goals>
        <goal>report</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

## Coverage Thresholds

### Industry Benchmarks

| Coverage Level | Assessment |
|----------------|------------|
| > 90% | Excellent |
| 80% to 90% | Good |
| 60% to 80% | Acceptable |
| 40% to 60% | Needs improvement |
| < 40% | Poor |

### Setting Targets

**Consider:**
- Project criticality (medical, financial = higher targets)
- Code complexity (complex code = higher targets)
- Team maturity (experienced teams = higher targets)
- Legacy code (gradually improve, don't demand 80% immediately)

**Example targets:**
```
New code: > 80% statement coverage, > 70% branch coverage
Critical modules: > 90% statement coverage, > 80% branch coverage
Legacy code: Improve by 5% per quarter
Overall project: > 70% statement coverage
```

## Coverage Limitations

### What Coverage Does NOT Tell You

**1. Test quality:**
```python
# 100% coverage but poor test
def test_add():
    result = add(2, 3)
    # No assertion! Test always passes
```

**2. Edge cases:**
```python
# 100% coverage but missing edge case
def divide(a, b):
    return a / b

def test_divide():
    assert divide(10, 2) == 5
    # No test for divide(10, 0) - division by zero!
```

**3. Logic errors:**
```python
# 100% coverage but wrong logic
def calculate_tax(amount):
    return amount * 0.2  # Should be 0.1

def test_calculate_tax():
    assert calculate_tax(100) == 20  # Wrong expected value!
```

### Coverage Pitfalls

**1. Chasing 100%:**
- Last 10% often costs more than first 90%
- May test trivial code (getters, setters)
- Diminishing returns

**2. Gaming coverage:**
```python
# Bad: Adding tests just for coverage
def test_everything():
    import_module()  # Just imports, no assertions
    call_function()  # No verification
```

**3. Ignoring uncovered code:**
- Uncovered code may be untestable
- May indicate design problems
- May be dead code

## Beyond Code Coverage

### Requirements Coverage

**Definition:** Percentage of requirements with corresponding tests

**Example:**
```
Requirements: 50
With tests: 45
Requirements coverage: 90%

Missing tests for:
- REQ-45: Error handling for timeout
- REQ-46: Retry logic
- REQ-47: Audit logging
- REQ-48: Rate limiting
- REQ-49: Data export
```

**Tracking:**
```markdown
| Requirement | Test Case | Status |
|-------------|-----------|--------|
| REQ-001 | TC-001 | ✓ Pass |
| REQ-002 | TC-002 | ✓ Pass |
| REQ-003 | TC-003 | ✗ Fail |
| REQ-004 | None | Missing |
```

### Risk Coverage

**Definition:** Percentage of high-risk areas with tests

**Risk assessment:**
```
High risk (must test):
- Payment processing ✓
- Authentication ✓
- Data encryption ✓
- User permissions ✓

Medium risk (should test):
- Search functionality ✓
- Email notifications ✓
- Report generation ✗ (gap)

Low risk (nice to test):
- UI animations ✗
- Help text ✗
```

### Mutation Testing

**Definition:** Measures test quality by introducing small bugs (mutations) and checking if tests catch them

**Tools:**
- **mutmut** (Python)
- **Stryker** (JavaScript, Java, C#)

**Example:**
```python
# Original code
def is_adult(age):
    return age >= 18

# Mutation 1: Change >= to >
def is_adult(age):
    return age > 18  # Should fail test for age=18

# Mutation 2: Change 18 to 17
def is_adult(age):
    return age >= 17  # Should fail test for age=17

# If tests catch mutations: Good test quality
# If tests don't catch mutations: Tests need improvement
```

**Mutation score:**
```
Mutations created: 100
Mutations killed (tests caught them): 85
Mutation score: 85%

Target: > 80%
```

## Coverage Dashboard

```
Coverage Dashboard
═══════════════════════════════════════

Overall Coverage:
  Statement: 82% ✓ (target: 80%)
  Branch: 74% ✓ (target: 70%)
  Function: 88% ✓ (target: 85%)

By Module:
  src/auth/     95% ✓ (critical)
  src/payment/  92% ✓ (critical)
  src/api/      78% ⚠ (needs improvement)
  src/utils/    65% ⚠ (low priority)
  src/legacy/   45% ⚠ (improving)

Trends:
  [Graph: Coverage over last 6 months - increasing]

Mutation Testing:
  Mutation score: 82% ✓ (target: 80%)

Requirements Coverage:
  45/50 requirements have tests (90%) ✓
```

## Best Practices

### 1. Set Realistic Targets

**Consider context:**
- Critical code: Higher targets
- New code: Higher targets
- Legacy code: Gradual improvement
- Utility code: Lower targets acceptable

### 2. Focus on Quality, Not Quantity

**Write meaningful tests:**
- Clear assertions
- Test edge cases
- Test error conditions
- Test business logic

### 3. Use Multiple Coverage Types

**Combine metrics:**
- Statement coverage (breadth)
- Branch coverage (depth)
- Mutation testing (quality)
- Requirements coverage (completeness)

### 4. Track Trends

**Monitor over time:**
- Is coverage improving?
- Are new features well-tested?
- Are gaps being filled?
- Is test quality improving?

### 5. Make Coverage Visible

**Integrate into workflow:**
- Show coverage in PR checks
- Include in code reviews
- Track in dashboards
- Celebrate improvements

## Key Takeaways

1. **Coverage is necessary but not sufficient:** High coverage does not guarantee quality
2. **Use multiple coverage types:** Statement, branch, mutation, requirements
3. **Focus on test quality:** Meaningful assertions, edge cases, error conditions
4. **Set realistic targets:** Based on criticality, not arbitrary numbers
5. **Track trends:** Improvement over time matters more than absolute numbers

## Related Topics

- [[01_Defect_Metrics]]: What defects tell us
- [[03_Process_Metrics]]: Process efficiency
- [[06_Measurement_Pitfalls]]: Coverage pitfalls

## Existing Vault Connections

- [[software-engineering-note/05_Software_Testing/15_Test_Metrics]]: Test coverage metrics
