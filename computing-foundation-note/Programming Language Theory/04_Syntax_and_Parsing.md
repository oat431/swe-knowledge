---
tags:
  - syntax
  - parsing
  - grammar
  - ast
  - programming-language-theory
source: "PLT Ch 11-13"
---

# Syntax and Parsing

> **Source**: Programming Language Theory, Chapters 11–13 — Concrete Syntax, Abstract Syntax & Parsing, Syntax Exercise.

---

## 1. Concrete vs Abstract Syntax

The **syntax** of a language defines the *form* of programs (which strings are valid). The **semantics** defines the *meaning* (how programs evaluate).

| Concept | Concern | Primary Audience |
|---|---|---|
| **Concrete syntax** | How programs are written as strings; readability & writability | Language users |
| **Abstract syntax** | Representation of programs as trees; used internally by implementations | Language implementers |

- Concrete syntax is the **user interface** — design focuses on readability, tradition (e.g., `{ … }` for blocks).
- Abstract syntax makes the **tree structure explicit** — each node is a constructor, children are sub-expressions.
- A **parser** converts concrete syntax (strings) → abstract syntax (trees), resolving ambiguity along the way.

---

## 2. Context-Free Grammars (CFGs)

A **context-free grammar** defines a language inductively using:

- **Terminals** — the alphabet Σ; basic building blocks (tokens)
- **Non-terminals** (𝒩) — defined via productions; represent syntactic categories
- **Productions** — rules of the form `N ::= α` where N ∈ 𝒩 and α ∈ (Σ ∪ 𝒩)*

> Notation: `::=` is sometimes written as `→`. The empty sequence is written ε.

Multiple productions for the same non-terminal are combined with `|`:

$$N ::= \alpha_1 \mid \cdots \mid \alpha_n$$

This notation is called **BNF** (Backus-Naur Form).

### Example: Language of Integers

```
integers  i ::= -n | n
numbers   n ::= d | d n
digits    d ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

Alphabet: Σ = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, -}

The **start symbol** is the first non-terminal listed (here `i`). Strings like `1`, `42`, `-7` are in the language; `012` and `-0` are also valid.

### 2.1 Derivations

A string is in the language iff we can **derive** it from the start symbol. A one-step derivation `α ⟹ β` replaces one non-terminal in α with the RHS of one of its productions.

A **leftmost derivation** always expands the leftmost non-terminal. A **rightmost derivation** expands the rightmost.

**Example** — deriving `012`:

```
i ⟹ n ⟹ dn ⟹ 0n ⟹ 0dn ⟹ 01n ⟹ 01d ⟹ 012
```

The language of grammar G:

$$\mathcal{L}(G) = \{ s \mid s \in \Sigma^* \text{ and } S \Rightarrow^* s \}$$

where `⇒*` is the reflexive-transitive closure of `⟹`.

### 2.2 Lexical Structure vs Syntactic Structure

| Phase | Tool | Specifies | Example |
|---|---|---|---|
| **Lexing** | Lexer / Scanner | Grouping characters → tokens (regular expressions) | `"23 + 45"` → `num("23"), +, num("45")` |
| **Parsing** | Parser | Recognizing token strings (context-free grammars) | `expr ::= num \| expr + expr` |

- Analogy: lexing ≈ grouping letters into words; parsing ≈ classifying words into grammatical roles.
- **Scanner-less parsers** skip lexing and parse character streams directly (possible because CFGs ⊃ regular languages).

---

## 3. Ambiguous Grammars

A grammar is **ambiguous** when some sentence has **more than one parse tree** (equivalently, more than one derivation).

### 3.1 Associativity

**Problem**: `100 / 10 / 5` — is it `(100/10)/5 = 2` or `100/(10/5) = 50`?

Ambiguous grammar: `e ::= n | e / e`

**Fix** — rewrite with left or right recursion:

| | Ambiguous | Left-Recursive (left-associative) | Right-Recursive (right-associative) |
|---|---|---|---|
| Grammar | `e ::= n \| e / e` | `e ::= n \| e / n` | `e ::= n \| n / e` |

- **Left-associative**: parse tree linearizes left → `(100/10)/5`
- **Right-associative**: parse tree linearizes right → `100/(10/5)`

### 3.2 Precedence

**Problem**: `10 - 10 / 10` — does `-` or `/` bind tighter?

**Fix** — layer the grammar with multiple non-terminals:

```
expressions  e ::= f | e - f        (- has lower precedence)
factors      f ::= n | f / n        (/ has higher precedence, left-associative)
```

- Higher precedence = "binds tighter" = **lower** in the parse tree.
- Lower precedence = "binds looser" = **higher** in the parse tree.

> **Key insight**: Ambiguity is a *syntactic* concern (which tree?), distinct from *semantics* (what do operators mean?). But knowing semantics can help probe precedence in an unknown language.

---

## 4. Abstract Syntax

Abstract syntax makes tree structure **explicit** using constructors. Each production becomes a case class.

**Ambiguous grammar** (concrete syntax):
```
e ::= n | e / e | e - e
```

**Abstract syntax** (unambiguous — parentheses enforce grouping):
```scala
sealed trait Expr
case class N(n: Int) extends Expr
case class Divide(e1: Expr, e2: Expr) extends Expr
case class Minus(e1: Expr, e2: Expr) extends Expr
```

| Concrete | Abstract |
|---|---|
| `10 - 10 / 10` (ambiguous string) | `Minus(N(10), Divide(N(10), N(10)))` (one specific tree) |
| | `Divide(Minus(N(10), N(10)), N(10))` (a different tree) |

Each case class instance is a **node** in an n-ary tree; each non-typed argument is a **subtree**.

> Practice: give an ambiguous grammar for human readability; treat the corresponding abstract syntax as the real specification.

---

## 5. Parsing

### 5.1 Top-Down Parsing (Recursive Descent)

**Combinator parsers** use higher-order functions to build parsers from smaller parsers.

Key characteristics:
- Implements **recursive-descent parsing** — automates top-down leftmost derivation, trying productions left-to-right.
- Works best when the grammar is:
  1. **Unambiguous** (deterministic top-down derivation)
  2. **No left recursion** (each step consumes some prefix of input)

### 5.2 Left Recursion Problem

Left recursion causes **infinite recursion** in recursive descent:

```
e ⟹ e+e ⟹ e+e+e ⟹ e+e+e+e ⟹ …
```

The parser can't decide how far to "lookahead" to choose between the recursive and base-case production.

### 5.3 Solution: Restrict Concrete Syntax

Force explicit parentheses to eliminate ambiguity and left recursion:

```
terms       t ::= n | ( e )       ← s-expression style
expressions e ::= t + t
```

This is more restrictive (programmers must write more parentheses) but parses correctly with recursive descent.

> **S-expressions**: commonly used in Lisp/Scheme/Racket; easy to parse because structure is explicit via parentheses.

### 5.4 Scala Parser Combinators — Implementation

```scala
import scala.util.parsing.combinator.RegexParsers

