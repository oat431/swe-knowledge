---
tags:
  - programming
  - fundamental
  - types
---

# 02 Variables & Data Types

Variables are named containers for data; data types define what kind of data a variable holds and what operations are valid. Choosing the right type prevents bugs, saves memory, and communicates intent to other developers.

---

## Primitive Types (Java)

| Type | Size | Range / Values | Default |
|---|---|---|---|
| `byte` | 8 bits | -128 to 127 | `0` |
| `short` | 16 bits | -32,768 to 32,767 | `0` |
| `int` | 32 bits | -2³¹ to 2³¹ - 1 (~2.1 billion) | `0` |
| `long` | 64 bits | -2⁶³ to 2⁶³ - 1 | `0L` |
| `float` | 32 bits | ~±3.4 × 10³⁸ (6-7 decimal digits) | `0.0f` |
| `double` | 64 bits | ~±1.7 × 10³⁰⁸ (15-16 decimal digits) | `0.0d` |
| `char` | 16 bits | 0 to 65,535 (Unicode) | `'\u0000'` |
| `boolean` | ~1 bit (JVM-dependent) | `true` / `false` | `false` |

TypeScript and Python handle primitives differently — TypeScript's `number` covers both `int` and `float`; Python's `int` has arbitrary precision.

```java
// Java — explicit types
int count = 42;
double price = 19.99;
boolean active = true;
char grade = 'A';
```

```typescript
// TypeScript — type annotations
let count: number = 42;
let price: number = 19.99;
let active: boolean = true;
let grade: string = "A";
```

```python
# Python — dynamic typing (types are inferred)
count = 42          # int
price = 19.99       # float
active = True       # bool
grade = "A"         # str
```

---

## Reference Types vs Value Types

| Aspect | Value Types | Reference Types |
|---|---|---|
| **Stored in** | Stack | Heap (reference on stack) |
| **Contains** | The actual data | A pointer/address to the data |
| **Assignment** | Copies the value | Copies the reference (both point to same object) |
| **Examples (Java)** | `int`, `double`, `boolean`, `char` | `String`, `Array`, `Object`, custom classes |
| **Examples (TS)** | `number`, `string`, `boolean`, `bigint` | Objects, arrays, functions |

```java
// Java — value vs reference behaviour
int a = 5;
int b = a;       // b gets a COPY of 5
b = 10;          // a is still 5

int[] arr1 = {1, 2, 3};
int[] arr2 = arr1;  // arr2 points to the SAME array
arr2[0] = 99;       // arr1[0] is now 99 too!
```

```typescript
// TypeScript — same concept
let a = 5;
let b = a;
b = 10;         // a is still 5

let arr1 = [1, 2, 3];
let arr2 = arr1;
arr2[0] = 99;   // arr1[0] is now 99 — same reference
```

---

## Type Coercion

### Implicit Widening (safe — no data loss)

```java
int i = 42;
double d = i;    // int → double (automatic, safe)
long l = i;      // int → long (automatic, safe)
```

### Explicit Casting (potentially lossy)

```java
double d = 9.78;
int i = (int) d;  // 9 — truncates decimal part

long big = 300;
byte b = (byte) big;  // 44 — overflow! 300 % 256 = 44
```

```typescript
// TypeScript / JavaScript
let x: number = 9.78;
let y: number = Math.floor(x);  // 9 — explicit truncation
let z: number = parseInt("42");  // string → number
```

---

## Constants vs Variables

| Language | Constant Keyword | Variable Keyword |
|---|---|---|
| Java | `final int MAX = 100;` | `int count = 0;` |
| TypeScript | `const MAX = 100;` | `let count = 0;` |
| Python | `MAX = 100` (convention: ALL_CAPS) | `count = 0` |
| C/C++ | `const int MAX = 100;` / `#define MAX 100` | `int count = 0;` |
| C# | `const int MAX = 100;` | `int count = 0;` |

> Prefer constants by default. Use variables only when the value genuinely needs to change.

---

## Naming Conventions

| Convention | Use Case | Example |
|---|---|---|
| `camelCase` | Variables, methods, parameters | `totalPrice`, `getName()` |
| `PascalCase` | Classes, interfaces, types | `UserProfile`, `ArrayList` |
| `snake_case` | Python variables/functions, DB columns | `total_price`, `get_name()` |
| `UPPER_SNAKE_CASE` | Constants | `MAX_RETRIES`, `PI` |
| `kebab-case` | CSS classes, URL slugs | `nav-bar`, `user-profile` |

---

## Scope

| Scope | Description | Example |
|---|---|---|
| **Block scope** | Variables exist only within `{ }` | Loop counters, `if`-block variables |
| **Function scope** | Variables exist for the entire function | `var` in JS, function parameters |
| **Global scope** | Accessible everywhere | Global constants, module-level vars |
| **Class scope** | Fields accessible via instances or class | Instance fields, static fields |

```java
// Java — block scope
for (int i = 0; i < 10; i++) {
    int temp = i * 2;  // temp is block-scoped
}
// i and temp are NOT accessible here
```

```typescript
// TypeScript — let vs var
for (let i = 0; i < 10; i++) { }
// console.log(i);  // ReferenceError — let is block-scoped

for (var j = 0; j < 10; j++) { }
console.log(j);  // 10 — var leaks out of the loop (function-scoped)
```

---

## Common Pitfalls

### Integer Overflow

```java
int max = Integer.MAX_VALUE;  // 2,147,483,647
int overflow = max + 1;        // -2,147,483,648 — wraps around silently!
```

### Floating-Point Precision

```java
System.out.println(0.1 + 0.2);  // 0.30000000000000004 — not 0.3!
```

```python
# Python — use Decimal for precision-critical math
from decimal import Decimal
result = Decimal("0.1") + Decimal("0.2")  # Decimal('0.3')
```

### Null Reference

```java
String name = null;
// System.out.println(name.length());  // NullPointerException!

// Defensive check
if (name != null) {
    System.out.println(name.length());
}
```

---

## Sources

- *Effective Java* (3rd ed.) — Joshua Bloch, Items 19-21
- Oracle Java Language Specification — Primitive Types
- TypeScript Handbook — Everyday Types (typescriptlang.org)
- Python Docs — Built-in Types (docs.python.org)
