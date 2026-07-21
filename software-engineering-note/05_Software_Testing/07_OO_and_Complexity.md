---
tags: [testing, oop, complexity, metrics, software-testing]
source: Jorgensen Ch 15-16
created: 2026-07-21
---

# Object-Oriented Testing & Software Complexity

> **Source:** Jorgensen, P.C. *Software Testing: A Craftsman's Approach* (4th Ed.), Chapters 15–16.

---

## Part I: Object-Oriented Testing (Ch. 15)

### 15.1 Issues in Testing Object-Oriented Software

Object-oriented software introduces testing challenges not present in procedural code. While early hopes for OO envisioned reusable, pre-tested components, the current consensus is that OO software has **potentially more severe testing problems** than traditional software.

#### 15.1.1 What Is an OO Unit?

Two competing definitions:

| Definition | Implication |
|---|---|
| **Method as unit** — smallest compilable/executable component | Reduces OO unit testing to traditional procedural testing; shifts burden to integration; requires mock objects |
| **Class as unit** — work of one designer, never split across developers | Solves intraclass integration; StateChart supports test case identification; clearer integration goals |

In practice, large industrial classes may require the method-as-unit approach. Some go further: an OO unit may be a **subset of class operations** or even a subclass containing only the attributes needed by a single method.

**Terminology note:** *Operation* = definition of a class function; *Method* = its implementation.

#### 15.1.2 Composition and Encapsulation

Composition (vs. decomposition) is the central OO design strategy — classes are composed with possibly unknown other units. This creates a need for **very strong unit testing** because:

- Traditional coupling and cohesion concerns still apply
- Even with good unit testing, the real burden is at **integration testing**
- Good encapsulation → loosely coupled classes → better reusability and testability

**Example: Windshield Wiper Controller** — Three possible designs illustrate the tension:

1. **Best:** Lever and dial always report their positions; wiper figures out what to do. Minimal coupling, maximum reusability.
2. **Tightly coupled:** Lever is "smart" — when in INT state, it queries dial position and tells wiper what speed. Tight coupling reduces reusability.
3. **Polling:** Wiper polls lever/dial sensors continuously. Forces continuous activity, wasteful.

> **Conclusion:** Good encapsulation results in classes that can more easily be composed (and thus reused) and tested.

#### 15.1.3 Inheritance

Inheritance complicates class-as-unit testing because standalone compilation is sacrificed. **Binder's solution: flattened classes** — expand the class to include all inherited attributes and operations.

Problems with flattened classes:
- Not part of the final system → residual uncertainty
- May need **special-purpose test methods** → test methods can themselves be faulty (false positive/negative)
- Redundant testing (e.g., `getBalance`/`setBalance` tested in both `checkingAccount` and `savingsAccount`)
- Multiple/selective inheritance makes flattening complex

#### 15.1.4 Polymorphism

Polymorphism — the same method applies to different objects. When classes are units, polymorphism is covered by class/unit testing, but the redundancy sacrifices hoped-for OO economies.

#### 15.1.5 Levels of OO Testing

| Level | What Is Tested |
|---|---|
| **Operation/Method** | Identical to procedural unit testing |
| **Class (Intraclass)** | Interactions among previously tested methods within a class |
| **Integration (Interclass)** | Interactions among previously tested classes |
| **System** | Port event level; identical to traditional system testing |

If methods are chosen as units, you get all four levels; if classes are units, you get three (method-level is folded into class testing).

#### 15.1.6 Data Flow Testing for OO

Data flow testing for procedural code is restricted to single units. OO requires extensions to address inheritance and composition. **Event-Driven Petri Nets (EDPNs)** are extended (→ EMDPNs) to describe data flow among OO operations (see §15.4.3).

---

### 15.2 Example: ooNextDate

The OO calendar application (NextDate) is decomposed into classes:

| Class | Responsibility |
|---|---|
| **CalendarUnit** (abstract) | Base class — provides `setCurrentPos()` and abstract `increment()` |
| **testIt** | Test driver — creates Date object, calls `increment()` and `printDate()` |
| **Date** | Composed of Day, Month, Year objects; orchestrates increment logic |
| **Day** | Inherits CalendarUnit; knows its month to determine last day |
| **Month** | Inherits CalendarUnit; array of month sizes; uses `isLeap()` from Year |
| **Year** | Inherits CalendarUnit; leap year calculation |

**Key observation:** Cyclomatic complexities of individual operations are uniformly low. `Date.increment` has the highest at only V(G) = 3. This is intentional — encapsulation keeps methods simple. With validity checking, complexities would increase.

---

### 15.3 Object-Oriented Unit Testing

#### 15.3.1 Methods as Units

