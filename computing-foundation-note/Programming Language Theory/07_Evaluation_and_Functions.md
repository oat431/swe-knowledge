---
title: "Evaluation and Functions"
tags:
  - small-step
  - evaluation-order
  - substitution
  - non-determinism
  - lambda-calculus
  - programming-language-theory
source: "PLT Ch 21-22"
---

# Evaluation and Functions

Where big-step semantics jumps directly from expression to value, **small-step semantics** breaks evaluation into individual reduction steps. This finer granularity reveals *how* an expression evaluates—exposing evaluation order, intermediate states, and non-determinism. Paired with the lambda calculus model of functions, these ideas form the operational foundation for reasoning about language behavior.

## Small-Step Operational Semantics

A **small-step operational semantics** (also called **structural operational semantics**) describes evaluation as a *transition system*—one reduction step at a time.

### One-Step Judgment

The core judgment is:

```
e → e'    — expression e reduces to e' in one step
```

This contrasts with big-step (`e ⇓ v`) in a fundamental way:

| Aspect | Big-step (`⇓`) | Small-step (`→`) |
|--------|----------------|-------------------|
| Granularity | Expression → Value in one jump | One reduction step per rule |
| Intermediate states | Invisible | Observable |
| Non-terminating programs | No derivation | Infinite chain `e → e₁ → e₂ → …` |
| Evaluation order | Fixed by rule structure | Explicit in the rules |

### Multi-Step (Transitive Closure)

Multi-step evaluation is the reflexive-transitive closure of `→`:

```
e →* e'    — e reduces to e' in zero or more steps
```

Defined by two rules:

**StepRefl:**

```
e →* e
```

**StepTrans:**

```
e → e₁    e₁ →* e'
────────────────────
e →* e'
```

### Evaluation to Value

We define "evaluates to value" as reaching a value in zero or more steps:

```
e →* v    (where v is a value, i.e., irreducible)
```

A **value** is an expression that cannot be reduced further (no rule applies).

## Reduction Rules

### Arithmetic (Concrete Reduction)

For a language with numbers and `+`:

**StepPlusLeft** (reduce left operand first):

```
e₁ → e₁'
────────────────
e₁ + e₂ → e₁' + e₂
```

**StepPlusRight** (left is a value, reduce right):

```
e₂ → e₂'
────────────────
v₁ + e₂ → v₁ + e₂'
```

**StepPlus** (both are values—compute the result):

```
n₁ + n₂ → n₁ + n₂      (where + on the right is meta-language addition)
```

> [!important] Evaluation Context
> The first two rules implement a fixed evaluation order: left-to-right. StepPlusLeft applies when the left operand is not yet a value; StepPlusRight applies only after the left is a value. StepPlus applies when both are values.

### Conditional

**StepIfTrue:**

```
if true then e₂ else e₃ → e₂
```

**StepIfFalse:**

```
if false then e₂ else e₃ → e₃
```

**StepIf:**

```
e₁ → e₁'
────────────────────────────────
if e₁ then e₂ else e₃ → if e₁' then e₂ else e₃
```

### Variables and Constants

**StepConstDecl:**

```
const x = v₁; e₂ → [v₁/x]e₂
```

When the right-hand side is a value, the declaration reduces by substituting the value for the variable in the body. This is the **substitution model** in action—variables are eliminated as evaluation proceeds.

### Functions

**StepAppLeft:**

```
e₁ → e₁'
────────────
e₁(e₂) → e₁'(e₂)
```

**StepAppRight:**

```
e₂ → e₂'
────────────
v₁(e₂) → v₁(e₂')
```

**StepAppBeta** (β-reduction):

```
((x) => e)(v) → [v/x]e
```

The function application reduces by substituting the argument value `v` for the parameter `x` in the function body `e`.

## Evaluation Contexts

Writing out reduction rules for every syntactic form is tedious. **Evaluation contexts** factor this into a generic mechanism.

### Definition

An **evaluation context** `E[·]` is an expression with a "hole" `[·]` that marks the next subexpression to be reduced:

```
E ::= [·] | E + e | v + E | if E then e else e | E(e) | v(E)
```

The context grammar specifies the evaluation order: the hole marks *where* the next reduction step should happen.

### Contextual Reduction

Using evaluation contexts, reduction rules are simplified to two forms:

**StepContext:**

```
e → e'
──────────────
E[e] → E[e']
```

