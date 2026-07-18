---
tags:
  - requirements
  - elicitation
  - specification
  - software-engineering
source: "SWE Theory & Practice Ch 4"
---

# 12 Requirements Engineering

> *"The hardest single part of building a software system is deciding precisely what to build."* — Fred Brooks (1987)

Requirements errors are the most expensive to fix. If it costs \$1 during requirements definition, it costs \$5 during design, \$10 during coding, \$20 during unit testing, and up to **\$200 after delivery** (Boehm & Papaccio 1988). The Standish Group found that incomplete requirements (13.1%) and lack of user involvement (12.4%) are the top causes of project failure.

---

## 4.1 The Requirements Process

A **requirement** is an expression of desired behavior — dealing with objects/entities, their states, and the functions that change them. Requirements focus on **what** the customer wants, **not how** it will be implemented.

### Four-Phase Process

```
Elicitation → Analysis → Specification → Validation
    ↓            ↓            ↓              ↓
  Collecting  Understanding  Documenting   Checking spec
  the user's  and modeling   the behavior  matches user's
  requirements desired behavior  of the SW   requirements
```

- **Elicitation** — collecting requirements from stakeholders
- **Analysis** — understanding and modeling desired behavior
- **Specification** — deciding which behavior goes into software; writing the SRS
- **Validation** — checking that the specification matches user expectations

The outcome is the **Software Requirements Specification (SRS)**, communicated to designers, testers, and maintainers.

### Agile Impact on Requirements

- **Heavy process**: requirements modeled and analyzed upfront before coding — best for large teams and safety-critical systems
- **Agile methods**: requirements gathered and implemented incrementally; early delivery of essential requirements, with emergent requirements in subsequent releases
- **XP (Extreme Programming)**: forgoes traditional documentation; requirements encoded as test cases — trade-off is difficulty making changes as requirements evolve

---

## 4.2 Requirements Elicitation

Elicitation is rarely as simple as "asking the right questions." Customers know their business but can't always describe it to outsiders; developers know solutions but not always the customer's domain.

### Stakeholders

| Stakeholder | Role |
|---|---|
| **Clients** | Pay for development; ultimate say on product |
| **Customers** | Buy the software after development |
| **Users** | Familiar with current system; know what needs improving |
| **Domain experts** | Know the problem domain (financial, meteorology, etc.) |
| **Market researchers** | Surveys on trends and customer needs |
| **Lawyers/auditors** | Government, safety, legal requirements |
| **Software engineers** | Technical/economic feasibility; cost estimation |

### Elicitation Techniques

- **Review documentation** — procedures, specs, user manuals of existing systems
- **Observe the current system** — gather objective info on how users perform tasks
- **Apprentice with users** (Beyer & Holtzblatt 1995) — learn tasks in detail as users perform them
- **Group interviews** — stakeholders inspire each other's ideas
- **Domain-specific strategies** — Joint Application Design (JAD), PIECES for information systems
- **Brainstorming** — with current and potential users
- **Reuse libraries** — templates and requirements from related systems (Volere model)

### Viewpoints Approach (Easterbrook & Nuseibeh 1996)

Rather than forcing early consistency, stakeholders' views are maintained as separate **Viewpoints** throughout development. Consistency rules are defined between viewpoints; inconsistencies are recorded and rechecked when viewpoints evolve. This avoids premature commitment to requirements or design decisions.

### Common Misunderstandings (Scharer 1990)

- Developers see users as: unable to articulate needs, politically motivated, wanting everything now
- Users see developers as: not understanding operational needs, too focused on technicalities, always late and over budget

> Good requirements analysis requires excellent **interpersonal skills** as well as solid technical skills.

---

## 4.3 Types of Requirements

### Functional Requirements

Describe required behavior in terms of activities — reactions to inputs, states before and after operations. They define the **solution space** boundary.

*Example: A payroll system states how often paychecks are issued, what input is necessary, under what conditions pay can be changed.*