Reduces to traditional procedural unit testing:
- Specification-based and code-based techniques apply
- Requires stubs and a driver (mock objects in OO)
- nUnit frameworks provide convenient assert mechanisms
- Encapsulation makes methods generally simple

For `Date.increment`, three equivalence classes emerge:

| Class | Condition |
|---|---|
| **D1** | `1 ≤ day < last day of the month` |
| **D2** | `day is the last day of a non-December month` |
| **D3** | `day is December 31` |

**Interface complexity is high despite low cyclomatic complexity** — `Date.increment` sends messages to 5 operations across 3 classes. This shifts burden to integration testing (intraclass and interclass).

#### 15.3.2 Classes as Units

Three views of a class:
1. **Static view** — source code; ignores inheritance (fix: flattened classes)
2. **Compile-time view** — when inheritance actually occurs
3. **Execution-time view** — when objects are instantiated (testing happens here)

Problems:
- Abstract classes cannot be instantiated
- Flattened classes must be "unflattened" after testing
- Without flattening, all ancestor classes needed to compile → SCM implications

Best suited when: little inheritance, internal control complexity exists, and the class has an interesting StateChart with substantial internal messaging.

**StateChart-based class testing coverage metrics:**
- Every event
- Every state in a component
- Every transition in a component
- All pairs of interacting states (across orthogonal components)
- Scenarios corresponding to customer-defined use cases

---

### 15.4 Object-Oriented Integration Testing

Integration testing is the least understood level for both traditional and OO software. Two steps after unit testing:
1. Restore original class hierarchy (if flattened classes were used)
2. Remove test methods (if added)

#### 15.4.1 UML Support for Integration Testing

**Collaboration diagrams** show message traffic among classes — analogous to a Call Graph. They support:

- **Pairwise integration:** Test each class with its adjacent classes (others as stubs). High stub effort, undesirable.
- **Neighborhood integration:** Start from the "ultracenter" of the graph and expand outward in concentric rings. Reduces stub effort at the expense of diagnostic precision.

**Sequence diagrams** trace execution-time paths through collaborations. They are nearly equivalent to MM-paths and provide a reasonable basis for OO integration testing.

#### 15.4.2 MM-Paths for Object-Oriented Software

> **Definition:** An OO MM-Path is a sequence of method executions linked by messages — **Method/Message Path.**

An MM-path starts with a method and ends when it reaches a method that issues no further messages — the point of **message quiescence**.

**Key properties:**
- The MM-path graph for ooNextDate has cyclomatic complexity V(G) = 23
- But a single MM-path covers many basis paths; many more are logically infeasible
- Minimum lower bound: 3 test cases (one per testIt statement)
- At minimum: need a set of MM-paths that covers **every message** (edge coverage)
- The 13 decision-table test cases from Chapter 8 constitute thorough integration test cases

#### 15.4.3 Framework for OO Data Flow Testing — EMDPNs

**Event-/Message-Driven Petri Nets (EMDPN)** extend EDPNs with message places:

> **Definition:** A quadripartite directed graph (P, D, M, S, In, Out):
> - **P** = port events
> - **D** = data places
> - **M** = message places
> - **S** = transitions (method execution paths)

**Message places capture:**
1. Output of a method execution path in the sending object
2. Input to a method execution path in the receiving object
3. Return as output of the receiving object
4. Return as input to the sending object

This framework supports:
- **Inheritance-induced data flow:** Alternating sequence of data places and degenerate method execution paths along the inheritance chain
- **Message-induced data flow:** Define/use paths across objects, e.g., `du1 = <mep3, msg2, mep5, d6, mep6, return(msg2), mep4, return(msg1), mep2>`
- **Slices?** Graph-theoretically possible but executable slices remain a stretch

---

### 15.5 Object-Oriented System Testing

System testing is (or should be) **independent of implementation paradigm**. Port input/output events are the primitives, and EDPNs express system-level threads. The difference: in OO, the system has been defined/refined with UML.

#### 15.5.1 UML-Based System Testing Levels

| Level | Coverage Criterion | Description |
|---|---|---|
| **Level 1** | System functions | Test each function in the incidence matrix; use real use cases derived from EEUCs |
| **Level 2** | All real use cases | Minimum acceptable system test coverage; customer-approved EEUCs |
| **Level 3** | FSM of GUI external appearance | State-based behavioral testing |
| **Level 4** | State-based event tables | Exhaustive per-state event testing (very detailed, likely redundant with integration tests) |

#### 15.5.2 UML Artifact Progression for System Testing

```
System Functions → High-Level Use Cases → Essential Use Cases
    → Expanded Essential Use Cases (with pre/postconditions)
    → Real Use Cases (specific data values) → System Test Cases
```