**StepBeta:**

```
((x) => e)(v) → [v/x]e       (or other base reduction rules)
```

The context rules handle *where* reduction occurs; the β-reduction rules handle *what* reduction does. This separates the "scheduling" from the "computation."

### Context Decomposition

Every non-value expression can be uniquely decomposed into a context and a redex:

```
e = E[e']    (where e' is the next redex)
```

This is how an interpreter decides what to reduce next.

## Evaluation Order

Small-step semantics makes evaluation order *observable*. Different context grammars give different orders.

### Call-by-Value (CBV)

Evaluate arguments before applying functions—this is the standard for JavaScript, Scala, and most imperative languages:

```
E ::= [·] | E + e | v + E | E(e) | v(E) | …
```

- `E + e`: reduce the left operand first
- `v + E`: left is a value, now reduce the right
- `E(e)`: reduce the function position first
- `v(E)`: function is a value, now reduce the argument

### Call-by-Name (CBN)

Apply functions without evaluating arguments—this is closer to Haskell's default:

```
E ::= [·] | E + e | v + E | E(e) | …
```

No `v(E)` rule—the argument is **not** reduced before application. The β-reduction substitutes the *unevaluated* expression:

```
((x) => e)(e₂) → [e₂/x]e
```

### Call-by-Need (Lazy Evaluation)

A refinement of call-by-name: expressions are substituted unevaluated, but once evaluated, the result is *memoized* (shared). This avoids re-computation and is how Haskell actually works.

```
((x) => e)(e₂) → [e₂/x]e    (but x's evaluation, once forced, is shared)
```

Implemented via an **environment** mapping variables to *thunks* (delayed computations).

### Comparison

| Strategy | Argument evaluation | Substitution target | Example languages |
|----------|--------------------|--------------------|-------------------|
| Call-by-value | Before application | Value `v` | JavaScript, Scala, Java |
| Call-by-name | Never (until used) | Unevaluated expression `e` | (Algol 60 default) |
| Call-by-need | Once (lazy, memoized) | Thunk reference | Haskell |

## Non-Determinism

### When Reduction Order Matters

If the context grammar allows multiple redexes to fire simultaneously, evaluation becomes **non-deterministic**—different reduction paths may lead to different results.

**Example with effects:**

```
let x = ref 0 in
let f = () => (x := 1; 1) in
let g = () => (x := 2; 2) in
f() + g()
```

- Left-to-right: `f()` first → `x = 1`, then `g()` → `x = 2`, result = 3, `x = 2`
- Right-to-left: `g()` first → `x = 2`, then `f()` → `x = 1`, result = 3, `x = 1`

The result is the same (3), but the final state differs.

### Confluence (Church-Rosser Property)

For **pure** (effect-free) languages, the Church-Rosser theorem guarantees **confluence**:

> If `e →* e₁` and `e →* e₂`, then there exists some `e'` such that `e₁ →* e'` and `e₂ →* e'`.

This means evaluation order does not affect the *final result*—only the reduction path. Confluence is what makes referential transparency possible.

**In the presence of effects** (mutable state, I/O), confluence is lost—evaluation order becomes semantically significant.

### Why Small-Step Matters

| Property | Observable in small-step? |
|----------|--------------------------|
| Evaluation order | Yes—context grammar encodes it |
| Intermediate states | Yes—each step is visible |
| Non-termination | Yes—infinite chain `e → e₁ → e₂ → …` |
| Confluence | Yes—by analyzing the step relation |
| Cost/complexity | Yes—count the steps |

## Substitution Revisited

### Definition

The substitution `[v/x]e` replaces all *free* occurrences of `x` in `e` with `v`:

```
[v/x]x          = v
[v/x]y          = y                    (y ≠ x)
[v/x](e₁ + e₂) = ([v/x]e₁) + ([v/x]e₂)
[v/x]((y) => e) = (y) => ([v/x]e)     (if y ≠ x and y ∉ FV(v))
[v/x]((x) => e) = (x) => e            (x is shadowed—no substitution)
```

### Capture-Avoidance

When substituting into a binding form, we must avoid **variable capture**:

```
[(y+1)/x]((y) => x + y)    // WRONG: y captured!
```

Solution: **α-convert** the bound variable first:

```
[(y+1)/x]((z) => x + z)    // α-convert y → z
= ((z) => (y+1) + z)       // now safe
```

This is why the condition `y ∉ FV(v)` appears in the substitution definition.