### Quality Requirements (Nonfunctional Requirements)

Describe quality characteristics the software must possess:

- **Performance** — execution speed, response time, throughput
- **Usability & human factors** — training required, ease of use
- **Security** — access control, data isolation, theft prevention
- **Reliability & availability** — MTBF, restart time, backup
- **Maintainability** — error correction, future changes, portability
- **Precision & accuracy** — calculation precision

### Design Constraints

Design decisions already made that restrict solutions (platform choice, interface components, physical environment).

### Process Constraints

Restrictions on techniques or resources used to build the system (e.g., must use agile methods, documentation standards).

### Making Requirements Testable (Sidebar 4.4)

**Fit criteria** establish objective acceptance tests:

- *"Water quality records must be retrieved within 5 seconds of a request"* (testable)
- *"Water quality information must be accessible immediately"* (not testable)

Three ways to improve testability:
1. Specify quantitative descriptions for adjectives/adverbs
2. Replace pronouns with specific entity names
3. Define every noun in exactly one place

### Prioritizing Requirements

1. **Essential** — must be met
2. **Desirable** — highly desirable but not necessary
3. **Optional** — possible but could be eliminated

Prioritization helps resolve conflicts between quality requirements (e.g., maintainability vs. performance; security vs. availability; portability vs. platform-specific optimization).

### Two Kinds of Requirements Documents

| Aspect | Requirements Definition | Requirements Specification |
|---|---|---|
| **Audience** | Business (clients, customers, users) | Technical (designers, testers, PMs) |
| **Language** | Customer's vocabulary | System's interface terms |
| **Scope** | Any entity in the environment | Only entities accessible via the system interface |
| **Example** | "No one should enter the zoo without paying" | "When force is applied to unlocked turnstile, it rotates one-half turn, ending locked" |

---

## 4.4 Characteristics of Requirements

A good set of requirements should be:

1. **Correct** — conforms to stakeholder understanding
2. **Consistent** — no conflicting requirements
3. **Unambiguous** — single valid interpretation
4. **Complete** — all states, inputs, outputs, constraints described (externally and internally)
5. **Feasible** — a solution exists
6. **Relevant** — no unnecessary restrictions or "feature explosion"
7. **Testable** — suggests clear acceptance tests
8. **Traceable** — organized, uniquely labeled, with correspondence between definition and specification

> *These characteristics are the "functional and quality requirements" for a set of product requirements.*

---

## 4.5 Modeling Notations

Seven basic notational paradigms for expressing requirements:

### 1. Entity-Relationship Diagrams (ER Diagrams)

Identify **entities** (objects/classes), **attributes** (properties), and **relationships** (associations). Provide a structural overview of the problem.

- Popular because the entity set is relatively stable when requirements change
- Difficulty: deciding the right level of detail (entity vs. attribute)

**Example: UML Class Diagrams** — sophisticated ER diagrams with:
- Classes with attributes and operations
- Associations with multiplicities, role names, labels
- Aggregation ("has-a"), composition, generalization ("is-a")
- Association classes (e.g., Loan between Patron and Publication)
- Class-scope attributes/operations (underlined)

### 2. Event Traces

Graphical descriptions of event sequences between entities. Each vertical line = entity timeline; horizontal lines = events/messages. Time flows top to bottom.

- Good for understanding key scenarios
- Inefficient for complete behavior description (trace explosion)

**Example: Message Sequence Charts (MSC)** — enhanced traces with create/destroy, actions, timers, conditions, composition, and refinement facilities.

### 3. State Machines

Graphical descriptions of all dialog between system and environment. **States** = stable conditions between events; **transitions** = behavior changes triggered by events.

- Compact representation of desired event traces
- Deterministic: unique response for every state/event pair
- Hard part: deciding how to decompose behavior into states

