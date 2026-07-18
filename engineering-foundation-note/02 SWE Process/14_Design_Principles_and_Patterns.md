---
tags: [design, oo, patterns, uml, software-engineering, swe-foundation]
source: "SWE Theory & Practice Ch 6 — Designing the Modules"
---

# Design Principles and Patterns

> *Source: Software Engineering: Theory and Practice* Chapter 6 — Designing the Modules

## Purpose

Module-level design bridges architecture and code. Unlike architectural design (which has established styles), module-level design relies more on innovation and continuous attention to design principles.

**Core Topics:**
- Design methodology
- Six design principles
- Object-oriented design heuristics
- UML notation
- Design patterns
- Exceptions and exception handling
- Design documentation

---

## 1. Design Methodology

### Design Process Characteristics

- **No clear boundary** — architecture and module design blend together
- **Iterative process** — designers alternate between top-down, bottom-up, and outside-in
- **Refactoring** — periodically review and revise design decisions, simplify overly complex solutions

> **"Faking the Rational Design Process" (Parnas & Clements, 1986):** Even when the actual design process isn't purely top-down, documentation should follow the ideal process:
> 1. Decompose into modules
> 2. Define module interfaces
> 3. Describe inter-module dependencies
> 4. Document internal module design

### Design Style Choices

| Method | Description |
|---|---|
| Top-down | Recursive decomposition of design elements |
| Bottom-up | Apply and adapt ready-made solutions |
| Outside-in | Focus on system inputs/outputs first |
| Agile slices | Vertical slice iterative design |

---

## 2. Design Principles

> Six core principles: **Modularity, Interfaces, Information Hiding, Incremental Development, Abstraction, Generality**

### 2.1 Modularity (Separation of Concerns)

**Definition:** Separate unrelated aspects of a system so each can be studied independently (Dijkstra, 1982).

**Goal:** Each module has a single purpose, is relatively independent → easy to understand, develop, locate faults, and modify.

#### Coupling

Measures inter-module dependency:

```
Tightly coupled ← → Loosely coupled ← → Uncoupled
```

**Coupling types (worst to best):**

| Type | Description | Example |
|---|---|---|
| **Content** | One module directly modifies another | Self-modifying code |
| **Common** | Multiple modules access shared data | Global variables |
| **Control** | Passing control flags to control another module's behavior | Passing flag to decide execution path |
| **Stamp** | Passing complex data structures | Passing entire objects |
| **Data** | Passing only data values | Passing primitive parameters |

> **Coupling in OO design:** Objects are usually low-coupling, but creating shared data storage objects can still produce common coupling.

#### Cohesion

Measures how closely module-internal elements are related:

**Cohesion types (worst to best):**

| Type | Description |
|---|---|
| **Coincidental** | Module parts unrelated, grouped for convenience |
| **Logical** | Related only by code logic structure |
| **Temporal** | Used in same execution phase (e.g., initialization) |
| **Procedural** | Related by execution order |
| **Communicational** | Operate on same data set |
| **Functional** | All elements serve a single function |
| **Informational** | Functional cohesion in OO design |

> **Goal:** Informational cohesion — all attributes, methods, and operations are interdependent and essential to the object.

### 2.2 Interfaces

**Definition:** Interfaces define what services a software unit provides and how to access them.

**Interface specification elements:**
- Purpose
- Preconditions
- Protocols
- Postconditions
- Quality Attributes

**Design principles:**
- Interfaces should prevent developers from knowing and exploiting design decision details
- Keep interfaces as small and simple as possible
- Minimize assumptions and requirements on the environment

### 2.3 Information Hiding

**Core idea (Parnas, 1972):** Each software unit encapsulates a design decision that may change in the future.

**Difference from traditional decomposition:**
- Functional decomposition → all units encapsulate functions
- Data decomposition → all units encapsulate data types
- **Information hiding** → different units encapsulate different types of decisions (data formats, algorithms, hardware, protocols, etc.)

**Benefits:**
- High cohesion (each unit hides one design decision)
- Low coupling (interfaces describe externally visible behavior)
- Easy maintenance (changes localized to specific units)

### 2.4 Incremental Development

**Uses Relation (Parnas, 1978b):** Unit A "uses" unit B if and only if A needs B's correct version to complete its task.

**Uses Graph Analysis:**
- **Fan-in:** Number of units using a given unit → goal: high fan-in
- **Fan-out:** Number of units a given unit uses → goal: low fan-out