### Substitution vs Environments

The two approaches to managing bindings are dual:

| Substitution model | Environment model |
|-------------------|-------------------|
| Replace variable with value eagerly | Store value in a map, look up later |
| Only closed terms at runtime | Open terms evaluated in environment |
| No environment needed | No substitution needed |
| Simpler for theory | More efficient in practice |

Most interpreters use environments; most theoretical proofs use substitution.

## Lambda Calculus Foundations

### Untyped Lambda Calculus

The lambda calculus is the simplest Turing-complete programming language:

```
e ::= x | λx.e | e₁ e₂
```

Three constructs: variables, lambda abstractions (function definitions), and applications (function calls).

### Reduction Rules

**α-conversion** (renaming bound variables):
```
λx.e ≡ λy.[y/x]e    (if y ∉ FV(e))
```

**β-reduction** (function application):
```
(λx.e₁) e₂ → [e₂/x]e₁
```

**η-reduction** (extensionality):
```
λx.(f x) → f    (if x ∉ FV(f))
```

### Church Encodings

Data can be encoded as functions in the lambda calculus:

**Booleans:**
```
TRUE  = λt.λf.t
FALSE = λt.λf.f
AND   = λp.λq.p q FALSE
OR    = λp.λq.p TRUE q
IF    = λp.λa.λb.p a b
```

**Natural numbers (Church numerals):**
```
0 = λf.λx.x
1 = λf.λx.f x
2 = λf.λx.f(f x)
n = λf.λx.fⁿ(x)

SUCC = λn.λf.λx.f(n f x)
PLUS = λm.λn.λf.λx.m f(n f x)
MULT = λm.λn.λf.m(n f)
```

**Pairs:**
```
PAIR = λa.λb.λf.f a b
FST  = λp.p(λa.λb.a)
SND  = λp.p(λa.λb.b)
```

### Recursion via Fixed Points

The lambda calculus has no built-in recursion. The **Y combinator** derives it:

```
Y = λf.(λx.f(x x))(λx.f(x x))
```

For any `F`, `Y F = F(Y F)`, so `Y F` is a fixed point of `F`.

Example: factorial as a fixed point:

```
FACT = Y(λf.λn.IS_ZERO n 1 (n (f (PRED n))))
```

In call-by-value languages, the Y combinator loops. The **Z combinator** (call-by-value variant) adds an extra thunk:

```
Z = λf.(λx.f(λv.x x v))(λx.f(λv.x x v))
```

## Normal Forms

### Normal Form

An expression is in **normal form** if no reduction rule applies to it. Not all expressions have a normal form:

```
(λx.x)(42)  →  42             // has normal form
(λx.x x)(λx.x x)  →  (λx.x x)(λx.x x)  // loops forever—no normal form
```

### Normalization

A language is **strongly normalizing** (SN) if every reduction sequence terminates. The simply-typed lambda calculus is strongly normalizing—the untyped lambda calculus is not.

A language is **weakly normalizing** (WN) if every expression has *some* reduction path to normal form. The lambda calculus is weakly normalizing (Church-Rosser guarantees that if a normal form exists, all paths reach it).

## Key Concepts Summary

| Concept | Definition |
|---------|-----------|
| **Small-step semantics** | Evaluation as a transition system—one reduction step at a time |
| **Evaluation context** | Expression with a hole marking where the next reduction occurs |
| **β-reduction** | `((x) => e)(v) → [v/x]e` — applying a function by substitution |
| **Call-by-value** | Evaluate arguments to values before function application |
| **Call-by-name** | Substitute arguments unevaluated; evaluate only when needed |
| **Call-by-need** | Call-by-name with memoization (lazy evaluation) |
| **Confluence** | Reduction order does not affect the final result (Church-Rosser) |
| **Capture-avoidance** | α-convert bound variables before substitution to prevent capture |
| **Church encoding** | Data represented as pure functions in the lambda calculus |
| **Y combinator** | `λf.(λx.f(x x))(λx.f(x x))` — derives recursion from functions alone |
| **Normal form** | Expression with no reducible subexpression (redex) |

## Related

- [[06_Operational_Semantics]] — Big-step semantics (the one-step-to-value approach)
- [[01_Expressions_and_Evaluation]] — Evaluation notation (`→`, `→*`, `⇓`)
- [[02_Binding_and_Scope]] — Substitution and variable binding