**Example: UML Statechart Diagrams** — rich syntax including:
- **State hierarchy** — superstates collect common transitions
- **Concurrency** — multiple concurrent submachines separated by dashed lines
- **Transition labels**: `event(args) [condition] /action* ^Object.event(args)*`
- **Entry/exit actions** on states; **activities** (complex, interruptible computations)
- FIFO message queues; objects react to one message at a time

**Example: Petri Nets** — state-transition notation for concurrent activities:
- **Places** (circles) = activities/conditions; **transitions** (bars) = events
- **Tokens** enable transitions; firing removes/inserts tokens
- High-level Petri nets support structured tokens and transition actions
- Naturally models concurrent, order-independent behavior

### 4. Data-Flow Diagrams (DFDs)

Model functionality and data flow between functions:
- **Bubbles** = processes/functions transforming data
- **Arrows** = data flows
- **Data stores** (parallel bars) = persistent repositories
- **Sources/sinks** (rectangles) = actors

Strengths: intuitive high-level functionality model; domain experts find them easy to read.
Weaknesses: ambiguous when processes have multiple inputs/outputs.

**Example: Use Cases** — UML use-case diagrams depict user-initiated functionality:
- System boundary (box), actors (stick figures), use cases (ovals)
- `<<include>>` for shared subcases; `<<extend>>` for optional extensions
- Detailed as textual event traces with pre/post-conditions and variants

### 5. Functions and Relations (Formal Methods)

Mathematical models mapping inputs to outputs. Functions are by definition consistent; total functions are by definition complete.

**Example: Decision Tables** — tabular mapping of events/conditions to actions. Check completeness (all combinations covered) and consistency (no conflicting outputs).

**Example: Parnas Tables** — tabular representations of mathematical functions with row/column predicates defining cases. Advantages:
- Localized per output variable
- Easy to check completeness and consistency
- Case-by-case review

### 6. Logic

**Descriptive notations** expressing global properties and constraints (vs. operational notations showing situational behavior).

**First-order logic**: typed variables, functions, predicates, logical connectives (∧, ∨, ¬, ⇒, ≡), quantifiers (∃, ∀).

**Temporal logic** adds connectives for time-varying behavior:
- **□** f — f is true now and throughout execution
- **◇** f — f is true now or at some future point
- **○** f — f is true at the next execution point
- **f W g** — f is true until g is true (but g may never be true)

**Example: Object Constraint Language (OCL)** — mathematically precise yet readable:
- Expresses invariants, pre/post-conditions on object models
- Navigation via association paths; collection operations
- Now part of the UML standard

**Example: Z** — formal specification language:
- Structured set-theoretic definitions into abstract data type models
- **Schemas** specify system state (typed variables + invariants), initial state, and operations
- Pre/post-conditions with primed/unprimed variables for before/after values
- Supports mathematical proofs and automated checks

### 7. Algebraic Specifications

Specify operations by pairwise interactions rather than individual behavior. Execution traces are sequences of operations; axioms specify effects of operation pairs.

- No implementation bias (unlike class diagrams, DFDs, state machines)
- Operations classified as **generators** (build canonical representations), **manipulators** (return values of the type), and **queries** (return other types)
- Hard to construct concise, complete, consistent axiom sets

**Example: SDL Data** — user-defined data types in SDL with NEWTYPE, LITERALS, OPERATORS, and AXIOMS sections.

---

## 4.6 Requirements and Specification Languages

Each paradigm models problems from a different perspective. A complete specification often combines several models. Three notable combined languages:

### UML (Unified Modeling Language)

Eight graphical notations + OCL:
- **Use-case diagram** (DFD) — top-level essential functions
- **Class diagram** (ER) — entities and relationships
- **Sequence diagram** (event trace) — message traces between instances
- **Collaboration diagram** (event trace) — traces overlaid on class diagram
- **Statechart diagram** (state machine) — per-class dynamic behavior
- **OCL properties** (logic) — constraints on any model