object ExprParser extends RegexParsers {
  def term: Parser[Expr] =
    num ^^ { (n: String) => N(n.toDouble) } |
    "(" ~ expr ~ ")" ^^ { case _ ~ e ~ _ => e }

  def expr: Parser[Expr] =
    term ~ "+" ~ term ^^ { case e1 ~ _ ~ e2 => Plus(e1, e2) }

  def num: Parser[String] =
    """-?(\d+(\.\d*)?|\d*\.\d+)([eE][+-]?\d+)?""".r

  def parse(str: String): Either[String, Expr] =
    parseAll(term, str) match {
      case Success(e, _) => Right(e)
      case Failure(msg, _) => Left(s"Failure: $msg")
      case Error(msg, _) => Left(s"Error: $msg")
    }
}
```

**Key library methods**:
- `|` — separates alternative productions
- `~` — sequences symbols on the RHS of a production
- `^^` — attaches a **semantic action** (converts parse result to AST node)

**Parser[A]**: `A` is the result type. `Parser[Expr]` returns an AST; `Parser[String]` returns a matched string.

**Either[String, A]**: `Left` = error message, `Right` = successful parse result.

### 5.5 Regular Expressions for Terminals

Terminal `num` is specified by a regex:

```scala
"""-?(\d+(\.\d*)?|\d*\.\d+)([eE][+-]?\d+)?""".r
```

Matches: `1`, `-1`, `.1`, `2e2`, `2e-10`, `2E10`, etc.

---

## 6. AST Exercises — Boolean Expressions

Given grammar for Boolean expressions:

```
Boolean expressions  e ::= x | ¬e | e ∧ e | e ∨ e
variables            x
```

**Defining the inductive data type** — each production becomes a case class extending a sealed trait `Expr`, with constructors corresponding to each syntactic form.

---

## Summary Table

| Concept | Key Idea |
|---|---|
| Concrete syntax | Human-readable string representation; specified by CFGs |
| Abstract syntax | Machine-friendly tree representation; case classes / ASTs |
| Context-free grammar | `N ::= α₁ | … | αₙ`; terminals, non-terminals, productions |
| Derivation | `S ⟹* s` — sequence of production applications from start symbol |
| Parse tree | Bottom-up witness of string membership; captures syntactic structure |
| Ambiguity | Multiple parse trees for one string; resolved by rewriting grammar |
| Associativity | Left/right recursion in grammar enforces left/right grouping |
| Precedence | Layered non-terminals enforce binding strength |
| Lexer | Characters → tokens (regex-based) |
| Parser | Tokens → AST (CFG-based; recursive descent / combinator) |
| Left recursion | Causes infinite loops in top-down parsers; must be eliminated |
| Semantic action (`^^`) | Converts parser match into AST node |


## Related

- [[Programming Language Theory Overview]] — All PLT topics
- [[05_Type_Systems_and_Judgments]] — Static analysis after parsing
- [[06_Operational_Semantics]] — Semantic interpretation of ASTs
