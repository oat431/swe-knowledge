---
title: "Type Systems and Judgments"
tags:
  - type-systems
  - judgments
  - inference-rules
  - programming-language-theory
source: "PLT Ch 14–17"
---

# Type Systems and Judgments

## Static Scoping

### Variable Binding in Object Languages

Different languages express variable binding with different keywords, but the underlying structure is the same:

| Language   | Binding Syntax                | Scope Body   |
|------------|-------------------------------|--------------|
| JavaScript | `const x = e₁; e₂`          | `e₂`         |
| OCaml      | `let x = e₁ in e₂`          | `e₂`         |
| Scala      | `val x = e₁; e₂`            | `e₂`         |

**Key property:** `x` is **not** in scope in `e₁` (the binding expression). The scope of a binding is exactly `e₂`.

### Abstract Syntax for Binding

```scala
sealed trait Expr
case class N(n: Double) extends Expr                // n
case class Plus(e1: Expr, e2: Expr) extends Expr    // e1 + e2
case class Var(x: String) extends Expr              // x
case class ConstDecl(x: String, e1: Expr, e2: Expr) extends Expr  // const x = e1; e2
```

### Free Variables

A **free variable** in expression `e` is a variable whose binding is not inside `e`. The `freeVars` function computes them structurally:

```scala
def freeVars(e: Expr): Set[Var] = e match {
  case N(_)               => Set.empty
  case Plus(e1, e2)       => freeVars(e1) union freeVars(e2)
  case x @ Var(_)         => Set(x)
  case ConstDecl(x, e1, e2) =>
    freeVars(e1) union (freeVars(e2) - Var(x))
}
```

| Expression                         | Free Variables      |
|------------------------------------|---------------------|
| `N(2)`                             | `∅`                 |
| `Var("four")`                      | `{Var("four")}`     |
| `Plus(Var("four"), Var("four"))`   | `{Var("four")}`     |
| `ConstDecl("four", Plus(N(2),N(2)), Plus(Var("four"),Var("four")))` | `∅` |

A **closed expression** has no free variables ($FV(e) = \emptyset$). An **open expression** has at least one.

### Value Environments and Evaluation

An **environment** maps variable names to their values:

$$\text{Env} = [x_1 \mapsto v_1, \dots, x_n \mapsto v_n]$$

Evaluation requires an environment to give meaning to free-variable uses:

```scala
def eval(env: Env, e: Expr): Double = e match {
  case N(n)               => n
  case Plus(e1, e2)       => eval(env, e1) + eval(env, e2)
  case x @ Var(_)         => env(x)                // lookup in env
  case ConstDecl(x, e1, e2) =>
    val v1 = eval(env, e1)
    eval(env + (Var(x) -> v1), e2)                 // extend env
}
```

- `Var` case: looks up the variable in the environment.
- `ConstDecl` case: evaluates `e₁`, then extends the environment with `x ↦ v₁` for evaluating `e₂`.
- The **public interface** `evalExpr` calls `eval` with an empty environment and requires the expression to be closed.

### α-Renaming and α-Equivalence

Renaming bound variables consistently is called **α-renaming** (from the λ-calculus). These expressions are α-equivalent:

```javascript
const four = (2 + 2); (four + four)
const x    = (2 + 2); (x + x)
const fuzz = (2 + 2); (fuzz + fuzz)
```

All three have the same meaning — the binding structure is identical; only the name differs.

### Higher-Order Abstract Syntax (HOAS)

One way to encode binding structure into the AST is to use the **meta-language's** own variable binding:

```scala
case class ConstDecl(e1: Expr, e2: Double => Expr) extends Expr
```

There is no `Var` AST node — object-language variable uses are represented by meta-language variable uses in the `e2` function. This is called **higher-order abstract syntax**.

## Judgments

### Judgment Forms and Inference Rules

A **judgment** is a statement about syntactic objects — it asserts a relation on a set of objects. The **judgment form** describes the shape of the relation.

Example judgment form:

$$e : \tau$$

read as "*expression $e$ has type $\tau$*." The colon `:` is punctuation; $e$ and $\tau$ are meta-variables.

### Inference Rules

An **inference rule** has the form:

$$\frac{J_1 \quad J_2 \quad \cdots \quad J_n}{J}$$

- **Premises** ($J_1 \dots J_n$) above the horizontal line.
- **Conclusion** ($J$) below the horizontal line.
- If all premises hold, the conclusion holds.
- An **axiom** is an inference rule with no premises (empty set of premises).

### Grammars as Inference Rules

A grammar defines syntactic objects inductively. The same can be expressed with inference rules. For natural numbers:

```
grammar:   Nat  n ::= z | s(n)
```

$$\frac{}{z \in \text{Nat}} \text{Zero} \qquad \frac{n \in \text{Nat}}{s(n) \in \text{Nat}} \text{Successor}$$

This defines the **judgment form** "$n \in \text{Nat}$" — "*syntactic object $n$ is an element of the set Nat.*"

Corresponding Scala:

```scala
sealed abstract class Nat
case object Z extends Nat
case class S(n: Nat) extends Nat

def isNat(n: Nat): Boolean = n match {
  case Z    => true           // Zero axiom
  case S(n) => isNat(n)       // Successor rule
}
```