### SDL (Specification and Description Language)

Standardized by ITU for real-time, concurrent, distributed processes:
- **System diagram** (DFD) — top-level blocks and channels
- **Block diagram** (DFD) — lower-level blocks or processes
- **Process diagram** (state machine) — state transitions with inputs, decisions, tasks, outputs
- **Data type** (algebraic) — user-defined complex types
- Accompanied by **MSC** (Message Sequence Charts)

### SCR (Software Cost Reduction)

Models requirements as mathematical function **REQ** mapping monitored variables to controlled variables. Decomposed into tabular functions (Parnas-style) organized in a DFD network. Execution propagates changes through the network in topological order.

---

## 4.7 Prototyping Requirements

When customers are uncertain ("I'll know it when I see it"), building a prototype elicits detailed feedback.

### Two Approaches

| Approach | Throwaway Prototype | Evolutionary Prototype |
|---|---|---|
| **Purpose** | Learn about the problem; never delivered | Answer questions AND be incorporated into final product |
| **Quality** | Quick-and-dirty; poorly structured; may be a facade | Must exhibit final quality requirements |
| **After use** | Thrown away; engineering starts fresh | Refined into the delivered system |

Both are called **rapid prototyping** — partial solutions built to understand requirements or evaluate design alternatives.

### When to Model vs. Prototype

- **Prototype** when: questions about user interfaces, feature prioritization, visual/spatial aspects
- **Model** when: questions about event ordering, synchronization, constraints, data flow
- **Consider** which is faster: model → develop from models, or prototype → document from prototype

---

## 4.8 Requirements Documentation

### Requirements Definition (Business Audience)

1. **Purpose and scope** — general goals, benefits, related systems, terminology
2. **Background and rationale** — why existing approach is unsatisfactory
3. **Essential characteristics** — core functionality (use-case level), quality requirements, priorities
4. **Operating environment** — hardware, software, user backgrounds, constraints
5. **Proposed solution outline** — if customer has one; evaluate if it's a constraint or overspecification
6. **Assumptions** — environmental conditions causing failure; changes that would alter requirements

### Requirements Specification (Technical Audience)

1. **System interface** — all inputs/outputs in detail (sources, destinations, formats, protocols, timing)
2. **Functional restatement** — requirements rewritten in terms of interface inputs/outputs
3. **Quality attributes** — performance, reliability, security, maintainability with measurable criteria
4. **Constraints** — design and process constraints on the implementation
5. **Validation criteria** — how to demonstrate the system meets its requirements

### Level of Specification (Sidebar 4.6)

- No universally correct level — experienced customers prefer high-level; less experienced prefer detail
- Each clause should contain **only one requirement**
- Avoid cross-references between requirements
- Collect similar requirements together
- Avoid overspecifying (mandating particular solutions) or underspecifying (missing operating environment, maintenance, fault tolerance)

---

## Key Takeaways

- Requirements focus on **what**, not **how** — premature solution discussion constrains design flexibility
- Multiple stakeholder viewpoints must be reconciled; conflicts are natural and require negotiation
- Requirements should be **testable** with quantified fit criteria
- Different modeling notations serve different purposes: structural (ER), behavioral (state machines), functional (DFDs), formal (logic/Z)
- No single notation suffices; practical specifications combine multiple paradigms
- Prototyping bridges the gap when customers "know it when they see it"
- Two documents serve different audiences: requirements definition (business) and requirements specification (technical)
- Correct, consistent, unambiguous, complete, feasible, relevant, testable, traceable — the quality checklist for requirements


## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[02 SWE Process/10_SE_Fundamentals_and_Process|10_SE_Fundamentals_and_Process]] — Software process models and life cycle
- [[02 SWE Process/13_Software_Architecture|13_Software_Architecture]] — Architecture design from requirements
- [[02 SWE Process/16_Testing_Strategies|16_Testing_Strategies]] — Validation and verification of requirements
