---
title: Operational Semantics
tags:
  - operational-semantics
  - big-step
  - evaluation
  - programming-language-theory
source: "PLT Ch 18-20"
---

# Operational Semantics

An **operational semantics** describes how programs evaluate in terms of the language itself—essentially specifying an interpreter using mathematical relations between syntactic objects. Unlike compilation to a machine model, it defines meaning directly through inference rules.

## Big-Step Operational Semantics

A **big-step operational semantics** (also called **natural semantics**) describes evaluation from expressions to values in one "big step" using inference rules that define a judgment form.

### Evaluation Judgment Form

The core notation is:

```
e ⇓ v
```

This **judgment form** states: "Expression `e` evaluates to value `v`." Defining this judgment corresponds closely to writing a recursive interpreter over abstract syntax trees.

A set of inference rules defining this judgment form constitutes the big-step operational semantics. Rules can be read:

- **Top-down**: "If premises hold, then the conclusion holds"
- **Bottom-up** (more implementation-oriented): "To evaluate X, do Y"

### Example: Numbers and Addition

For a language with just numbers and `+`:

```
expressions  e ::= v | e₁ bop e₂
values       v ::= n
```

**EvalNum:**

```
n ⇓ n
```

**EvalPlus:**

```
e₁ ⇓ n₁    e₂ ⇓ n₂    n = n₁ + n₂
─────────────────────────────────────
e₁ + e₂ ⇓ n
```

This translates directly to an interpreter:

```scala
def eval(e: Expr): Expr = e match {
  case n @ N(_) => n                          // EvalNum
  case Binary(Plus, e1, e2) =>                // EvalPlus
    val N(n1) = eval(e1)
    val N(n2) = eval(e2)
    N(n1 + n2)
}
```

> [!note] Meta-language vs Object-language
> The `+` in the premise is the meta-language (implementation) addition, while `+` in the conclusion is the syntactic symbol in the object language (source language). Context determines which is which.

## Dynamic Typing

Adding boolean values to the language:

```
values  v ::= n | b
```

**EvalVal** (unified rule for all values):

```
v ⇓ v
```

When an operator receives incompatible operand types (e.g., `true + 2`), there is **no derivation** for the judgment—this is a **type error**. Detecting such errors at run time is **dynamic typing**:

```scala
case class DynamicTypeError(e: Expr) extends Exception
```

The implementation explicitly checks after evaluation:

```scala
case Binary(Plus, e1, e2) =>
  (eval(e1), eval(e2)) match {
    case (N(n1), N(n2)) => N(n1 + n2)
    case _ => throw DynamicTypeError(e)
  }
```

## Type Coercions

To match JavaScript semantics (where `true + 2` evaluates to `3`), introduce a coercion judgment:

```
v ⇝ n    "value v coerces to number n"
```

**ToNumberNum:** `n ⇝ n`
**ToNumberTrue:** `true ⇝ 1`
**ToNumberFalse:** `false ⇝ 0`

The evaluation rule then applies coercions before arithmetic:

```
E ⊢ e₁ ⇓ v₁    E ⊢ e₂ ⇓ v₂    v₁ ⇝ n₁    v₂ ⇝ n₂
───────────────────────────────────────────────────────
E ⊢ e₁ + e₂ ⇓ n₁ + n₂
```

## Variables and Environments

Adding variables requires a richer judgment form with an **environment** parameter:

```
E ⊢ e ⇓ v
```

"In value environment `E`, expression `e` evaluates to value `v`."

### Value Environments

A **value environment** `E` is a finite map from variables to values:

```
E ::= · | E[x ↦ v]
```

- `·` is the empty environment
- `E[x ↦ v]` extends `E` with mapping `x` to `v`
- `E(x)` looks up the value of `x`

### Variable Rules

**EvalVar:**

```
E ⊢ x ⇓ E(x)
```

**EvalConstDecl:**

```
E ⊢ e₁ ⇓ v₁    E[x ↦ v₁] ⊢ e₂ ⇓ v₂
────────────────────────────────────────
E ⊢ const x = e₁; e₂ ⇓ v₂
```

This reveals that `const x = e₁; e₂` evaluates `e₂` in an environment extended with `x` bound to the result of `e₁`—the scope of `x` is `e₂`.

## JavaScripty: Full Big-Step Semantics

The complete JavaScripty semantics (Figure 18.2) includes:

| Rule | Description |
|------|-------------|
| **EvalVar** | Variable lookup in environment |
| **EvalConstDecl** | Variable binding with scoping |
| **EvalVal** | Values evaluate to themselves |
| **EvalNeg** | Unary negation (coerces to number) |
| **EvalNot** | Logical negation (coerces to boolean) |
| **EvalArith** | Arithmetic operators (`+`, `-`, `*`, `/`) |
| **EvalAndTrue/False** | Short-circuit `&&` |
| **EvalOrTrue/False** | Short-circuit `||` |
| **EvalIfTrue/False** | Conditional expressions |
| **EvalEquality** | `===` and `!==` (no coercion) |
| **EvalInequality** | `<`, `<=`, `>`, `>=` (coerce to numbers) |

