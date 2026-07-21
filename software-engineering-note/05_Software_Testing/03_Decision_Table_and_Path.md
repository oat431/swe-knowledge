---
tags:
  - testing
  - decision-table
  - path-testing
  - coverage
  - software-testing
  - cause-effect-graphing
  - mcdc
  - cyclomatic-complexity
  - dd-paths
  - basis-path-testing
---

# Decision Table & Path Testing

> **Source:** Jorgensen, *Software Testing: A Craftsman's Approach*, Chapters 7–8
> **Purpose:** Rigorous functional testing via decision tables and structural testing via program graphs, DD-paths, coverage metrics, and basis path testing.

## Overview

Chapters 7 and 8 represent the two major schools of test case design: **specification-based** (decision tables — the most rigorous functional method) and **code-based** (path testing — the most analytically precise structural method). Together they form a complete framework: decision tables handle complex input-domain logic, while path testing ensures adequate code-level coverage.

---

## Part I: Decision Table–Based Testing (Ch. 7)

### 7.1 Decision Table Fundamentals

A decision table has four quadrants divided by bold horizontal and vertical lines:

| | **Stub** (what) | **Entry** (values) |
|---|---|---|
| **Condition** (above) | Condition Stub — lists all conditions | Condition Entries — T/F/— values |
| **Action** (below) | Action Stub — lists all actions | Action Entries — X marks selected actions |

- A **rule** is one column in the entry portion — it specifies which actions fire for a given set of condition values.
- Binary conditions (T/F, Yes/No, 0/1) produce a **Limited Entry Decision Table (LEDT)** — essentially a truth table rotated 90°.
- Multi-valued conditions produce an **Extended Entry Decision Table (EEDT)**.
- Decision tables are **declarative** — no condition order is implied, and actions don't occur in any prescribed sequence.

### 7.2 Decision Table Techniques

#### Completeness and Rule Counting

For an LEDT with *n* conditions, there must be 2ⁿ independent rules. Don't care entries ("—") mean the condition is irrelevant or doesn't apply. The rule count algorithm:

> Rules with no don't cares count as 1. Each don't care entry doubles the count of that rule. The sum of rule counts must equal 2ⁿ.

#### Don't Care Entries and the "Impossible" Action

- **Don't care ("—"):** Condition is irrelevant to the rule. Can also mean "must be false" in mutually exclusive condition sets (some use "F!" notation).
- **Impossible action:** Marks rules that are logically contradictory. Example: in the triangle problem, if `a = b` and `b = c` are both true, then `a = c` *must* be true by transitivity — rules where `a = b` and `b = c` are true but `a = c` is false are impossible.

#### Redundancy and Inconsistency

- **Redundant:** Two rules have identical condition entries and identical action entries — harmless.
- **Inconsistent:** Two rules have identical condition entries but *different* action entries — makes the table **nondeterministic**.

#### Mutually Exclusive Conditions

When conditions refer to equivalence classes (e.g., month ∈ M1, month ∈ M2, month ∈ M3), at most one can be true. The naive rule-count algorithm overcounts; you must expand rules and eliminate duplicates. The missing rule is always **all conditions false**.

### 7.3 Triangle Problem — Decision Table

Using conditions `a < b + c`, `b < a + c`, `c < a + b`, `a = b`, `a = c`, `b = c`:

| Test Case | a | b | c | Expected Output |
|-----------|---|---|---|-----------------|
| DT1 | 4 | 1 | 2 | Not a triangle |
| DT2 | 1 | 4 | 2 | Not a triangle |
| DT3 | 1 | 2 | 4 | Not a triangle |
| DT4 | 5 | 5 | 5 | Equilateral |
| DT5–DT6 | — | — | — | Impossible |
| DT7 | 2 | 2 | 3 | Isosceles |
| DT8 | — | — | — | Impossible |
| DT9 | 2 | 3 | 2 | Isosceles |
| DT10 | 3 | 2 | 2 | Isosceles |
| DT11 | 3 | 4 | 5 | Scalene |