**Larman's UML approach** adds:
1. **System functions:** Evident, hidden, and frill categories
2. **Presentation layer:** GUI sketch for customer walk-through
3. **HLUCs:** Narrative descriptions
4. **EUCs:** Actor actions + system responses
5. **Detailed GUI definition:** Named controls
6. **EEUCs:** Pre/postconditions, alternative sequences, cross-references
7. **Real use cases:** Specific data replaces generic descriptions

#### 15.5.3 StateChart Caveat

StateCharts are prescribed at the class level in UML. There is **no easy way to compose StateCharts of several classes** to get a system-level StateChart. Work-around: translate each class-level StateChart into EDPNs, then compose the EDPNs.

#### 15.5.4 Incidence Matrix

Cross-reference use cases against system functions to identify coverage gaps and derive regression test suites:

| EEUC | Start | End | Input $ | Select Country | Compute | Clear | Exclusive-OR |
|------|-------|-----|---------|----------------|---------|-------|--------------|
| 1 | × | — | — | — | — | — | — |
| 2 | — | × | — | — | — | — | — |
| 3 | — | — | × | × | × | — | — |
| 5 | — | — | × | × | × | — | × |
| 6 | — | — | × | × | — | × | × |

---

## Part II: Software Complexity (Ch. 16)

### Why Measure Complexity?

Software complexity directly affects:
- **Extent of required software testing** (most direct impact)
- **Software maintenance difficulty** (program comprehension)
- **Development effort** (though often analyzed too late, on existing code)
- **Potential for improved programming practices and design**

Complexity is analyzed as a **static (compile-time) property**, not execution-time.

---

### 16.1 Unit-Level Complexity

#### 16.1.1 Cyclomatic Complexity (McCabe)

> **Definition:** For a strongly connected directed graph G:
> $$V(G) = e - n + p$$
> where $e$ = edges, $n$ = nodes, $p$ = connected regions.

For structured programs (single-entry, single-exit), the graph is not strongly connected. Two formulas:

| Graph Type | Formula |
|---|---|
| Strongly connected (edge added from sink → source) | $V(G) = e - n + p$ |
| Not strongly connected (structured program) | $V(G) = e - n + 2p$ |

**Shortcuts:**

1. **Cattle Pens Method:** Visually count enclosed regions (cycles) in the program graph. Count the "outside" region as well.

2. **Node Outdegrees Method:** Sum the reduced outdegrees of all decision nodes + 1:
   $$V(G) = 1 + \sum_{i=1}^{n} reducedOut(i)$$
   where $reducedOut(n) = outDeg(n) - 1$

   | Construct | Contribution to V(G) |
   |---|---|
   | Simple loop | 1 |
   | If-Then / If-Then-Else | 1 |
   | Case/Switch with k alternatives | $k - 1$ |

#### 16.1.2 Decisional Complexity

Cyclomatic complexity alone is insufficient — **compound conditions add hidden complexity**.

```pascal
// V(G) = 2 but hides complexity
If (a < b + c) AND (b < a + c) AND (c < a + b)
```

Expanding compound conditions:
```pascal
// V(G) = 4 — makes complexity explicit
If (a < b + c)
  If (b < a + c)
    If (c < a + b)
```

> **Rule:** Added decisional complexity = (number of simple conditions in compound expression) − 1

This avoids double-counting because the compound condition already contributes 1 to cyclomatic complexity.

#### 16.1.3 Computational Complexity — Halstead Metrics

Based on operators and operands in source code:

| Quantity | Symbol |
|---|---|
| Distinct operators | $n_1$ |
| Distinct operands | $n_2$ |
| Total operators | $N_1$ |
| Total operands | $N_2$ |

**Derived metrics:**

| Metric | Formula | Meaning |
|---|---|---|
| **Program Length** | $N = N_1 + N_2$ | Total tokens |
| **Program Vocabulary** | $n = n_1 + n_2$ | Distinct tokens |
| **Program Volume** | $V = N \cdot \log_2(n)$ | Information content |
| **Program Difficulty** | $D = (n_1 \cdot N_2) / (2 \cdot n_2)$ | Error-proneness indicator |

**Example: Two implementations of Zeller's Congruence (day-of-week calculation):**

| Metric | Version 1 | Version 2 |
|---|---|---|
| Program Length (N) | 46 | 36 |
| Vocabulary (n) | 24 | 23 |
| Volume (V) | 210.68 | 162.72 |
| Difficulty (D) | 7.500 | 6.428 |

> Halstead metrics are applied at the **DD-path level** (a DD-path contains no internal branching). The cyclomatic complexity of a program equals the cyclomatic complexity of its DD-path graph.

---

### 16.2 Integration-Level Complexity

