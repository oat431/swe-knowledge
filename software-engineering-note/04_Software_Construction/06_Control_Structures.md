---
tags:
  - construction
  - control-flow
  - conditionals
  - loops
  - table-driven
  - code-complete
source: "McConnell, Steve. Code Complete, 2nd Edition. Chapters 14–19."
created: 2026-07-21
---

# 06 — Control Structures

> **Source:** Steve McConnell, *Code Complete, 2nd Edition*, Chapters 14–19.  
> **Scope:** Organizing straight-line code, if/else and case statements, loop design (while, for, foreach), unusual control structures (multiple returns, recursion, goto), table-driven methods, and general control issues (boolean expressions, blocks, deep nesting, structured programming).

---

## Chapter 14 — Organizing Straight-Line Code

Straight-line code is the simplest kind of control flow: statements and blocks placed in sequential order. Even here, organizational subtleties influence correctness, readability, and maintainability.

### 14.1 Statements That Must Be in a Specific Order

When order matters — data must be read before it can be processed, results must be calculated before they are printed — the underlying concept is **dependencies**. Make those dependencies obvious.

**Guidelines for ordering statements with dependencies:**

| Technique | Description |
|---|---|
| **Organize code so dependencies are obvious** | Don't hide initialization inside unrelated routines. Write a dedicated `Initialize…()` routine. |
| **Name routines to reveal dependencies** | A routine that initializes AND computes should be named accordingly (or better, split into two). |
| **Use routine parameters to make dependencies obvious** | Pass shared data between routines; if all routines use the same `expenseData`, the order hint is clear. |
| **Document unclear dependencies with comments** | Last resort. Prefer structural techniques over comments. |
| **Check dependencies with assertions or error-handling code** | For critical code, use status variables (`isExpenseDataInitialized`) and assertions to enforce order at runtime. |

### 14.2 Statements Whose Order Doesn't Matter

When execution order is irrelevant, use **secondary criteria** guided by the **Principle of Proximity**: keep related actions together.

- **Make code read from top to bottom.** Group all operations on `marketingData` together (declare → compute → print) rather than interleaving with `salesData` and `travelData`.
- **Group related statements.** Print a listing, draw boxes around related sections — boxes should not overlap (good) or overlap (bad).
- **Extract strongly related groups into their own routines.**

> **Key Point:** The strongest principle for straight-line code is ordering dependencies. Make dependencies obvious through routine names, parameter lists, and comments.

---

## Chapter 15 — Using Conditionals

### 15.1 `if` Statements

#### Plain `if-then`

1. **Write the nominal path first; then the unusual cases.** Don't let error handling obscure the normal flow.
2. **Branch correctly on equality.** Think through the `=` case to avoid off-by-one errors.
3. **Put the normal case after the `if` rather than after the `else`.** Process errors in the `else` clause so the reader finds the nominal path by reading the `if` branches.
4. **Follow the `if` with a meaningful statement.** Never write `if (test) ; else { … }` — negate the condition instead: `if (!test) { … }`.
5. **Consider the `else` clause.** A classic GM study found 50–80% of `if` statements should have had an `else`. At minimum, document why the `else` is unnecessary.
6. **Test the `else` clause for correctness.**
7. **Check for reversal of `if` and `else` clauses.**

#### Chains of `if-then-else`

- **Simplify complicated tests with boolean function calls.** Replace `if ( 'a' <= ch && ch <= 'z' )` with `if ( IsLetter( ch ) )`.
- **Put the most common cases first.** Minimizes reading and improves efficiency.
- **Make sure all cases are covered.** Code a final `else` with an error message/assertion.
- **Replace chains with `case`/`switch` if your language supports it.**

### 15.2 `case` Statements

#### Ordering Cases

| Strategy | When to Use |
|---|---|
| **Alphabetical / numerical** | Cases are equally important; improves searchability. |
| **Normal case first** | One normal case, several exceptions. |
| **By frequency** | Most-executed cases first — faster for humans and CPUs. |

