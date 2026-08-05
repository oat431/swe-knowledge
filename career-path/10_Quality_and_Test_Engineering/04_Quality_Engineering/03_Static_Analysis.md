---
title: Static Analysis
parent: Quality Engineering
topic: Using tools to find issues automatically
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - static-analysis
---

# Static Analysis

> **Core Principle:** Static analysis tools automatically find bugs, security vulnerabilities, and code smells without executing the code.

## What Static Analysis Is

Static analysis is:
- **Automated code inspection:** Tools analyze code without running it
- **Pattern matching:** Finds known problematic patterns
- **Rule-based:** Checks against coding standards and best practices
- **Fast feedback:** Results in seconds or minutes

## Why Static Analysis Matters

**Benefits:**
- Catches bugs humans miss
- Enforces coding standards consistently
- Finds security vulnerabilities
- Identifies code smells and technical debt
- Fast and repeatable
- Scales to large codebases

**Limitations:**
- False positives (reports issues that aren't real)
- False negatives (misses real issues)
- Cannot find all bugs (need testing too)
- Requires configuration and tuning

## Types of Static Analysis

### 1. Linters

**Purpose:** Enforce coding style and catch common mistakes

**What they find:**
- Style violations (indentation, naming)
- Syntax errors
- Unused variables
- Undefined variables
- Common mistakes

**Tools by language:**
- **Python:** pylint, flake8, black (formatter)
- **JavaScript:** ESLint, Prettier (formatter)
- **Java:** Checkstyle, PMD
- **Go:** golint, gofmt
- **Ruby:** RuboCop

**Example (pylint):**
```python
# Bad code
def calculate_total(items):
    total = 0
    for i in items:
        total = total + i.price
    return total

# Pylint output:
# C0103: Variable name "i" doesn't conform to snake_case naming style
# C0103: Function name "calculate_total" should be more descriptive

# Fixed code
def calculate_total_price(items):
    total = 0
    for item in items:
        total = total + item.price
    return total
```

### 2. Type Checkers

**Purpose:** Verify type correctness

**What they find:**
- Type mismatches
- Missing type annotations
- Incorrect function signatures
- Null/None errors

**Tools:**
- **Python:** mypy, pyright
- **JavaScript:** TypeScript, Flow
- **Java:** Built-in compiler
- **C#:** Built-in compiler

**Example (mypy):**
```python
# Bad code
def greet(name: str) -> str:
    return "Hello, " + name

greet(123)  # Passing int instead of str

# Mypy output:
# error: Argument 1 to "greet" has incompatible type "int"; expected "str"

# Fixed code
greet("Alice")  # Correct type
```

### 3. Security Scanners

**Purpose:** Find security vulnerabilities

**What they find:**
- SQL injection
- Cross-site scripting (XSS)
- Hardcoded credentials
- Insecure dependencies
- Buffer overflows

**Tools:**
- **Python:** bandit, safety
- **JavaScript:** npm audit, Snyk
- **Java:** Find Security Bugs, OWASP Dependency-Check
- **General:** SonarQube, Snyk, Veracode

**Example (bandit):**
```python
# Bad code
import os
password = "secret123"  # Hardcoded password
os.system("ls -la")  # Shell injection risk

# Bandit output:
# B105: Possible hardcoded password: 'secret123'
# B605: Starting a process with a shell, possible injection detected

# Fixed code
import os
import subprocess
from getpass import getpass

password = getpass("Enter password: ")
subprocess.run(["ls", "-la"])  # No shell, safer
```

### 4. Complexity Analyzers

**Purpose:** Measure code complexity

**What they measure:**
- Cyclomatic complexity (number of paths)
- Cognitive complexity (how hard to understand)
- Lines of code
- Nesting depth
- Coupling between components

**Tools:**
- **Python:** radon, mccabe
- **JavaScript:** complexity-report
- **Java:** PMD, Checkstyle
- **General:** SonarQube, CodeClimate

**Example (radon):**
```python
# Complex code (cyclomatic complexity = 8)
def process_order(order):
    if order.status == "pending":
        if order.total > 1000:
            if order.customer.is_premium:
                apply_discount(order)
                if order.payment.method == "credit":
                    process_credit_payment(order)
                elif order.payment.method == "paypal":
                    process_paypal_payment(order)
            else:
                process_regular_payment(order)
        else:
            process_small_order(order)
    else:
        handle_non_pending_order(order)

# Radon output:
# process_order - Cyclomatic complexity: 8 (High)

# Simplified code (cyclomatic complexity = 3)
def process_order(order):
    if order.status != "pending":
        return handle_non_pending_order(order)
    
    if order.total > 1000 and order.customer.is_premium:
        apply_discount(order)
    
    return process_payment(order)
```

### 5. Code Smell Detectors

**Purpose:** Find maintainability issues

**What they find:**
- Duplicated code
- Long functions
- Large classes
- Deep nesting
- God classes (too many responsibilities)
- Feature envy (using other objects too much)

**Tools:**
- **General:** SonarQube, CodeClimate, Codacy

**Example (SonarQube):**
```python
# Code smell: Duplicated code
def calculate_tax(amount):
    return amount * 0.1

def calculate_discount(amount):
    return amount * 0.1  # Same logic, duplicated

# SonarQube output:
# Code smell: Duplicate code block (2 lines)

# Fixed code
def calculate_percentage(amount, percentage):
    return amount * percentage

def calculate_tax(amount):
    return calculate_percentage(amount, 0.1)

def calculate_discount(amount):
    return calculate_percentage(amount, 0.1)
```

## Setting Up Static Analysis

### Step 1: Choose Tools

**Consider:**
- Language and framework
- Team size and skills
- Integration with CI/CD
- Cost (free vs commercial)
- False positive rate

**Recommended stack:**
- **Linter:** Language-specific (ESLint, pylint)
- **Type checker:** mypy, TypeScript
- **Security:** bandit, npm audit
- **Comprehensive:** SonarQube (free community edition)

### Step 2: Configure Tools

**Start with default rules:**
```bash
# Python example
pip install pylint flake8 mypy bandit

# Run with defaults
pylint src/
flake8 src/
mypy src/
bandit -r src/
```

**Customize configuration:**
```python
# .pylintrc
[MESSAGES CONTROL]
disable=C0114,C0115,C0116  # Disable missing docstring warnings

[FORMAT]
max-line-length=100

[DESIGN]
max-args=10
max-locals=20
```

```javascript
// .eslintrc.json
{
  "extends": ["eslint:recommended", "plugin:react/recommended"],
  "rules": {
    "no-console": "warn",
    "no-unused-vars": "error",
    "max-len": ["error", { "code": 100 }]
  }
}
```

### Step 3: Integrate with Workflow

**Local development:**
```bash
# Pre-commit hook
# .git/hooks/pre-commit
#!/bin/bash
pylint src/ || exit 1
flake8 src/ || exit 1
mypy src/ || exit 1
```

**IDE integration:**
- VS Code: Install extensions (Pylint, ESLint, mypy)
- PyCharm: Built-in inspections
- IntelliJ: Built-in inspections

**CI/CD integration:**
```yaml
# GitHub Actions
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install dependencies
        run: pip install pylint flake8 mypy bandit
      - name: Run linters
        run: |
          pylint src/
          flake8 src/
          mypy src/
          bandit -r src/
```

### Step 4: Review and Tune

**Reduce false positives:**
- Disable rules that don't apply
- Add exceptions for special cases
- Configure thresholds

**Example:**
```python
# pylint: disable=too-many-arguments
def complex_function(arg1, arg2, arg3, arg4, arg5, arg6):
    # This function genuinely needs 6 arguments
    pass
```

## Static Analysis in CI/CD

### Quality Gates

**Fail build if:**
- Critical issues found
- Security vulnerabilities detected
- Code coverage below threshold
- New technical debt introduced

**Example (SonarQube):**
```yaml
# sonar-project.properties
sonar.qualitygate.wait=true
sonar.qualitygate.timeout=300

# Quality gate conditions:
# - Coverage on new code >= 80%
# - No new vulnerabilities
# - No new bugs
# - Technical debt on new code <= 30min
```

### Trend Analysis

**Track over time:**
- Number of issues
- Issue severity
- Technical debt
- Code coverage
- Complexity

**Dashboard:**
```
Static Analysis Dashboard
═══════════════════════════════════

Issues:
  Critical: 0 ✓
  Major: 3 (down from 8 last sprint)
  Minor: 15 (down from 22 last sprint)

Security:
  Vulnerabilities: 0 ✓
  Security hotspots: 2 (under review)

Code Quality:
  Coverage: 87% ✓ (target: 80%)
  Duplications: 3.2% (down from 4.5%)
  Technical debt: 2 days (down from 5 days)

Complexity:
  Average: 8.5 (target: < 10) ✓
  Max: 23 (in PaymentService)
```

## Best Practices

### 1. Start Small

**Don't enable all rules at once:**
- Start with critical rules
- Fix existing issues
- Gradually add more rules
- Allow team to adapt

### 2. Automate Everything

**Integrate into workflow:**
- Pre-commit hooks
- IDE plugins
- CI/CD pipeline
- Automated reporting

### 3. Fix Issues Promptly

**Don't let issues accumulate:**
- Fix critical issues immediately
- Schedule time for major issues
- Track minor issues as tech debt
- Review and clean up regularly

### 4. Educate the Team

**Help team understand:**
- Why static analysis matters
- How to interpret results
- How to fix common issues
- How to configure tools

### 5. Review and Update Rules

**Regularly review:**
- Are rules still relevant?
- Are there too many false positives?
- Are we missing important checks?
- Should we add custom rules?

## Common Challenges

| Challenge | Solution |
|-----------|----------|
| **Too many false positives** | Tune configuration, disable irrelevant rules |
| **Slow analysis** | Run incrementally, cache results |
| **Team resistance** | Start small, show value, educate |
| **Inconsistent results** | Use same configuration across environments |
| **Legacy code** | Focus on new code, gradually improve old code |

## Key Takeaways

1. **Static analysis catches bugs automatically:** Fast, consistent, scalable
2. **Use multiple tools:** Linters, type checkers, security scanners
3. **Integrate into workflow:** Pre-commit, IDE, CI/CD
4. **Start small and grow:** Don't overwhelm the team
5. **Review and tune:** Reduce false positives, improve effectiveness

## Related Topics

- [[01_Defect_Prevention]]: Static analysis as prevention
- [[02_Code_Reviews]]: Tools assist human reviews
- [[05_Quality_Metrics]]: Tracking static analysis results

## Existing Vault Connections

- [[software-engineering-note/12_Software_Quality/05_Static_Analysis]]: Static analysis tools and techniques