```
High fan-out (poor)         Optimized (high fan-in)
    A                           A
  B C D E F                   Utility
    G                        B C D E F
```

**Breaking cycles in Uses graphs — Sandwiching technique:**

```
Original: A ⟷ B (circular dependency)
Decomposed: A → B₂ → B₁ → A₂ (no cycle)
```

> **Goal:** Tree-shaped Uses graph → each subtree is a subsystem that can be incrementally developed and tested.

### 2.5 Abstraction

**Types:**
1. **Decomposition levels** — System → subsystem → sub-subsystem
2. **Interface specification** — Focus on external behavior, ignore internal details
3. **Multiple views** — Different structural views (runtime process vs code units)
4. **Virtual machine** — Layered architecture where each layer abstracts the layer below

**Abstraction levels example (sorting algorithm):**

```
Level 1: Sort L in nondecreasing order
Level 2: Selection sort algorithm
Level 3: Complete nested loop implementation code
```

### 2.6 Generality

**Goal:** Make software units as general as possible to increase future reuse potential.

**Methods to increase generality:**
1. **Parameterization** — Turn context-specific information into parameters
2. **Remove preconditions** — Make software work under conditions previously assumed not to occur
3. **Simplify postconditions** — Split complex units into multiple independently usable units

**Generality example:**

```
PROCEDURE SUM: INTEGER;
  POST: Returns sum of 3 global variables        ← Lowest generality

PROCEDURE SUM(a, b, c: INTEGER): INTEGER;
  POST: Returns sum of parameters

PROCEDURE SUM(a[]: INTEGER; len: INTEGER): INTEGER
  PRE: 0 ≤ len ≤ size(a)
  POST: Returns sum of a[1..len]

PROCEDURE SUM(a[]: INTEGER): INTEGER
  POST: Returns sum of all array elements         ← Highest generality
```

> **Trade-off:** Generality vs customization — sometimes reducing generality is needed to optimize performance or other quality attributes.

---

## 3. Object-Oriented Design

### OO Core Features

| Feature | Description |
|---|---|
| **Object** | Runtime entity with unique identity |
| **Composition** | Object attributes can be other objects |
| **Inheritance** | Reuse and extend existing class implementation |
| **Polymorphism** | Same message produces different behavior on different object types |

### OO Terminology

- **Object:** Runtime entity encapsulating data (attributes) and operations (methods)
- **Class:** Module implementing an abstract data type
- **Interface:** Externally accessible set of attributes and methods
- **Instance Variable:** Program variable referencing an object
- **Dynamic Binding:** Runtime determination of actual object type referenced by instance variable

### Inheritance vs Composition

| Dimension | Inheritance | Composition |
|---|---|---|
| **Encapsulation** | Subclass can directly access parent attributes | Only access components through interface |
| **Flexibility** | Determined at design time, fixed at runtime | Can dynamically replace components at runtime |
| **Understandability** | Inheritance hierarchy is clear | Runtime structure hard to visualize |
| **Performance** | No extra indirection layer | Introduces indirection, may impact performance |
| **Extension** | Can override inherited methods | Can replace compatible components |

> **Rule of thumb:** **Favor composition over inheritance** — runtime replaceability is more valuable.

### Liskov Substitution Principle (LSP)

**Three conditions for subclass to replace parent:**

1. **Method compatibility:** Subclass supports all parent methods with compatible signatures
2. **Behavior compatibility:**
   - Precondition rule: `pre_parent ⊇ pre_sub` (subclass preconditions ≤ parent)
   - Postcondition rule: `post_parent ⊇ post_sub` (subclass postconditions ≥ parent)
3. **Attribute preservation:** Subclass preserves all declared attributes of parent

**Example:**
- `BoundedStack` ❌ cannot replace `Stack` (push behavior differs)
- `PeekStack` ✅ can replace `Stack` (only adds new methods)

### Law of Demeter

**Alias:** "Don't talk to strangers" principle

**Rule:** Client code using composition should only know the composed class itself, not its component classes.

**Violating Law of Demeter:**
```java
// Bill directly traverses CustomerAccount → Sale → Item
c.s.i.printName();
c.s.i.printPrice();
```

**Following Law of Demeter:**
```java
// Bill only interacts with CustomerAccount
c.printSaleItems();
// CustomerAccount internally calls Sale.printItemList()
// Sale internally calls Item.printName()
```