#### Tips

- **Keep case actions simple.** If a case needs more than a few lines, call a routine.
- **Don't manufacture phony variables** just to use `case`. Use `if-then-else` chains for complex matching (e.g., full string comparison).
- **Use `default` only for legitimate defaults.** Don't collapse the last real case into `default`.
- **Use `default` to detect errors.** `DisplayInternalError( "Internal Error 905: Call customer support." )`
- **In C/C++/Java, avoid fall-through.** If intentional, comment `// FALLTHROUGH` explicitly.

> **Key Point:** Choose the control construct most appropriate for each section of code. All control constructs are not created equal.

---

## Chapter 16 — Controlling Loops

### 16.1 Selecting the Kind of Loop

| Loop Type | Flexibility | Test Location | Use When |
|---|---|---|---|
| **`for`** | Rigid (C++/Java: flexible) | Beginning | Known iteration count; simple increments |
| **`while`** | Flexible | Beginning | Unknown count; may execute zero times |
| **`do-while`** | Flexible | End | Must execute at least once |
| **`foreach`** | Rigid | Beginning | Operating on every element of a container |
| **Loop-with-exit** | Flexible | Middle | Avoids "loop-and-a-half" duplication |

**Loop-with-exit** (e.g., `while (true) { … if (condition) break; … }`) is the **preferred kind of loop control** (Software Productivity Consortium, 1989). Students scored 25% higher on comprehension tests when loop-with-exit loops were used (Soloway, Bonar, Ehrlich 1983).

**Don't abuse `for` loop headers** by cramming non-control statements into them. The `for` header should contain only loop-control statements (initialize, test, advance).

### 16.2 Controlling the Loop

**Core principle: Treat the loop as a black box.** Keep control conditions outside the loop body. The surrounding program knows the control conditions but not the contents.

#### Entering the Loop
- **Enter from one location only** (the top).
- **Put initialization code directly before the loop** (Principle of Proximity).
- **Use `while (true)` for infinite loops**, not `for i = 1 to 99999`.
- **Prefer `for` loops when they're appropriate** — they package control in one place.

#### Processing the Middle
- **Always use `{ }` braces** even for single-statement bodies.
- **Avoid empty loops.** Convert `while ((ch = get()) != EOF) ;` to `do { ch = get(); } while (ch != EOF);`
- **Group housekeeping chores** at either the beginning or the end of the loop.
- **Make each loop perform only one function.**

#### Exiting the Loop
- **Assure yourself the loop ends.** Mentally simulate nominal, endpoint, and exceptional cases.
- **Make termination conditions obvious.** Put control in one place.
- **Don't monkey with the `for` loop index** to force termination. Use a `while` instead.
- **Avoid code that depends on the loop index's final value.** Use a separate variable (e.g., `found = true`).
- **Consider safety counters** for critical loops: `if (safetyCounter >= SAFETY_LIMIT) Assert(false, …)`.

#### Exiting Loops Early (`break` / `continue`)
- **`break`:** Use instead of boolean flags when it simplifies loop control.
- **Be wary of many `break`s** — a proliferation suggests unclear structure.
- **`continue`:** Good for skipping at the top of a loop (e.g., discarding records of a certain type). Avoid `continue` in the middle or end of a loop — use `if` instead.
- **Use labeled `break`** (Java) for unambiguous multi-level exits.
- **Use `break` and `continue` with caution** — they break the "black box" abstraction.

#### Checking Endpoints
Mentally run the first, middle, and last cases. This discipline separates efficient programmers from those who experiment randomly with `<` vs. `<=`.

#### Loop Variables
- **Use ordinal/enumerated types** — not floating-point (26,742,897.0 + 1.0 may equal 26,742,897.0).
- **Use meaningful names** in nested loops. `payCodeIdx`, `month`, `divisionIdx` > `i`, `j`, `k`.
- **Limit scope of loop-index variables to the loop itself** (`for (int i = 0; …)`).