### String Overloading

String operations follow specific coercion rules:
- **String concatenation** (`+`): applies if **either** operand is a string
- **String comparison** (`<`, `<=`, `>`, `>=`): applies only if **both** operands are strings

## Functions and Dynamic Scoping

### Functions as Values

```
values      v ::= (x) => e₁
expressions e ::= e₁(e₂)
```

Function literals are values—they cannot reduce further until called. Functions are **first-class**: they can be passed and returned like any other value.

### Dynamic Scoping (The "Historical Mistake")

The straightforward EvalCall rule:

```
E ⊢ e₁ ⇓ (x) => e'    E ⊢ e₂ ⇓ v₂    E[x ↦ v₂] ⊢ e' ⇓ v'
───────────────────────────────────────────────────────────────
E ⊢ e₁(e₂) ⇓ v'
```

This evaluates the function body `e'` in the **current** environment `E`, not the environment where the function was defined. This is **dynamic scoping**—the binding site of a variable depends on program execution.

**Example exhibiting dynamic scoping:**

```
const x = 1;
const g = (y) => x;
((x) => g(2))(3)
```

- Under **static scoping**: `x` in `g` always refers to the outer `x = 1`, result is `1`
- Under **dynamic scoping**: `x` in `g` resolves to the caller's `x = 3`, result is `3`

## Closures (Static Scoping)

A **closure** captures both the function and its definition environment:

```
v ::= (x) => e₁ [E]   "function literal with environment E"
```

**EvalFun** — evaluating a function literal creates a closure:

```
E ⊢ (x) => e ⇓ (x) => e [E]
```

**EvalCall** — function body evaluated in the closure's environment:

```
E ⊢ e₁ ⇓ (x) => e' [E']    E ⊢ e₂ ⇓ v₂    E'[x ↦ v₂] ⊢ e' ⇓ v'
─────────────────────────────────────────────────────────────────────
E ⊢ e₁(e₂) ⇓ v'
```

The key difference: `E'` (the environment captured at definition time) is used instead of `E` (the current environment). This implements **static scoping**.

```scala
case f @ Fun(x, e) => Closure(f, env)  // EvalFun: capture env
case Call(e1, e2) => eval(env, e1) match {
  case Closure(Fun(x, e_), env_) =>    // EvalCall: use captured env
    val v2 = eval(env, e2)
    eval(extend(env_, x, v2), e_)
}
```

## Substitution Model

An alternative to closures for implementing static scoping is **substitution**: replace variable uses with their values eagerly, maintaining only closed expressions.

Write `[v₁/x]e` for substitution of value `v₁` for free variable uses of `x` in `e`.

**EvalConstDecl:**

```
e₁ ⇓ v₁    [v₁/x]e₂ ⇓ v₂
────────────────────────────
const x = e₁; e₂ ⇓ v₂
```

**EvalCall:**

```
e₁ ⇓ (x) => e'    e₂ ⇓ v₂    [v₂/x]e' ⇓ v'
────────────────────────────────────────────────
e₁(e₂) ⇓ v'
```

No EvalVar rule needed (variables are substituted away). No EvalFun rule needed (function literals are values directly).

## Recursive Functions

To support recursion, extend function literals with an optional name:

```
e ::= x?(y) => e₁ | e₁(e₂)
```

When a function has a name `x`, the variable `x` is bound to the function itself during evaluation, enabling self-reference.

```scala
case class Fun(xopt: Option[String], y: String, e1: Expr) extends Expr
```

The `xopt` is `Some(x)` for `x(y) => e₁` or `None` for `(y) => e₁`.

## Concrete Syntax for Functions

JavaScript provides multiple concrete syntax forms for function literals:

- `(x) => e` — arrow function (single expression)
- `(x) => { body }` — arrow function with block body
- `function x?(y) { body }` — traditional function syntax

Function bodies (`body`) consist of declarations followed by `return e;`.

## Key Concepts Summary

| Concept | Definition |
|---------|-----------|
| **Big-step semantics** | Evaluation from expression to value in one step via inference rules |
| **Judgment form** | `E ⊢ e ⇓ v` — in environment E, expression e evaluates to value v |
| **Dynamic typing** | Type errors detected at runtime when no derivation exists |
| **Type coercion** | Implicit value conversion via judgment `v ⇝ n` |
| **Dynamic scoping** | Variable binding depends on call-time environment |
| **Static scoping** | Variable binding determined by definition-time environment |
| **Closure** | Function literal paired with its definition environment `(x) => e [E]` |
| **Substitution** | Replace variable uses with values eagerly to maintain closed expressions |

## Related

- [[01_Lambda_Calculus]] — Foundation of function abstraction
- [[03_Type_Systems]] — Static typing as alternative to dynamic type checking