**Effect:** Fewer class dependencies → fewer faults → easier to modify.

### Dependency Inversion

**Purpose:** Reverse the dependency direction between two classes.

**Three-step method:**

```
Step 1 (Original):     Client ──uses──→ Server
Step 2 (Add interface): Client ──uses──→ ClientServerInterface
Step 3 (Wrap service): NewClient + Interface ←─uses── ServerWrapper
```

**Benefit:** Both client and server depend only on interface → changing implementation doesn't require recompiling the other.

---

## 4. UML for OO Design

### UML in the Development Process

```
Requirements → Architecture → Design → Coding
    │              │            │         │
Use Case     Component    Sequence    Class
Diagrams     Diagrams     Diagrams    Diagrams
    │              │            │         │
Activity    Deployment    State     Package
Diagrams    Diagrams     Diagrams   Diagrams
```

### Class Diagram

**Design steps:**

1. **Extract nouns** → candidate classes (actors, physical objects, places, organizations, records, transactions)
2. **Group attributes and classes**
3. **Extract verbs** → behaviors/responsibilities
4. **Iterative refinement:**
   - Add abstract classes (e.g., Message, Service)
   - Remove external system classes
   - Use association classes (e.g., Discount)
   - Add multiplicity
   - Set visibility (+/−/#)

### UML Relationship Types

```
association ────  Association
composition ◆──  Composition (strong ownership)
aggregation ◇──  Aggregation (weak ownership)
generalization ◁──  Generalization/Inheritance
dependency ─ ─ →  Dependency
navigation ─ ─→  Navigation
```

### Other UML Diagrams

| Diagram Type | Purpose |
|---|---|
| **Sequence Diagram** | Temporal order of message exchanges |
| **Communication Diagram** | Message sequence between objects (numbering scheme) |
| **State Diagram** | Possible object states and transition conditions |
| **Activity Diagram** | Execution flow of processes or activities |
| **Package Diagram** | Packages and inter-package dependencies |

---

## 5. OO Design Patterns

> Design patterns are **templates** rather than ready-made solutions — must be modified and adapted for each specific use.

### Design Pattern Classification

#### Creational Patterns — Class Instantiation

| Pattern | Purpose |
|---|---|
| **Abstract Factory** | Create related object sets by theme |
| **Builder** | Separate construction from representation |
| **Factory Method** | Defer to subclass which class to instantiate |
| **Prototype** | Clone objects from prototype |
| **Singleton** | Restrict to single instance with global access point |

#### Structural Patterns — Class and Object Composition

| Pattern | Purpose |
|---|---|
| **Adapter** | Wrap incompatible interfaces |
| **Bridge** | Separate abstraction from implementation |
| **Composite** | Compose objects into tree structures |
| **Decorator** | Dynamically add/override responsibilities |
| **Façade** | Provide unified interface to simplify usage |
| **Flyweight** | Share data/state to avoid creating many objects |
| **Proxy** | Provide placeholder for another object to control access |

#### Behavioral Patterns — Class and Object Communication

| Pattern | Purpose |
|---|---|
| **Chain of Responsibility** | Delegate commands to handler chain |
| **Command** | Encapsulate actions and parameters |
| **Iterator** | Sequential access without exposing underlying representation |
| **Mediator** | Encapsulate object interactions |
| **Memento** | Restore object to previous state |
| **Observer** | Publish/subscribe, multiple objects respond to events |
| **State** | Change behavior when object state changes |
| **Strategy** | Select algorithm from family at runtime |
| **Template Method** | Define algorithm skeleton, subclasses provide concrete behavior |
| **Visitor** | Separate algorithm from object structure |

### Key Pattern Deep Dives

#### Template Method Pattern

**Scenario:** Multiple subclasses have similar but not identical method implementations.

```
Service (abstract class)
├── list_line_item()    ← Template method (prints description, price, tax)
│   └── price()         ← Abstract primitive operation
├── Refuel.price()      ← Price = gallons × unit price
├── VehicleMaint.price()← Cost = parts + labor
└── ParkingPermit.price()← Fee = duration × daily/weekly/monthly rate
```

**Core:** Parent class defines operation skeleton, subclasses provide concrete implementation at variation points.

#### Factory Method Pattern

**Scenario:** Need to decide at runtime which class to instantiate.

**Similar to Template Method:** Abstract class defines abstract construction method, subclasses override to construct concrete objects.

#### Strategy Pattern

**Scenario:** Select from multiple algorithms at runtime.

```
UserSession
├── policy: AuthenticationPolicy
└── authenticate()
        │
AuthenticationPolicy (abstract class)
├── Password.authenticate()
├── ChallengeHandshake.authenticate()
└── PublicPrivateKey.authenticate()
```

#### Decorator Pattern

**Scenario:** Dynamically extend object functionality at runtime (alternative to inheritance).

**Two key properties:**
1. Decorator is a subclass of the decorated object
2. Decorator contains a reference to the decorated object

**Benefit:** Multiple decorators can be chained.

#### Observer Pattern

**Scenario:** Need to notify multiple objects of critical events (publish/subscribe architecture).

```
Document
├── observers: DocumentObserver[]
├── register(observer)
├── notify()
│
DocumentObserver (abstract class)
├── Editor.update(info)
├── Thumbnail.update(info)
└── Toolbar.update(info)
```

**Constraint:** Requires registration/notification mechanism; observers must agree on notification interface.

#### Composite Pattern

**Scenario:** Heterogeneous, potentially recursive object collections.

```
Expr (abstract class)
├── evaluate()
├── BinaryOp: left: Expr, right: Expr
├── UnaryOp: operand: Expr
└── Variable: value
```

**Conflict:** Conflicts with Liskov Substitution Principle — subclasses may inherit meaningless operations (e.g., Variable inherits left/right operations).

---

## Core Characteristics of Design Patterns

> **Patterns are not libraries, not frameworks** — they are the **encoding** of design decisions and best practices.

**Commonalities:**
- Heavy use of interfaces, information hiding, polymorphism
- Introduce clever indirection layers
- Add extra classes, associations, and method calls
- **Trade performance and development convenience for modularity**

**Applicability:** Use patterns only when the extra flexibility is worth the extra cost.

---

## 6. Exceptions and Exception Handling

> **Exception:** An event deviating from normal execution path

**Exception handling strategy:**
- Exceptions should be handled at the appropriate level
- Should not simply terminate/restart the system at the low level
- Should use context information to determine recovery strategy

**Ariane-5 Lesson:**
- Hot standby redundancy cannot recover from software failures (all copies have same defect)
- Exception handling should not simply shut down processors
- Should collaborate with client to determine recovery strategy

---

## 7. Design Documentation

### Class Description Template

```
Class name: Refuel
Category: service
Export control: Public
Cardinality: n
Hierarchy:
  Superclasses: Service
Associations:
  <no rolename>: fuel in association updates
Operation name: price
  Public member of: Refuel
  Preconditions: gallons > 0
  Semantics: price = gallons * fuel.price_per_gallon
  Concurrency: sequential
Public interface:
  Operations: price
Private interface:
  Attributes: gallons
```

**Template includes:**
- Class position in inheritance hierarchy
- Export control (public/private/protected)
- Cardinality (expected number of objects)
- Associations with other classes
- Public/private interface for operations
- Preconditions and semantics

---

## 8. What Makes a Good Design?

### Evaluation Checklist

- [ ] Is the architecture modular, well-structured, and easy to understand?
- [ ] Is it portable to other platforms?
- [ ] Does it support reusability?
- [ ] Does it support testability?
- [ ] Does it maximize performance where appropriate?
- [ ] Does it include proper fault handling and failure prevention?
- [ ] Can it accommodate expected design changes and extensions?

---

## Key Terms

| Term | Definition |
|---|---|
| **Coupling** | Degree of inter-module dependency |
| **Cohesion** | Degree of intra-module element relatedness |
| **Information Hiding** | Encapsulating changeable design decisions |
| **Uses Relation** | Dependency relationship between units |
| **Fan-in/Fan-out** | Number of units using / used by a unit |
| **Sandwiching** | Decomposing units to break circular dependencies |
| **LSP** | Subclass should safely replace parent |
| **Law of Demeter** | Don't talk to strangers |
| **Dependency Inversion** | Reverse dependency direction through interfaces |
| **Design Pattern** | Template of design decisions and best practices |

## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[02 SWE Process/13_Software_Architecture|13_Software_Architecture]] — Architecture design (upstream)
- [[02 SWE Process/15_Coding_Standards_and_Practices|15_Coding_Standards_and_Practices]] — Implementation (downstream)
- [[02 SWE Process/12_Requirements_Engineering|12_Requirements_Engineering]] — Requirements that drive design decisions