#### Loop Length
- Make loops short enough to view all at once (≤ 15–20 lines).
- Limit nesting to **three levels** (comprehension deteriorates beyond that).
- Move long loop innards into separate routines.

### 16.3 Creating Loops Easily — From the Inside Out

1. Write the body steps as comments.
2. Convert comments to code with **concrete, literal values**.
3. Replace literals with array/loop indexes.
4. Wrap in a loop construct.
5. Generalize variables that depend on the loop index.
6. Add initializations.

> **Key Point:** Loops are complicated. Keep them simple — avoid exotic loops, minimize nesting, make entries/exits clear, and keep housekeeping in one place.

---

## Chapter 17 — Unusual Control Structures

### 17.1 Multiple Returns from a Routine

**Use `return` when it enhances readability.** Guard clauses (early returns for error conditions) simplify deeply nested error processing:

```vb
' Guard clauses — nominal path is clear
If Not file.validName() Then Exit Sub
If Not file.Open() Then Exit Sub
If Not encryptionKey.valid() Then Exit Sub
If Not file.Decrypt(encryptionKey) Then Exit Sub
' lots of nominal code…
```

**Minimize the number of returns.** A reader at the bottom shouldn't be surprised by an early return above.

### 17.2 Recursion

Recursion solves a small part of a problem, divides the rest, and calls itself. It produces elegant solutions for a **small set of problems** (e.g., quicksort, maze traversal, tree walking). For most problems, iteration is more understandable.

#### Tips for Using Recursion

- **Make sure the recursion stops** — include a non-recursive path (base case).
- **Use safety counters** to prevent infinite recursion.
- **Limit recursion to one routine.** Cyclic recursion (A → B → C → A) is dangerous.
- **Watch the stack.** Use heap allocation (`new`) for memory-intensive objects in recursive functions.
- **Don't use recursion for factorials or Fibonacci numbers.** These are textbook anti-patterns — iteration is simpler, faster, and more predictable.

> **Key insight:** You can do anything with stacks and iteration that you can do with recursion. Consider both before choosing.

### 17.3 `goto`

The `goto` debate (Dijkstra, "Go To Statement Considered Harmful," 1968) is still relevant. Modern equivalents appear in debates about multiple returns, multiple loop exits, and exception handling.

#### Arguments Against
- Code quality is inversely proportional to `goto` count.
- Hard to format and indent correctly.
- Defeats compiler optimizations.
- Violates top-to-bottom flow.

#### Arguments For (in specific, rare cases)
- Eliminates duplicate cleanup code in resource allocation/deallocation.
- Can avoid deep nesting in error-handling code.

#### Alternatives to `goto`

| Approach | Pros | Cons |
|---|---|---|
| **Nested `if`s** | Textbook approach, no `goto` | Deep nesting, error code far from test |
| **Status variable** | Avoids deep nesting, readable | Extra tests, less common practice |
| **`try-finally`** | Simplest, cleanest | Not available in all languages; requires consistent exception strategy |
| **Extract to routine** | Best for shared code in `else` clauses | Not always practical |

#### Guidelines (if you must use `goto`)
- Use only to emulate structured constructs in languages that lack them.
- Limit to **one label per routine**.
- `goto` **forward only**, not backward.
- Ensure all labels are used; delete unused ones.
- **Measure and document** any efficiency gain.
- If a programmer is aware of alternatives and still argues for `goto`, it's probably OK.

> **Key Point:** In modern languages, you can replace 99 out of 100 `goto`s. For the remaining 1%, document clearly and use it.

### 17.4 Perspective

Historically accepted constructs that are now considered dangerous: unrestricted `goto`, computed `goto` targets, jumping between routines, self-modifying code. **The field advances by restricting what programmers can do.**

---

## Chapter 18 — Table-Driven Methods

> A table-driven method looks up information in a table rather than using logic statements (`if`, `case`). As logic chains grow complex, tables become increasingly attractive.

### Core Insight