**11 test cases** total: 3 impossible, 3 triangle-property failures, 1 equilateral, 1 scalene, 3 isosceles.

### 7.4 NextDate Function — Three Iterations

NextDate is the canonical example of **input-domain dependencies** — indiscriminate Cartesian products of equivalence classes produce nonsensical tests (e.g., June 31). Decision tables expose these dependencies via "impossible" rules.

#### First Try (256 rules — too many)

Conditions: Month ∈ {M1(30d), M2(31d), M3(Feb)}, Day ∈ {D1(1–28), D2(29), D3(30), D4(31)}, Year ∈ {leap, non-leap}. Yields 256 rules, most impossible.

#### Second Try (36 rules → 16 after combination)

Introduces gray-box actions: increment day, reset day, increment month, reset month, increment year. Still has problems with December transitions and Feb 28.

#### Third Try (40 elements → 22 rules → reduced to test cases)

Refined equivalence classes:
- **M1:** 30-day months | **M2:** 31-day months except December | **M3:** December | **M4:** February
- **D1:** 1–27 | **D2:** 28 | **D3:** 29 | **D4:** 30 | **D5:** 31
- **Y1:** leap year | **Y2:** common year

Key insight: **rule equivalence** — when action sets are identical across rules, combine them with don't care entries. The 22 rules collapse to a practical test suite:

| Case ID | Month | Day | Year | Expected Output |
|---------|-------|-----|------|-----------------|
| 1–3 | 4 | 15 | 2001 | 4/16/2001 |
| 4 | 4 | 30 | 2001 | 5/1/2001 |
| 5 | 4 | 31 | 2001 | Invalid input date |
| 6–9 | 1 | 15 | 2001 | 1/16/2001 |
| 10 | 1 | 31 | 2001 | 2/1/2001 |
| 11–14 | 12 | 15 | 2001 | 12/16/2001 |
| 15 | 12 | 31 | 2001 | 1/1/2002 |
| 16 | 2 | 15 | 2001 | 2/16/2001 |
| 17 | 2 | 28 | 2004 | 2/29/2004 |
| 18 | 2 | 28 | 2001 | 3/1/2001 |
| 19 | 2 | 29 | 2004 | 3/1/2004 |
| 20 | 2 | 29 | 2001 | Invalid input date |
| 21, 22 | 2 | 30 | 2001 | Invalid input date |

### 7.5 Commission Problem

Decision tables add little value here — the variables are truly independent, no impossible rules arise, and test cases mirror equivalence class testing. **Decision tables are most valuable when logical dependencies exist among input variables.**

### 7.6 Cause-and-Effect Graphing

A **hardware-inherited** technique: inputs flow through AND/OR/NOT gates from left to right. Basic operations: AND, OR, NOT, Identity, Masks, Requires, Only One.

**Limitation:** Shows paths from faulty outputs back to inputs but provides little support for actually identifying test cases. Decision tables fully subsume this approach. Included primarily for historical completeness.

### 7.7 Decision Table Guidelines

1. **Best for:** prominent if–then–else logic, logical input dependencies, cause-and-effect relationships, high cyclomatic complexity
2. **Scaling problem:** LEDTs grow as 2ⁿ. Mitigations: use EEDTs, algebraic simplification, factor large tables into smaller ones, exploit repeating patterns
3. **Iterate:** First conditions/actions are rarely "right." Use as a stepping stone and refine
4. **Decision tables improve programming:** Analysis done for testing often reveals insights useful during detailed design

---

## Part II: Path Testing (Ch. 8)

### 8.1 Program Graphs

**Definition:** A directed graph where nodes are *statement fragments* and edges represent *flow of control*. An edge exists from node *i* to node *j* iff fragment *j* can execute immediately after fragment *i*.

#### Structured Programming Constructs as Program Graphs

| Construct | Graph Pattern |
|-----------|--------------|
| Sequence | Linear chain: node → node → node |
| If–Then–Else | One decision node → two parallel branches → merge node |
| If–Then | Decision node → then branch → bypass → merge |
| Case/Switch | One decision node → *n* parallel branches → merge |
| Pretest Loop (While) | Decision node → loop body → back edge; false exit |
| Posttest Loop (Do-Until) | Loop body → decision node → back edge; false exit |

