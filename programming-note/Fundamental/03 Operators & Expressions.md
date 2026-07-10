---
tags:
  - programming
  - fundamental
  - operators
---

# 03 Operators & Expressions

Operators are the building blocks of expressions — the instructions that compute values, compare conditions, and control program logic. Misunderstanding operator semantics (especially precedence and short-circuiting) is a top source of subtle bugs.

---

## Arithmetic Operators

| Operator | Meaning | Java Example | Result |
|---|---|---|---|
| `+` | Addition | `5 + 3` | `8` |
| `-` | Subtraction | `5 - 3` | `2` |
| `*` | Multiplication | `5 * 3` | `15` |
| `/` | Division | `7 / 2` | `3` (integer division!) |
| `%` | Modulus (remainder) | `7 % 2` | `1` |

```java
// Integer division truncates — a common source of bugs
int result = 7 / 2;           // 3, not 3.5
double precise = 7.0 / 2;     // 3.5
```

```python
# Python 3 always does true division
7 / 2    # 3.5
7 // 2   # 3 (floor division)
```

```typescript
// JavaScript / TypeScript — all numbers are floating-point
7 / 2    // 3.5
Math.floor(7 / 2)  // 3
```

---

## Comparison Operators

| Operator | Meaning | Java | Python |
|---|---|---|---|
| `==` | Equal (value) | `a == b` | `a == b` |
| `!=` | Not equal | `a != b` | `a != b` |
| `<` | Less than | `a < b` | `a < b` |
| `>` | Greater than | `a > b` | `a > b` |
| `<=` | Less than or equal | `a <= b` | `a <= b` |
| `>=` | Greater than or equal | `a >= b` | `a >= b` |

---

## Logical Operators

| Operator | Meaning | Behaviour |
|---|---|---|
| `&&` | Logical AND | Returns `true` if **both** operands are `true` |
| `\|\|` | Logical OR | Returns `true` if **either** operand is `true` |
| `!` | Logical NOT | Inverts the boolean value |

```java
boolean isValid = age >= 18 && hasLicense;
boolean canEnter = isAdmin || isGuest && hasPass;
boolean isLocked = !isOpen;
```

> Use parentheses to make complex logical expressions readable. The compiler doesn't care; your teammates do.

---

## Bitwise Operators

| Operator | Meaning | Use Case |
|---|---|---|
| `&` | AND | Mask bits, check flags |
| `\|` | OR | Set bits, combine flags |
| `^` | XOR | Toggle bits, simple encryption |
| `~` | NOT | Invert all bits |
| `<<` | Left shift | Multiply by 2ⁿ |
| `>>` | Right shift (sign-preserving) | Divide by 2ⁿ |
| `>>>` | Unsigned right shift (Java) | Zero-fill shift |

### Practical Use: Permission Flags

```java
// Define permission flags using bit positions
final int READ    = 0b001;  // 1
final int WRITE   = 0b010;  // 2
final int EXECUTE = 0b100;  // 4

// Grant permissions
int userPerm = READ | WRITE;       // 0b011 (3)

// Check permissions
boolean canRead  = (userPerm & READ) != 0;    // true
boolean canExec  = (userPerm & EXECUTE) != 0;  // false

// Add permission
userPerm |= EXECUTE;               // 0b111 (7)

// Remove permission
userPerm &= ~WRITE;                // 0b101 (5)
```

```python
# Python — same bitwise logic
READ, WRITE, EXECUTE = 1, 2, 4
perm = READ | WRITE
can_read = bool(perm & READ)       # True
perm |= EXECUTE
perm &= ~WRITE
```

---

## Assignment Operators

| Operator | Meaning | Equivalent To |
|---|---|---|
| `=` | Assign | — |
| `+=` | Add and assign | `a = a + b` |
| `-=` | Subtract and assign | `a = a - b` |
| `*=` | Multiply and assign | `a = a * b` |
| `/=` | Divide and assign | `a = a / b` |
| `%=` | Modulus and assign | `a = a % b` |
| `&=` | Bitwise AND and assign | `a = a & b` |
| `\|=` | Bitwise OR and assign | `a = a \| b` |
| `^=` | Bitwise XOR and assign | `a = a ^ b` |
| `<<=` | Left shift and assign | `a = a << b` |
| `>>=` | Right shift and assign | `a = a >> b` |

---

## Ternary Operator

A compact conditional expression:

```java
// condition ? valueIfTrue : valueIfFalse
String status = score >= 60 ? "Pass" : "Fail";
```

```typescript
// TypeScript
const status = score >= 60 ? "Pass" : "Fail";
```

```python
# Python ternary (conditional expression)
status = "Pass" if score >= 60 else "Fail"
```

> Keep ternaries simple. Nested ternaries are unreadable — use `if/else` instead.

---

## Operator Precedence

Highest to lowest (simplified — see JLS §15 for full table):

| Precedence | Operators | Associativity |
|---|---|---|
| 1 (highest) | `()` (parentheses) | — |
| 2 | `!`, `~`, `++`, `--`, unary `-` | Right to left |
| 3 | `*`, `/`, `%` | Left to right |
| 4 | `+`, `-` | Left to right |
| 5 | `<<`, `>>`, `>>>` | Left to right |
| 6 | `<`, `<=`, `>`, `>=`, `instanceof` | Left to right |
| 7 | `==`, `!=` | Left to right |
| 8 | `&` | Left to right |
| 9 | `^` | Left to right |
| 10 | `\|` | Left to right |
| 11 | `&&` | Left to right |
| 12 | `\|\|` | Left to right |
| 13 | `?:` (ternary) | Right to left |
| 14 (lowest) | `=`, `+=`, `-=`, etc. | Right to left |

> **When in doubt, add parentheses.** Explicit grouping eliminates ambiguity and makes intent clear.

---

## Short-Circuit Evaluation

`&&` and `||` short-circuit: they stop evaluating as soon as the result is determined.

```java
// && — stops at first false
if (obj != null && obj.isValid()) {
    // obj.isValid() is never called if obj is null
}

// || — stops at first true
String name = userInput != null ? userInput : "Default";
// If userInput != null, the right side is never evaluated
```

```python
# Python — same short-circuit behaviour
if obj is not None and obj.is_valid():
    pass

name = user_input or "Default"
```

---

## Common Pitfalls

### `==` vs `.equals()` in Java

```java
// ❌ WRONG — compares references, not content
String a = new String("hello");
String b = new String("hello");
System.out.println(a == b);        // false (different objects)

// ✅ CORRECT — compares actual string content
System.out.println(a.equals(b));   // true
```

### Floating-Point Comparison

```java
// ❌ WRONG — floating point is imprecise
double a = 0.1 + 0.2;
System.out.println(a == 0.3);  // false!

// ✅ CORRECT — compare with epsilon tolerance
double epsilon = 1e-9;
System.out.println(Math.abs(a - 0.3) < epsilon);  // true
```

```python
# Python — use math.isclose()
import math
math.isclose(0.1 + 0.2, 0.3)  # True
```

### Integer Division Truncation

```java
// ❌ WRONG — expecting a percentage
int percentage = (part / total) * 100;  // Always 0 if part < total

// ✅ CORRECT — cast first
double percentage = ((double) part / total) * 100;
```

---

## Sources

- Oracle Java Language Specification §15 — Expressions
- MDN — Expressions and Operators (developer.mozilla.org)
- Python Docs — Expressions (docs.python.org)