Replace this:
```java
if (( 'a' <= ch && ch <= 'z' ) || ( 'A' <= ch && ch <= 'Z' ))
    type = Letter;
else if ( ch == ' ' || ch == ',' || … )
    type = Punctuation;
else if ( '0' <= ch && ch <= '9' )
    type = Digit;
```

With this:
```java
type = charTypeTable[ ch ];
```

**Put knowledge into data, not logic.**

### Two Key Questions

1. **How to look up entries?** → Direct access, indexed access, stair-step access.
2. **What to store?** → Data (values) or actions (codes / routine references).

### 18.2 Direct Access Tables

Use when the lookup key maps directly to a table index.

**Examples:** Days per month (`daysPerMonth[month-1]`), insurance rates by multi-dimensional array, flexible message format interpreter.

**Flexible-Message-Format Example:** Instead of writing 20 separate routines (one per message type), define message formats in a data table. One generic routine reads any message format by looking up field types. **Data is more flexible than logic.** New message types require only data changes, not code changes.

**Fudging Lookup Keys:** When data doesn't map directly (e.g., age bands: under 18, 18–65, over 65):
- **Duplicate information** to make the key work (wastes space).
- **Transform the key** with a function: `max(min(66, Age), 17)`.
- **Isolate the transformation in its own routine** (e.g., `KeyFromAge()`).

### 18.3 Indexed Access Tables

When the key space is sparse (e.g., 100 part numbers in a 0000–9999 range), use an intermediate index array. The index maps sparse keys to a dense main table, saving memory.

**Advantages:** Space efficiency, cheaper index manipulation, maintainability.

### 18.4 Stair-Step Access Tables

When entries are valid for **ranges** of data rather than discrete points (e.g., grading: ≥90 = A, 75–89 = B, etc.).

- Put the upper end of each range in a table.
- Loop to find the first range the value falls into.
- **Watch endpoints** — handle the topmost range correctly.
- **Consider binary search** for large tables.
- **Consider indexed access** if execution speed matters and space permits.

**Put the stair-step lookup into its own routine.**

> **Key Point:** Tables provide an alternative to complicated logic AND complicated inheritance structures. If you're confused by a program's logic or inheritance tree, ask whether a lookup table could simplify it.

---

## Chapter 19 — General Control Issues

### 19.1 Boolean Expressions

#### Using `true` and `false`
- Use named boolean constants, not 0 and 1.
- **Compare boolean values implicitly:** `while (!done)` not `while (done == false)`.
- This reads more like conversational English.

#### Simplifying Complicated Expressions
- **Break into partial tests with new boolean variables.**
- **Move complicated expressions into well-named boolean functions.** Even single-use tests benefit from the abstraction and documentation.
- **Use decision tables** to replace complicated multi-variable conditions.

#### Forming Boolean Expressions Positively
- **Convert negatives to positives** and flip-flop `if`/`else` clauses.
  - `if (!statusOK)` → `if (statusOK)` with swapped bodies.
- **Apply DeMorgan's Theorems** to simplify negated expressions:
  - `!A && !B` ≡ `!(A || B)`
  - `!A || !B` ≡ `!(A && B)`

#### Parentheses
- **Fully parenthesize logical expressions.** Parentheses are free; ambiguity is expensive.
- **Use the counting technique to balance parentheses:** +1 for `(`, −1 for `)`, should end at 0.

#### Short-Circuit Evaluation
- C++, Java, C# use short-circuit evaluation: `&&` and `||` stop as soon as the result is known.
- `if ((denom != 0) && (item/denom > MIN))` — safe; division skipped if `denom == 0`.
- Java's `&` and `|` are **non-short-circuit** logical operators — all terms evaluated.
- **Prefer nested tests** over relying on evaluation order for clarity.

#### Numeric Expressions in Number-Line Order
- Write: `MIN_ELEMENTS <= i && i <= MAX_ELEMENTS`
- Not: `i > MIN_ELEMENTS && i < MAX_ELEMENTS`
- Visualize the comparison on a number line.