**Key property:** Program executions correspond to paths from source to sink nodes. This gives an explicit relationship between test cases and the code they exercise.

### 8.2 DD-Paths (Decision-to-Decision Paths)

Originated by E.F. Miller (1977). A DD-path is a sequence of statements that begins at a decision's "outway" and ends at the next decision's "inway" — like dominoes: no internal branches.

**Formal Definition (5 cases):**

| Case | Condition | Meaning |
|------|-----------|---------|
| Case 1 | Single node, indegree = 0 | Source node (program entry) |
| Case 2 | Single node, outdegree = 0 | Sink node (program exit) |
| Case 3 | Single node, indegree ≥ 2 **or** outdegree ≥ 2 | Decision nodes, merge nodes |
| Case 4 | Single node, indegree = 1 and outdegree = 1 | "Short branch" — preserves one-fragment-per-DD-path |
| Case 5 | Maximal chain of length ≥ 1 | Normal case: a chain where all interior nodes have indegree = outdegree = 1 |

> A **chain** is a path where initial and terminal nodes are distinct and every interior node has indegree = 1 and outdegree = 1.

The **DD-path graph** is a condensation of the program graph where each DD-path becomes a single node. Edges represent control flow between successor DD-paths.

### 8.3 Test Coverage Metrics

#### 8.3.1 Program Graph–Based Metrics

| Metric | Symbol | Definition |
|--------|--------|------------|
| **Node Coverage** | G_node | Every node in the program graph is traversed (≈ statement coverage) |
| **Edge Coverage** | G_edge | Every edge in the program graph is traversed (≈ branch/predicate outcome coverage) |
| **Chain Coverage** | G_chain | Every chain of length ≥ 2 is traversed (= node coverage of the DD-path graph) |
| **Path Coverage** | G_path | Every source-to-sink path is traversed (severely limited by loops) |

**Relationship:** G_path ⊃ G_chain ⊃ G_edge ⊃ G_node

#### 8.3.2 E.F. Miller's Coverage Metrics

| Metric | Name | Description |
|--------|------|-------------|
| **C₀** | Statement Coverage | Every statement executed at least once |
| **C₁** | DD-Path Coverage | Every DD-path traversed (= G_chain = G_edge) |
| **C₁ₚ** | Predicate Outcome Coverage | Each predicate evaluated to both true and false |
| **C₂** | C₁ + Loop Coverage | DD-path coverage plus exercising both outcomes of every loop decision |
| **C_d** | Dependent Pairs | C₁ + every dependent pair of DD-paths (e.g., define/reference pairs) |
| **C_MCC** | Multiple Condition Coverage | All combinations of simple conditions in compound predicates |
| **C_ik** | Loop Repetition Coverage | Every path with up to *k* loop repetitions (usually k = 2) |
| **C_stat** | Statistical Coverage | "Statistically significant" fraction of paths |
| **C_∞** | Exhaustive Path Coverage | All possible execution paths |

> **Key insight (Miller, 1991):** When DD-path coverage (C₁) is attained, roughly **85% of all faults** are revealed.

#### Coverage Lattice

These metrics form a **lattice** — some are equivalent, some implied by others. The importance: fault types exist that are revealed at one level but escape detection at inferior levels.

| Level | Reveals |
|-------|---------|
| C₀ | Missing statements, dead code |
| C₁ / C₁ₚ | Untested branches, missing decision outcomes |
| C₂ | Loop boundary errors, off-by-one in iterations |
| C_d | Define/reference anomalies, infeasible paths |
| C_MCC | Short-circuit evaluation bugs, compound condition errors |

### 8.3.3 Compound Conditions and MCDC

#### Boolean Expression Terminology (per Chilenski)

