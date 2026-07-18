---
title: "Expressions and Evaluation"
tags:
  - expressions
  - types
  - evaluation
  - programming-language-theory
source: "PLT Ch 3"
---

# Expressions and Evaluation

## 3.1 Is a Program Executed or Evaluated?

The fundamental "schism" between imperative and functional programming comes down to how we view a computation step.

### Imperative vs Functional Perspective

| Aspect | Imperative | Functional |
|--------|-----------|------------|
| Focus | Executing statements for effects on memory | Evaluating expressions by rewriting to a value |
| Program state | Sequence of statements + separate memory/store | An expression (term) |
| External memory | Yes (the store) | To first approximation, no |
| Examples | Assembly, C | Lambda calculus, Haskell |

### Expression Rewriting (Familiar from Arithmetic)

```
(1+1)+(1+1) ⟶ 2+(1+1)
            ⟶ 2+2
            ⟶ 4
```

The `⟶` arrow signifies one evaluation rewriting step.

### The Schism Is Somewhat False

Few languages are exclusively imperative or exclusively functional:
- **"Imperative" languages** have effect-free expression subsets (e.g., arithmetic)
- **"Functional" languages** have effectful expressions (e.g., printing to screen)

### Purity and Referential Transparency

Being **effect-free** (pure) has advantages:
- Independent of *how* a machine evaluates expressions
- **Referential transparency**: replacing a subexpression with its value cannot be observed as evaluating differently
- Final result does not depend on evaluation order
- Easier to reason about programs in isolation
- Easier for compilers to optimize

> **Key insight**: It is too simplistic to say a language *is* imperative or functional. Rather, it is a **bias in perspective** in how we see computation and programs.

- For **imperative** constructs → speak of *statement execution* modifying memory
- For **functional** constructs → think of *expression evaluation* reducing to a value

## 3.2 Basic Values, Types, and Expressions

A **value** has a **type**, which defines the operations that can be applied to it.

### Basic Types (Scala examples)

| Type | Literal Example | Description |
|------|----------------|-------------|
| `Int` | `42` | Integer |
| `Long` | `42L` | Long integer |
| `Float` | `1.618f` | Single-precision float |
| `Double` | `1.618` | Double-precision float |
| `Boolean` | `true` | Boolean |
| `Char` | `'a'` | Character |
| `String` | `"Hello!"` | String |

### Expressions

An expression can be a literal or consist of operations awaiting evaluation:

```scala
44 - 2          // Int = 42
!true           // Boolean = false
true && false   // Boolean = false
1 < 2           // Boolean = true
if (1 < 2) 3 else 4  // Int = 3
"Hello" + "!"   // String = "Hello!"
```

> A **value** is an expression that cannot be evaluated any further — the result of evaluating an expression.

### Meta-Variables

To refer to arbitrary entities in a language, we use **meta-variables**:
- `v` for a value
- `τ` for a type
- `e` for an expression

### 3.2.1 Static Type Checking

We assert types using the notation `e : τ` (expression `e` has type `τ`):

```scala
42: Int                    // ✓ well-typed
true: Boolean              // ✓ well-typed
44 - 2: Int                // ✓ well-typed
42: Boolean                // ✗ type mismatch — compilation error
true - 2                   // ✗ value - is not a member of Boolean
```

**Type checking** validates at compile-time that all operations have the expected type:

```
(1 + 2) + (3 + 4): Int
├── 1 + 2: Int
│   ├── 1: Int
│   └── 2: Int
└── 3 + 4: Int
    ├── 3: Int
    └── 4: Int
```

> **Scala is statically typed**: the compiler performs type checking at compile-time and only translates well-typed expressions.

### 3.2.2 Run-Time Errors

An expression may not always yield a value:

```scala
42 / 0   // run-time error: ArithmeticException
```

**Key terminology**:
- **Static** = before evaluating the program (compile-time)
- **Dynamic** = during the evaluation of the program (run-time)
- **Dynamically typed** = no type checking before evaluation; run-time type errors raised when evaluation encounters an operation on wrong types

### 3.2.3 Unit

Scala does not distinguish between expressions and statements. "Effectful statements" are expressions of type `Unit`:

```scala
assert(44 - 2 == 42): Unit
println("Hello!"): Unit
(): Unit   // the single unit value
```

> The `Unit` type has one single value `()` — usually associated with side-effecting expressions.

### 3.2.4 Operators

Scala has all usual operators on numeric, Boolean, and String types. Implicit conversions exist (e.g., `toInt`, `toLong`, `toString`).

**All operators are actually methods** — operator syntax is syntactic sugar:

```scala
3 + 4        // same as
3.+(4)       // method call

"Hello" endsWith "lo"   // same as
"Hello".endsWith("lo")
```

> **Syntactic sugar**: `3 + 4` is syntactic sugar for `3.+(4)`. This works for any binary method, not just symbolic ones.

## 3.3 Evaluation

### One-Step Evaluation

The computation state is an expression. We write:

```
e ⟶ e'    — expression e steps to expression e' in one step
```

Example (assuming left-to-right evaluation):
```
(1+2)+(3+4) ⟶ 3+(3+4)
```

### Multi-Step Evaluation

```
e ⟶* e'    — expression e steps to e' in 0 or more steps
```

Examples:
```
(1+2)+(3+4) ⟶* (1+2)+(3+4)   // 0 steps
(1+2)+(3+4) ⟶* 3+(3+4)       // 1 step
(1+2)+(3+4) ⟶* 3+7           // 2 steps
(1+2)+(3+4) ⟶* 10            // 3 steps
```

### Evaluation to Value

When we only care about the final value:

```
e ⇓ v    — expression e evaluates to value v
```

Example:
```
3 + 4 ⇓ 7
```

### Evaluation Order

**Eager evaluation**: sub-expressions are evaluated to values before applying the operation.

Scala evaluates `+` **left-to-right**, which can be observed via side effects:

```scala
{ println("eval left"); 1 + 2 } + { println("eval right"); 3 + 4 }
// Prints: "eval left" then "eval right"
// Result: 10
```

> If expressions are **pure** (effect-free), evaluation order cannot be observed — this is referential transparency.

## Summary of Notation

| Notation | Meaning |
|----------|---------|
| `e : τ` | Expression `e` has type `τ` |
| `e ⟶ e'` | `e` steps to `e'` in one evaluation step |
| `e ⟶* e'` | `e` steps to `e'` in zero or more steps |
| `e ⇓ v` | `e` evaluates to value `v` |


## Related

- [[Programming Language Theory Overview]] — All PLT topics
- [[02_Binding_and_Scope]] — How names get their values
- [[03_Data_Types_and_Polymorphism]] — Types and collections