#### Comparisons to 0
| Context | Recommendation | Example |
|---|---|---|
| Logical | Implicit | `while (!done)` |
| Numeric | Explicit | `while (balance != 0)` |
| Character (C) | Explicit | `while (*ptr != '\0')` |
| Pointer | Explicit | `while (ptr != NULL)` |

#### Common Pitfalls
- **C-derived languages:** Put constants on the left (`if (MIN == i)`) — compiler catches `=` vs `==` errors. (May conflict with number-line ordering; McConell prefers number-line ordering.)
- **Java:** `a.equals(b)` for logical equality, `a == b` for reference identity.

### 19.2 Compound Statements (Blocks)

- **Write pairs of braces together first**, then fill in the middle.
- **Always use braces**, even for single-statement bodies — prevents errors during maintenance.

### 19.3 Null Statements

In C++, `while (…) ;` is a null statement.

- **Call attention to null statements.** Give the semicolon its own line, or use empty braces `{ }`.
- **Create a `DoNothing()` macro or inline function** for explicit intent.
- **Consider whether a non-null loop body would be clearer.** Move side effects out of the loop-control expression and into the body.

### 19.4 Taming Dangerously Deep Nesting

> Studies suggest few people can understand more than **three levels** of nested `if`s (Noam Chomsky, Gerald Weinberg). Deep nesting works against managing complexity.

**Techniques to reduce nesting:**

| Technique | Description |
|---|---|
| **Retest part of the condition** | Replace nested `if`s with a flatter structure using combined conditions. |
| **Use a `break` block** | `do { if (fail) break; … } while (FALSE);` — a single-exit block. (Uncommon; use only if team adopts it.) |
| **Convert to `if-then-else` chain** | Restructure bushy decision trees into flat chains. |
| **Convert to `case` statement** | When ranges are clean, `Select Case` / `switch` eliminates nesting. |
| **Factor into its own routine** | Move deeply nested loop innards or conditional branches into separate, well-named routines. |

### 19.5 Structured Programming

Structured programming is the foundation of modern control flow. Its core idea: programs should be built from a small set of single-entry, single-exit control constructs — sequence, selection (`if-then-else`), and iteration (`while`, `for`). The movement, sparked by Dijkstra, Böhm, and Jacopini in the late 1960s, demonstrated that `goto` is never strictly necessary. Modern languages have fully embraced this: structured constructs are the norm, and unrestricted `goto` is the exception.

### 19.6 Control Structures and Complexity

The relationship between control structures and complexity is central to software construction:

- **Complexity is the primary technical imperative.** Control structures are the primary tool for managing it.
- **Measure complexity** by counting decision points (McCabe's cyclomatic complexity). Every `if`, `while`, `for`, `&&`, `||`, and `case` adds 1.
- **Keep complexity low.** Routines with complexity > 10 are hard to test; > 20 are nearly untestable.
- **Choose the simplest control structure** that solves the problem clearly. Favor `for` over `while` when iteration count is known; favor table lookup over long `if-else` chains; favor guard clauses over deep nesting.

> **McConnell's Law:** The more control structures you have, the more paths through your code. The more paths, the harder it is to understand, test, and maintain. Simplicity in control flow is not a luxury — it's a necessity.

---

## Summary: Key Principles Across All Control Structures

1. **Make dependencies obvious** — through names, parameters, and structure.
2. **Put the normal case first** — in conditionals, loops, and error handling.
3. **Keep it simple** — short routines, shallow nesting, one purpose per loop.
4. **Use the right tool** — `for` for counted iteration, `while` for conditional, tables for complex logic.
5. **Treat loops as black boxes** — control outside, work inside.
6. **Put knowledge in data, not logic** — table-driven methods trump complex conditionals.
7. **Avoid deep nesting** — more than 3 levels impairs comprehension.
8. **Use boolean abstractions** — well-named functions and variables make conditions self-documenting.