- **Boolean expression:** Evaluates to True/False. May be simple or compound with ∧, ∨, ⊕, ¬ operators.
- **Condition:** An operand of a Boolean operator — the "leaf" of an expression tree.
- **Coupled conditions:** Changing one also changes another. Can be *strongly coupled* (always) or *weakly coupled* (sometimes).
- **Masking:** Holding one operand of an operator at a value that prevents the other operand from affecting the output (domination laws):
  - `X AND False = False` (masks X)
  - `X OR True = True` (masks X)

#### Modified Condition Decision Coverage (MCDC)

Required for **DO-178B Level A** software. Six requirements:

1. Every statement executed at least once
2. Every entry/exit point invoked at least once
3. All outcomes of every control statement taken at least once
4. Every nonconstant Boolean expression evaluated to both true and false
5. Every nonconstant condition in a Boolean expression evaluated to both true and false
6. Every nonconstant condition **independently affects** the expression's outcome

**Three MCDC variants:**

| Variant | Rule |
|---------|------|
| **Unique-Cause MCDC** | Toggle a single condition, hold all others fixed, observe outcome change |
| **Unique-Cause + Masking MCDC** | Mask only for strongly coupled conditions |
| **Masking MCDC** (recommended for DO-178B) | Allow masking for all conditions |

**Example:** For `(a AND (b OR c))`:
- Toggle `a`: Rules 1↔5 (hold b,c constant) ✓
- Toggle `b`: Rules 2↔4 (hold a,c constant) ✓
- Toggle `c`: Rules 3↔4 (hold a,b constant) ✓

#### The Decision Table Fallback

When compound conditions get complex, the **best practical solution** is to create a decision table of the actual code and look for impossible rules. Dependencies among conditions typically generate impossible rules. Alternatively, rewrite compound conditions as nested If–Then–Else logic, which makes each condition's independence explicit in the program graph.

### 8.3.4 Coverage Analyzers