At the integration level, concern shifts from unit correctness to **interface correctness and communication traffic**.

> **Definition:** A **call graph** is a directed graph where nodes = units (methods/procedures) and edges = messages (calls). For OO: if method A sends a message to method B → edge A→B.

**Observation:** Integration-level call graphs of OO code are generally more complex than those of functionally equivalent procedural code — but unit-level method complexity is lower. This suggests a **"law of conservation of complexity"**: complexity doesn't disappear in OO, it relocates to the integration level.

#### 16.2.1 Integration-Level Cyclomatic Complexity

Same formulas as unit-level, but applied to the **call graph** instead of the program graph. Computed from the adjacency matrix:

- Sum of row $i$ = outdegree of node $i$
- Sum of column $j$ = indegree of node $j$
- Total sum of all entries = number of edges
- Sink nodes (outdegree = 0) → add imaginary edges to make strongly connected

**Example:** Call graph with 7 nodes, 9 edges, 2 sink nodes:
$$V(G) = e - n + p = 9 - 7 + 1 = 3$$

#### 16.2.2 Message Traffic Complexity

Cyclomatic complexity at the integration level is also insufficient — not all interfaces are equal. Use an **extended adjacency matrix** where entries count the number of times a method calls another (not just 0/1).

Repeated calls to the same destination add to integration testing effort.

---

### 16.3 Software Complexity Example — IntegrationNextDate

Functional decomposition of NextDate yields:

| Unit | Cyclomatic V(G) | Added Decisional | Total |
|---|---|---|---|
| Main | 1 | 0 | 1 |
| GetDate | 2 | 0 | 2 |
| IncrementDate | 3 | 0 | 3 |
| PrintDate | 1 | 0 | 1 |
| ValidDate | 6 | +5 | **11** |
| lastDayOfMonth | 4 | 0 | 4 |
| isLeap | 4 | 0 | 4 |
| **Sum** | | | **26** |

Integration-level: V(G(call graph)) = 3, plus message traffic increment of 1 = **total integration complexity 4**.

**Grand total complexity = 26 + 4 = 30** (approx.)

---

### 16.4 Object-Oriented Complexity — CK Metrics

The **Chidamber/Kemerer (CK) metrics** are the best-known OO-specific complexity metrics:

| Metric | Name | What It Measures |
|---|---|---|
| **WMC** | Weighted Methods per Class | Sum of cyclomatic complexities of all methods in a class; indicator of development/maintenance effort |
| **DIT** | Depth of Inheritance Tree | Maximum inheritance depth; deeper trees → more inherited behavior → harder to understand |
| **NOC** | Number of Child Classes | Number of immediate subclasses; high NOC → high reuse but also more testing needed (children may break parent contracts) |
| **CBO** | Coupling Between Classes | Count of classes to which a class is coupled; high CBO → less reusability, harder testing |
| **RFC** | Response for Class | Number of methods potentially executed in response to a message; larger RFC → more complex testing/debugging |
| **LCOM** | Lack of Cohesion on Methods | Measures how well methods "belong together"; high LCOM → class should probably be split |

Some CK metrics can be derived from call graphs; others require unit-level cyclomatic complexity calculations.

---

## Key Takeaways

1. **OO testing is harder than traditional testing** — encapsulation, inheritance, and polymorphism each introduce unique challenges that shift testing burden to integration.

2. **MM-Paths** are the OO analog of traditional integration testing paths — sequences of method executions linked by messages. Message coverage (every edge in the message graph) is the minimum acceptable criterion.

3. **EMDPNs** provide a formal framework for OO data flow testing, supporting both inheritance-induced and message-induced define/use paths.

4. **System testing is paradigm-independent** — the same EDPN/thread-based approach works regardless of OO vs. procedural implementation. UML artifacts (use cases, EEUCs) provide the basis.

5. **Cyclomatic complexity V(G)** = sum of reduced outdegrees + 1. Easy to compute from source code without drawing graphs.

6. **Decisional complexity** captures the hidden complexity of compound conditions that cyclomatic complexity misses.

7. **Halstead metrics** quantify computational (textual) complexity: program length, vocabulary, volume, and difficulty.

8. **Integration-level complexity** applies cyclomatic complexity to call graphs; message traffic complexity (extended adjacency matrix) refines it.

9. **CK metrics** are the standard for measuring OO-specific complexity: WMC, DIT, NOC, CBO, RFC, LCOM.

10. **"Law of conservation of complexity":** OO doesn't eliminate complexity — it shifts it from unit-level methods to integration-level message traffic.


## Related

- [[Software Testing Overview]] — All testing topics
- [[05_Integration_and_System]] — Integration testing
- [[08_Emerging_Topics]] — Emerging testing approaches