> **Key intuition:** Judgment forms ↔ function signatures. Inference rules ↔ function bodies. Judgments ↔ function calls.

### Derivations

A **derivation** is a tree where each node applies an inference rule and children are derivations of the premises. Leaves are axioms.

Example derivation for $s(s(z)) \in \text{Nat}$:

$$\frac{\frac{\frac{}{z \in \text{Nat}} \text{Zero}}{s(z) \in \text{Nat}} \text{Successor}}{s(s(z)) \in \text{Nat}} \text{Successor}$$

A judgment holds **if and only if** there exists a derivation for it (the **least** relation closed under the rules).

### Inductively-Defined Relations

#### Example: Structural Equality

Define $n_1 =_{\text{Nat}} n_2$ — "*natural number $n_1$ is structurally equal to $n_2$*":

$$\frac{}{z =_{\text{Nat}} z} \text{ZeroEq} \qquad \frac{n_1 =_{\text{Nat}} n_2}{s(n_1) =_{\text{Nat}} s(n_2)} \text{SuccessorEq}$$

```scala
def eqNat(n1: Nat, n2: Nat): Boolean = (n1, n2) match {
  case (Z, Z)           => true   // ZeroEq
  case (S(n1), S(n2))   => eqNat(n1, n2)  // SuccessorEq
  case _                => false  // no matching rule → judgment doesn't hold
}
```

- $z =_{\text{Nat}} z$ holds (derivation: ZeroEq axiom).
- $s(z) =_{\text{Nat}} s(z)$ holds (derivation: ZeroEq → SuccessorEq).
- $s(s(z)) =_{\text{Nat}} s(s(s(z)))$ does **not** hold (no complete derivation).

### Functions vs. Relations

Judgment forms are **inductively-defined relations**. When translated to code:

| Judgment Form | Code Translation | Return Type |
|---|---|---|
| Unary (membership) | Characteristic function | `Boolean` |
| Binary (functional) | Actual function | Result type |
| Binary (non-functional) | Relation check | `Boolean` |

#### Example: Semantics as a Function

Define $n \Downarrow i$ — "*natural number $n$ evaluates to integer $i$*":

$$\frac{}{z \Downarrow 0} \text{EvalZero} \qquad \frac{n \Downarrow i}{s(n) \Downarrow i+1} \text{EvalSuccessor}$$

```scala
def eval(n: Nat): Int = n match {
  case Z    => 0          // EvalZero
  case S(n) => eval(n) + 1 // EvalSuccessor
}
```

This defines a **deterministic function** because:

> **Proposition (Deterministic Evaluation).** If $n \Downarrow i_1$ and $n \Downarrow i_2$, then $i_1 = i_2$.

## Type Systems in Practice

### Representing Values

An interpreter for a language with multiple value types needs to distinguish values from general expressions:

```scala
values  v ::= n | b | str | undefined
```

The `isValue` judgment determines when an expression is a value:

```scala
def isValue(e: Expr): Boolean = e match {
  case N(_) | B(_) | S(_) | Undefined => true
  case _ => false
}
```

This corresponds to inference rules:

$$\frac{}{n \text{ value}} \text{NumVal} \qquad \frac{}{b \text{ value}} \text{BoolVal} \qquad \frac{}{\text{str value}} \text{StrVal} \qquad \frac{}{\text{undefined value}} \text{UndefVal}$$

### Implicit Coercions

Languages like JavaScript have **implicit coercions** — values are automatically converted between types depending on context. Three key coercion functions:

| Function    | Signature            | Purpose                          |
|-------------|----------------------|----------------------------------|
| `toNumber`  | `Expr → Double`      | Convert any value to a number    |
| `toBoolean` | `Expr → Boolean`     | Convert any value to a boolean   |
| `toStr`     | `Expr → String`      | Convert any value to a string    |

> **JavaScript example:** `"b" + "a" + "n" + (-"a") + "a" + "s"` → `"baNaNaS"` because `-"a"` coerces the string `"a"` to `NaN`.

### Value Environments for Interpreters

For an interpreter with multiple value types:

```scala
type Env = Map[String, Expr]   // invariant: Exprs must be values

def lookup(env: Env, x: String): Expr = env(x)

def extend(env: Env, x: String, v: Expr): Env = {
  require(isValue(v))   // enforce representation invariant
  env + (x -> v)
}
```

The `extend` function enforces that only **values** (not arbitrary expressions) are stored in the environment — a **representation invariant**.

## Meta-Language Correspondence

| Mathematical Concept     | Code Equivalent         |
|--------------------------|-------------------------|
| Judgment form            | Function signature      |
| Inference rule           | Function body / case    |
| Judgment (instance)      | Function call           |
| Derivation               | Execution trace         |
| Meta-variable / Grammar  | Type / Inductive ADT   |
| Term / Sentence          | Value of that type      |

## Related

- [[02_Binding_and_Scope]] — Value bindings, scoping, closures
- [[04_Syntax_and_Parsing]] — Grammars, concrete vs. abstract syntax
- [[06_Operational_Semantics]] — Big-step evaluation judgments
- [[Programming Language Theory Overview]]