Automated tools that **instrument** source code, execute test suites, and report which DD-paths/predicates were traversed. Enable: experimenting with test sets, identifying coverage gaps, and demonstrating compliance. Reference: [www.testtoolreview.com](http://www.testtoolreview.com).

---

### 8.4 Basis Path Testing (McCabe)

#### Mathematical Foundation

McCabe leverages graph theory: the **cyclomatic number** of a strongly connected graph equals the number of **linearly independent circuits**. By adding an edge from sink to source, any program graph becomes strongly connected.

**Cyclomatic Complexity Formula:**

```
V(G) = e − n + 2p    (for program graphs, independent paths source→sink)
V(G) = e − n + p     (for strongly connected graphs, independent circuits)
```

Where *e* = edges, *n* = nodes, *p* = connected regions (usually 1).

#### Vector Space of Paths

Program paths form a vector space with:
- **Addition:** Path concatenation (p1 followed by p2)
- **Scalar multiplication:** Path repetition

A **basis** is a set of paths that are linearly independent and "span" all possible paths. The size of the basis = V(G).

#### The Baseline Method

1. Choose a **baseline path** — typically the "normal case" with as many decision nodes as possible
2. Retrace the baseline, and at each decision node (outdegree ≥ 2), **flip** to take the alternative edge
3. For subsequent paths, keep later decisions identical to the baseline to minimize differences
4. Continue until all decision nodes have been flipped

#### Critical Problem: Infeasible Paths

The baseline method produces **topologically independent** paths, but these may be **semantically infeasible** due to logical dependencies among variables.

**Triangle program example:**

| Path | Description | Feasible? |
|------|-------------|-----------|
| p1: A–B–C–E–F–H–J–K–M–N–O–Last | Scalene | ✓ |
| p2: A–B–D–E–F–H–J–K–M–N–O–Last | Flip B (not a triangle → check equality) | ✗ Infeasible |
| p3: A–B–C–E–F–G–O–Last | Flip F (is triangle → output "not a triangle") | ✗ Infeasible |
| p4: A–B–C–E–F–H–I–N–O–Last | Flip H → Equilateral | ✓ |
| p5: A–B–C–E–F–H–J–L–M–N–O–Last | Flip J → Isosceles | ✓ |

**Resolution:** Enforce semantic rules when flipping:
- If node C traversed → must traverse node H
- If node D traversed → must traverse node G

This yields a **feasible basis** of 4 paths (instead of 5), demonstrating that logical dependencies *reduce* the effective basis size.

#### Relationship to DD-Path Coverage

**Basis path coverage guarantees DD-path coverage** — flipping every decision ensures every decision outcome is traversed. But the converse is not true: DD-path coverage does not guarantee basis path coverage.

#### Criticisms

- The vector space metaphor is strained: what does "2*p2 − p1" (negative path) actually mean?
- Testing only the basis is **not sufficient** to catch all faults
- The baseline method requires semantic awareness to avoid infeasible paths

---

### 8.4.3 Essential Complexity

McCabe's essential complexity condenses program graphs around structured programming constructs:

> Repeatedly find structured constructs (sequence, if–then, if–then–else, case, pretest loop, posttest loop), collapse each into a single node, until no more reductions are possible.

- **Well-structured programs** always reduce to V(G) = 1
- **Unstructured programs** resist reduction — they contain "elemental unstructures" where three distinct paths exist where structured constructs would only have two

**Implication for testing:** Well-structured programs are easier to test. Essential complexity measures how much unstructured "spaghetti" remains after structured condensation — a metric for both code quality and testability.

---

## Key Takeaways

| Principle | Decision Tables (Ch. 7) | Path Testing (Ch. 8) |
|-----------|------------------------|----------------------|
| **Approach** | Specification-based (black-box) | Code-based (white-box) |
| **Strengths** | Exposes input-domain dependencies, guarantees combinational completeness | Rigorous coverage measurement, mathematical foundation |
| **Weaknesses** | Exponential growth (2ⁿ rules), overkill for simple logic | Infeasible paths, vector space metaphor strain |
| **Best for** | If–then–else–heavy logic, interdependent inputs, high cyclomatic complexity | Coverage measurement, loop testing, safety-critical code |
| **Key metric** | Rule completeness (2ⁿ) | Cyclomatic complexity V(G) = e − n + 2p |
| **Minimum bar** | Satisfy all non-impossible rules | C₁ (DD-path coverage, ~85% fault detection) |
| **When both apply** | Use decision tables to derive functional tests, then measure coverage with DD-path analysis | |

### Decision Points

- **Decision tables** work best when *many logical dependencies* exist among inputs (NextDate, Triangle). They're unnecessary when variables are independent (Commission).
- **DD-path coverage (C₁)** is the industry minimum for structural testing — mandated by ANSI 187B and used at IBM since the 1970s.
- **MCDC** is required by DO-178B for safety-critical avionics software (Level A).
- **Basis path testing** is most useful as a coverage goal, not as a standalone test derivation method — always pair with semantic feasibility analysis.

---

## References

- Jorgensen, P.C., *Software Testing: A Craftsman's Approach*, 4th Edition, CRC Press, 2014. Chapters 7–8.
- McCabe, T.J., "A Complexity Measure," *IEEE Transactions on Software Engineering*, 1976.
- McCabe, T.J., *Structured Testing: A Software Testing Methodology Using the Cyclomatic Complexity Metric*, NBS Special Publication 500-99, 1982.
- Miller, E.F., "Tutorial: Program Testing Techniques," *COMPSAC '77*, IEEE Computer Society, 1977.
- Chilenski, J.J., "An Investigation of Three Forms of the Modified Condition Decision Coverage (MCDC) Criterion," FAA Technical Report DOT/FAA/AR-01/18, 2001.
- DO-178B, *Software Considerations in Airborne Systems and Equipment Certification*, RTCA, 1992.
- Elmendorf, W.R., *Cause–Effect Graphs in Functional Testing*, IBM System Development Division, TR-00.2487, 1973.


## Related

- [[Software Testing Overview]] — All testing topics
- [[01_Testing_Fundamentals]] — Graph theory for path testing
- [[04_Data_Flow_and_Retrospective]] — Data flow testing
