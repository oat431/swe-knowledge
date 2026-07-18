---
title: "Data Types and Polymorphism"
tags:
  - data-types
  - polymorphism
  - adt
  - programming-language-theory
source: "PLT Ch 6"
---

# Data Types and Polymorphism

> Covers standard collections (lists, options), type constructors, and pattern matching over data structures. Builds on [[02_Binding_and_Scope]].

## Standard Collections

### Lists

A `List` is a sequential, **immutable**, functional data structure — a singly-linked list. The type constructor is parameterized:

```scala
val numbers: List[Int] = List(1, 2, 3)
val names: List[String] = List("a", "b", "c")
```

#### Nil and Cons

An empty list is `Nil`. The `::` operator (read "cons") prepends an element — it is **right-associative**:

```scala
val numbers = 1 :: 2 :: 3 :: Nil
// parsed as: 1 :: (2 :: (3 :: Nil))
```

Cons is a method call under the hood: `Nil.::(3).::(2).::(1)`.

#### Immutability and Sharing

Prepending does **not** mutate the original list — it returns a new list. Because lists are immutable, `consZero` and `consTen` can **share the same tail**, making prepend O(1):

```scala
val numbers = List(1, 2, 3)
val consZero = 0 :: numbers   // List(0, 1, 2, 3)
val consTen  = 10 :: numbers  // List(10, 1, 2, 3)
// consZero.tail eq consTen.tail eq numbers  (reference equality)
```

- `eq` — reference equality (same object in memory)
- `==` — structural equality (same contents)

Appending (`:::`) is **O(n)** in the length of the left argument because it must traverse to the end.

```scala
List(1, 2, 3) ::: List(4, 5, 6)  // List(1, 2, 3, 4, 5, 6)
```

#### Indexing

Indexing is possible but uncommon in functional style:

```scala
numbers(0)          // 1 — syntactic sugar for numbers.apply(0)
numbers.head        // 1
numbers.tail        // List(2, 3)
```

#### Pattern Matching on Lists

The idiomatic way to deconstruct lists:

```scala
def isEmpty(l: List[Int]): Boolean = l match {
  case Nil           => true
  case head :: tail  => false
}
```

The `val` pattern matching for tuples is a special case of this general mechanism.

### Options

`Option[T]` represents an optional value — either `None` or `Some(value)`:

```scala
val none: Option[Int] = None
val some: Option[Int] = Some(42)
val alsoSome: Option[Int] = Option(42)      // factory method
val alsoNone: Option[Int] = Option.empty    // factory method
```

**Use case** — methods that may fail:

```scala
def div(n: Int, m: Int): Option[Int] = m match {
  case 0 => None
  case _ => Some(n / m)
}
```

Similarly, `headOption` on a list returns `None` for an empty list instead of throwing:

```scala
val emptyList: List[Int] = Nil
emptyList.head       // throws NoSuchElementException
emptyList.headOption // None — safe
```

## Higher-Order Methods on Collections

### foreach

`foreach` takes a callback `T => Unit` and applies it to each element. A `for` loop is syntactic sugar:

```scala
for (n <- numbers) println(n)
// sugar for:
numbers.foreach(n => println(n))
```

`foreach` is less commonly used because its callback type `T => Unit` forces imperative side effects.

### reduce

`reduce` accumulates a result functionally without mutable state:

```scala
def sum(l: List[Int]): Int = l.reduce { (acc, n) => acc + n }
```

Benefits over imperative loops:
- Decouples iteration scheduling from element processing
- Enables streaming, parallel, or distributed evaluation

### Composing Higher-Order Methods

Higher-order methods can be chained:

```scala
val l = List(1, 2, 3, 4, 5, 6)
val sumEvens = l.filter(_ % 2 == 0).reduce(_ + _)  // 12
```

This pipeline design is now commonplace across languages (Java streams, Python itertools, etc.) and enables **streaming** — consuming data online as a stream.

### Placeholder Syntax

Scala's `_` placeholder for function literals:

```scala
l.reduce(_ + _)
// sugar for:
l.reduce((x, y) => x + y)
```

Each `_` corresponds to a positional parameter. Use sparingly for readability.

## Pattern Matching

Pattern matching is a core mechanism for defining functions over structured data.

### Basic Match

```scala
def isZero(n: Int): Boolean = n match {
  case 0 => true
  case _ => false
}
```

- Cases are tried **top-to-bottom** — order specific patterns before general ones.
- A run-time error results if **no case matches**.

### Nested Patterns

Patterns can match deeply into nested structures:

```scala
val (_, (i, _)) = (3.14, (42, true))  // i = 42
```

### Tuples with Pattern Matching

The preferred way to destructure tuples (vs. using `._1`, `._2` projections):

```scala
val (div, rem) = divRem(7, 3)  // div = 2, rem = 1
val (div, _)   = divRem(7, 3)  // ignore second component
```

## Tuples

A tuple combines a fixed number of values: pair (2-tuple), triple (3-tuple), etc.

```scala
val pair: (Int, Boolean) = (1, true)
```

### Functions Returning Multiple Values

Tuples let functions return multiple associated values without defining a custom type:

```scala
def divRem(x: Int, y: Int): (Int, Int) = (x / y, x % y)
```

> **Rule of thumb:** Avoid tuples larger than 4–5 elements — use a named type instead.

### Unit (0-tuple)

There is no 1-tuple, but the **0-tuple** is `Unit` — the type with exactly one value `()`:

```scala
val u: Unit = { }   // block with no final expression returns ()
```

Functions returning `Unit` indicate **side effects** — they are executed for their effect, not their result.

## Side-Effecting Functions

An alternative syntax for functions with `Unit` return type:

```scala
def doNothing() { }   // = is omitted; return type fixed to Unit
```

This makes imperative Scala look more like C/Java. It is generally discouraged in functional style.

## References vs. Values

| Concept | Scala | Notes |
|---|---|---|
| Immutable binding | `val x = 1` | Name → fixed value |
| Mutable variable | `var x = 1` | Memory cell, value can change |
| Reference equality | `eq` | Same object in memory |
| Structural equality | `==` | Same contents |

Immutable data structures enable safe sharing of references, producing more efficient representations (e.g., list tail sharing).

## Related

- [[02_Binding_and_Scope]] — Value bindings, scoping, closures
- [[01_Expressions_and_Evaluation]] — Types, expressions, evaluation
- [[Programming Language Theory Overview]] — All PLT topics
