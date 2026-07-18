---
title: "Binding and Scope"
tags:
  - binding
  - scope
  - closures
  - programming-language-theory
source: "PLT Ch 4"
---

# Binding and Scope

## Binding Names

### Value Bindings

A **value binding** introduces a name bound to a value.

```
val x: τ = e
```

- Expression `e` must have type `τ` (annotation optional; inferred if omitted).
- At run-time, `x` is bound to the value obtained by evaluating `e`.
- If `e` does not evaluate to a value, no binding occurs.

Example:

```scala
val two = 2          // two ↦ 2
val four = two + two // four ↦ 4
```

Bindings produce a **value environment** — a finite map from names to values:

$$[x_1 \mapsto v_1, \dots, x_n \mapsto v_n]$$

To evaluate `two + two`, we apply the environment as substitution:

$$[\text{two} \mapsto 2]\;(two + two) = 2 + 2 \longrightarrow 4$$

For type checking, a **type environment** maps names to types, e.g. $[\text{two} \mapsto \text{Int}]$.

### Type Bindings

A **type alias** binds a type name to another type:

```scala
type Str = String
```

This becomes more useful with generic/parameterized types.

## Scoping

A **scope** is the region of a program where a name applies. Blocks `{ … }` introduce a new nested scope.

### Shadowing

An inner binding may **shadow** an outer binding of the same name:

```scala
val a = 1
val b = 2
val c = {
  val a = 3   // inner a shadows outer a
  a + b       // refers to inner a (3) + outer b (2) = 5
} + a          // refers to outer a (1) → result: 6
```

Key points:
- Shadowing is **not** assignment — the outer binding still exists but is hidden in the inner scope.
- We can **rename** bound variables to eliminate shadowing while preserving semantics.
- Inner-scope bindings are **not** visible in the outer/global scope.

### Static (Lexical) Scoping

Scala uses **static (lexical) scoping**: the binding for any name use is determined by examining the program text, independent of evaluation order.

**Rule:** For any use of variable `x`, the applicable binding is in the **innermost scope** that (a) contains the use of `x` and (b) has a binding for `x`.

Forward reference (using a variable before it is bound in the same scope) is a compile-time error.

## Free vs. Bound Variables

In expression `e`:
- A **bound variable** has its binding location inside `e`.
- A **free variable** has its binding location outside `e`.

```scala
{ val x = 3; x + y }  // x is bound, y is free
```

- Free variables are **inputs** to an expression — evaluation requires an environment providing their bindings.
- A **closed expression** has no free variables; it can be evaluated with an empty environment.
- An **open expression** has at least one free variable.
- When renaming bound variables within a sub-expression, we can rename consistently without changing semantics. Free variables **cannot** be renamed.

## Mutable Variables

An **immutable** variable (`val`) binds a name to a fixed value. A **mutable** variable (`var`) allocates a memory cell whose stored value can change over time.

| Language   | Immutable      | Mutable        |
|------------|----------------|----------------|
| Scala      | `val x = 1`   | `var x = 1`   |
| JavaScript | `const x = 1` | `var x = 1`   |
| Java       | `final int x` | `int x = 1`   |
| C          | `const int x` | `int x = 1`   |

Mutable variables **break referential transparency** — the value of an expression may depend on when it runs. Immutable bindings let the compiler share references aggressively, producing more efficient code.

## Functions and Tuples

### Function Definitions

```scala
def square(x: Int): Int = x * x
```

General form:

```
def f(x₁: τ₁, …, xₙ: τₙ): τ = e
```

- Formal parameter types are always required.
- Return type is good practice (always given here by convention).
- Braces `{}` are not required unless introducing new bindings.

### First-Class Functions

Functions are **values** in Scala — expressions can evaluate to function values. A **function literal** defines an anonymous function:

```scala
(x: Int) => x * x         // type: Int => Int
val square = (x: Int) => x * x  // bound to a name
```

Function values are historically called **lambdas** (from Lambda Calculus).


## Related

- [[Programming Language Theory Overview]] — All PLT topics
- [[01_Expressions_and_Evaluation]] — Expressions and evaluation
- [[05_Type_Systems_and_Judgments]] — Static scoping and type systems
- [[06_Operational_Semantics]] — Closures and environments
