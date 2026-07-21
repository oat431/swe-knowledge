---
tags: [modeling, uml, comet, software-models, software-engineering]
source: "Gomaa, H. (2011). Software Modeling and Design: UML, Use Cases, Patterns, and Software Architectures. Cambridge University Press. Chapters 1–5."
created: 2026-07-21
---

# 01 — Modeling Fundamentals (Gomaa Ch 1–5)

## Overview

This note consolidates the foundational concepts from Hassan Gomaa's *Software Modeling and Design*, covering the COMET method, UML notation, software life cycle models, software design and architecture concepts, and the COMET use case–based life cycle. It serves as the entry point for model-based software engineering.

---

## 1. Introduction (Ch 1)

### 1.1 What Is Software Modeling?

- **Modeling** = designing software applications before coding (OMG definition).
- Models are abstractions at some level of precision and detail, analyzed before implementation.
- Multiple views (requirements, static, dynamic) provide a complete understanding.
- A graphical language like UML helps develop, understand, and communicate these views.

### 1.2 Object-Oriented Methods and UML

- OO concepts address **modifiability, adaptation, and evolution**.
- Core principles: **information hiding, classes, inheritance**.
- UML provides a **standardized graphical language and notation** for OO models.
- UML is **methodology-independent** — it must be paired with a method (e.g., COMET).
- Modern OO methods combine:
  - **Use case modeling** — functional requirements via actors and use cases.
  - **Static modeling** — structural view (classes, attributes, relationships).
  - **Dynamic modeling** — behavioral view (object interactions, statecharts).

### 1.3 Software Architectural Design

- **Software architecture** separates overall structure (components + interconnections) from internal component details.
- **Programming-in-the-large** = component interconnection; **programming-in-the-small** = detailed internals.
- Described at multiple levels: system → subsystems → modules/components.
- Emphasis is on the **external view** of each component — its provided and required interfaces.
- **Software quality attributes** (performance, security, maintainability) must be considered at architecture time.

### 1.4 Method and Notation — Key Definitions

| Term | Definition |
|------|-----------|
| **Design notation** | Graphical or textual means of describing a design (e.g., UML, pseudocode) |
| **Design concept** | Fundamental idea applicable to designing a system (e.g., information hiding) |
| **Design strategy** | Overall plan and direction (e.g., OO decomposition) |
| **Structuring criteria** | Heuristics/guidelines for decomposing a system into components |
| **Design method** | Systematic approach describing the sequence of steps to create a design |

### 1.5 COMET Method

- **COMET** = Collaborative Object Modeling and Architectural Design Method.
- UML-based, **iterative, use case–driven, object-oriented**.
- Covers **requirements, analysis, and design modeling** phases.
- Functional requirements → actors + use cases.
- Analysis model → realizes use cases to identify participating objects and interactions.
- Design model → develops software architecture addressing distribution, concurrency, and information hiding.
- Based on design concepts: information hiding, classes, inheritance, concurrent tasks.

### 1.6 UML as a Standard

- UML 0.9 unified Booch, Jacobson (OOSE), and Rumbaugh (OMT) notations.
- UML 1.1 adopted as OMG standard (Nov 1997); UML 2.0 major revision in 2003.
- Current version: **UML 2**.
- **Model-Driven Architecture (MDA)**: UML models developed before implementation.
  - **PIM** (Platform-Independent Model) — precise architecture before platform commitment.
  - **PSM** (Platform-Specific Model) — PIM mapped to a specific platform (.NET, J2EE, Web Services, etc.).

### 1.7 Multiple Views of Software Architecture

Based on Kruchten's **4+1 view model** (use case view is the unifying "+1"):

| View | Description | UML Diagram |
|------|-------------|-------------|
| **Use case view** | Functional requirements; interactions between actors and system | Use case diagrams |
| **Static view** | Classes, associations, whole/part, generalization/specialization | Class diagrams |
| **Dynamic interaction view** | Objects and message communication; execution sequences | Communication / Sequence diagrams |
| **Dynamic state machine view** | Internal control and sequencing of control components | Statechart diagrams |
| **Structural component view** | Components, ports, provided/required interfaces | Structured class diagrams |
| **Dynamic concurrent view** | Concurrent components on distributed nodes, message communication | Concurrent communication diagrams |
| **Deployment view** | Physical configuration: components assigned to hardware nodes | Deployment diagrams |

### 1.8 Evolution of Software Modeling and Design Methods

- **1960s**: Flowcharts, subroutines for modularity.
- **1970s**: Top-down design, stepwise refinement (Dijkstra's T.H.E. OS), Structured Design (data flow → structure charts, coupling/cohesion), Jackson Structured Programming, Parnas's **information hiding**.
- **Late 1970s–80s**: MASCOT (concurrent/real-time), NRL Software Cost Reduction Method, RTSAD, DARTS, JSD (Jackson System Development — simulation of real world with concurrent tasks, middle-out design).
- **Late 1980s–90s**: OO design methods emerge — Booch, OMT (Rumbaugh), Shlaer-Mellor, Coad-Yourdon. Jacobson introduces **use cases**. Fusion, UML unification.
- **Real-time methods**: CODARTS, Octopus, ROOM (ObjecTime), Douglass's UML-RT, COMET.

### 1.9 Key Contributors

- **Parnas**: Information hiding as a design criterion — hide design decisions likely to change.
- **Booch**: OO concepts, information hiding, Ada-based design.
- **Rumbaugh (OMT)**: Static modeling (class diagrams), dynamic modeling (statecharts, sequence diagrams).
- **Jacobson**: Use case concept for functional requirements; use case–driven lifecycle.
- **Harel**: Hierarchical statecharts (state transition diagrams).
- **Chen**: Entity-relationship modeling.

---

## 2. Overview of UML Notation (Ch 2)

### 2.1 UML Diagrams Used in COMET

| Diagram | Purpose |
|---------|---------|
| **Use case diagram** | Actors, use cases, include/extend relationships |
| **Class diagram** | Classes, associations, aggregation/composition, generalization/specialization, visibility |
| **Communication diagram** | Objects, links, numbered messages (structural organization of interacting objects) |
| **Sequence diagram** | Objects, lifelines, messages ordered in time (top → bottom) |
| **State machine diagram (statechart)** | States, transitions, events, conditions, actions, composite/orthogonal states |
| **Package diagram** | Grouping of model elements (system, subsystem) |
| **Concurrent communication diagram** | Active/passive objects, async/sync message communication |
| **Deployment diagram** | Physical nodes, network connections, component-to-node assignment |
| **Composite structure diagram** | Components, ports, provided/required interfaces (UML 2) |

### 2.2 Use Case Diagrams

- **Actor**: Stick figure — external user/entity that initiates use cases.
- **Use case**: Ellipse — sequence of interactions between actor(s) and system.
- **System boundary**: Box containing use cases.
- **Relationships**: `«include»` (mandatory reuse), `«extend»` (optional extension).
- Communication associations connect actors to use cases.

### 2.3 Classes and Objects

- **Class**: Box with 1–3 compartments (name, attributes, operations).
- **Object**: Underlined name; can show `objectName : ClassName`, just `:ClassName`, or just `objectName`.
- Objects are **instances** of classes.

### 2.4 Class Diagrams — Relationships

#### Associations
- Solid line between classes; named with optional direction arrow.
- **Multiplicity**: `1` (exactly one), `0..1` (optional), `*` (zero or more), `1..*` (one or more), `m..n` (range).
- Optional stick arrow for navigability.

#### Aggregation and Composition (Whole/Part)
- **Composition** (black diamond): Strong whole/part — part cannot exist independently.
- **Aggregation** (hollow diamond): Weaker whole/part.
- Diamond touches the whole (composite/aggregate) class.

#### Generalization/Specialization (Inheritance)
- Arrow from subclass (child) to superclass (parent); arrowhead touches superclass.
- Child inherits attributes/operations; can add new ones or redefine existing ones.

#### Visibility
- `+` Public — visible outside the class.
- `-` Private — visible only within the defining class.
- `#` Protected — visible within the class and all subclasses.

### 2.5 Interaction Diagrams

#### Communication Diagram (was "Collaboration Diagram" in UML 1.x)
- Objects as boxes, lines = object interconnections.
- Labeled arrows = message name and direction.
- Numbered messages show sequence.
- `*` = iteration (message sent multiple times).
- `[Condition]` = conditional message.

#### Sequence Diagram
- Objects horizontally, time vertically (top → bottom).
- Vertical dashed **lifeline** from each object.
- Optional **activation bar** (double line) = object is executing.
- Horizontal arrows = messages; only source/destination matter.
- UML 2 adds loops and conditionals.

### 2.6 State Machine Diagrams (Statecharts)

- **State**: Rounded box.
- **Transition**: Arc with `Event [condition] / Action`.
- **Initial state**: Black circle → first state.
- **Final state**: Bull's-eye (black circle inside white circle).
- **Entry action**: Performed on state entry.
- **Exit action**: Performed on state exit.

#### Composite States
- **Sequential substates**: Statechart is in exactly one substate at a time (e.g., A1 → A2).
- **Orthogonal regions**: Statechart is in all regions simultaneously (concurrent substates, e.g., BC and BD).

### 2.7 Packages

- Folder icon (large rectangle + small tab).
- Group model elements (classes, objects, use cases).
- Can be nested; relationships: dependency, generalization.
- Stereotypes: `«system»`, `«subsystem»`.

### 2.8 Concurrent Communication Diagrams

- **Active object**: Rectangular box with two vertical parallel lines (UML 2) — has its own thread of control.
- **Passive object**: Plain rectangular box — no thread, executes only when invoked.

#### Message Types
| Type | Notation | Coupling |
|------|----------|----------|
| **Simple message** | Stick arrowhead | Undecided (analysis phase) |
| **Asynchronous (loosely coupled)** | Stick arrowhead (UML 1.4+/2) | Producer continues; FIFO queue possible |
| **Synchronous (tightly coupled) without reply** | Black arrowhead | Producer waits; no queue |
| **Synchronous with reply** | Black arrowhead (request) + dashed stick arrow (reply) | Producer waits for response |

### 2.9 Deployment Diagrams

- **Node**: Cube representing a computer node.
- **Connection**: Line with stereotype (`«local area network»`, `«wide area network»`).
- Constraint notation: e.g., `{1 node per ATM}`.
- Objects residing at each node can be depicted inside the cube.

### 2.10 UML Extension Mechanisms

1. **Stereotypes** (`«»`): Define new modeling elements derived from existing UML elements. COMET uses stereotypes extensively (e.g., `«entity»`, `«boundary»`, `«control»`, `«active object»`, `«passive object»`).
2. **Tagged values** (`{tag = value}`): Extend properties of UML elements (e.g., `{version = 1.0, author = Gill}`).
3. **Constraints** (`{condition}`): Specify conditions that must be true (e.g., `{balance >= 0}`). OCL (Object Constraint Language) available for formal constraints.

### 2.11 COMET Naming Conventions

- **Requirements model**: Use cases — uppercase initial, spaces in multiword names (e.g., `Withdraw Funds`).
- **Analysis model**: Classes — uppercase initial, no spaces in figures (e.g., `CheckingAccount`). Messages — uppercase initial, simple messages only (e.g., `Simple Message Name`).
- **Design model**: Messages/operations — lowercase first word, camelCase (e.g., `alarmMessage`, `validatePassword`).

---

## 3. Software Life Cycle Models and Processes (Ch 3)

### 3.1 Waterfall Model

- **Sequential phases**: Requirements → Analysis → Architectural Design → Detailed Design → Coding → Unit Testing → Integration Testing → System/Acceptance Testing.
- Each phase completed before the next begins (idealized).
- **Limitations**:
  - Requirements not properly tested until working system available (errors detected late, costly to fix).
  - Working system available only late in the life cycle — major design/performance problems found too late.

### 3.2 Throwaway Prototyping

- Develop prototype after preliminary requirements to **clarify user requirements**.
- Users exercise the prototype → feedback → revised requirements specification.
- Especially effective for interactive systems with complex UIs.
- Can also be used for **experimental prototyping** of design (algorithm validation, performance assessment).
- Prototype is **discarded** after requirements are clarified.

### 3.3 Evolutionary Prototyping (Incremental Development)

- Prototype **evolves** through intermediate operational systems into the delivered system.
- Subset of system working early, gradually built upon.
- First increment should test a complete path (external input → external output).
- Benefits:
  - Verifies system design early.
  - Tests whether key algorithms meet performance goals.
  - Spreads system integration over time.
  - Early operational version boosts team morale.
- Requires careful architecture design and interface specification from the start — quality cannot be an afterthought.

### 3.4 Combined Approach

1. **Throwaway prototype** to clarify requirements.
2. **Incremental development** after requirements are understood.
3. Further requirements changes possible between increments as user environment changes.

### 3.5 Spiral Model (Boehm, 1988)

- **Risk-driven** process model encompassing other life cycle models.
- Radial coordinate = cost; angular coordinate = progress.
- Four quadrants per cycle:
  1. **Define objectives, alternatives, constraints** — detailed planning.
  2. **Analyze risks** — identify project-specific risks; plan risk-mitigation activities.
  3. **Develop product** — requirements, design, or coding work.
  4. **Plan next cycle** — assess progress, plan subsequent cycle.
- **Process drivers**: Project-specific risks that require project-specific activities (e.g., throwaway prototyping if requirements unclear).

### 3.6 Unified Software Development Process (USDP / RUP)

- **Use case–driven**, UML-based, iterative.
- **Five core workflows**: Requirements → Analysis → Design → Implementation → Test.
- **Four phases**: Inception → Elaboration → Construction → Transition.
- Each phase has multiple iterations; a phase iteration ≈ a spiral model cycle.
- **Inception**: Seed idea developed to justify elaboration.
- **Elaboration**: Software architecture defined.
- **Construction**: Software built to release readiness.
- **Transition**: Software turned over to users.
- Compatible with COMET method.

### 3.7 Design Verification and Validation

- **Validation**: "Build the right system" — conforms to user's needs.
- **Verification**: "Build the system right" — each phase conforms to the previous phase's specification.
- **Software Quality Assurance**: Throwaway prototypes (validation), technical reviews (verification), requirements tracing.
- **Performance analysis**: Queuing models, simulation models, Petri nets (concurrent systems), real-time scheduling theory.

### 3.8 Software Life Cycle Activities

1. **Requirements Analysis and Specification** — produce SRS (Software Requirements Specification): external behavior only.
2. **Architectural Design** — structure system into components, define interfaces.
3. **Detailed Design** — algorithmic details, internal data structures (PDL/pseudocode).
4. **Coding** — implement components in chosen language.

### 3.9 Software Testing

| Level | Type | Description |
|-------|------|-------------|
| **Unit testing** | White box | Individual components; statement/branch coverage |
| **Integration testing** | White box | Progressively combine components; test interfaces |
| **System testing** | Black box | Integrated system against SRS; functional, load, performance testing |
| **Acceptance testing** | Black box | User organization testing at user installation |

---

## 4. Software Design and Architecture Concepts (Ch 4)

### 4.1 Object-Oriented Concepts

- **Object**: Groups data (attributes) and procedures (operations) that operate on the data. Has unique identity.
- **Class**: Template/collection of objects with the same characteristics (attributes + operations).
- **Attribute**: Data value held by an object in a class; each object has specific values.
- **Operation**: Specification of a function performed by an object; manipulates attribute values; has input/output parameters.
- **Signature**: Operation name + parameters + return value.
- **Interface**: Specification of the operations provided by a class (the visible part).

### 4.2 Information Hiding (Parnas, 1972)

- **Fundamental design concept**: Each module hides a design decision likely to change (its "secret").
- Goal: **design for change** — when internals change, only the owning module is affected.
- In OO design: encapsulation binds hidden data and the operations that access it.
- **Interface** = visible part (operations); **internals** = hidden (data structures, operation implementations).
- Other objects access hidden data only **indirectly** via operations.

#### Data Abstraction
- Encapsulating a data structure so its internal representation is hidden.
- Only access procedures (operations) directly manipulate it.

#### Stack Example — Functional vs. Information Hiding
| Approach | Stack representation | Impact of changing from array to linked list |
|----------|---------------------|---------------------------------------------|
| **Functional** | Global data structure; every module accesses directly | **Every module** that accesses the stack must change |
| **Information hiding** | Encapsulated in stack object; accessed via push/pop/empty/full | Only the **stack object internals** change; interface unchanged; other objects unaffected |

#### Designing Information Hiding Objects (Two-Step Process)
1. **Design the interface** (external view) — what operations should the object provide? (High-level design)
2. **Design the internals** — data structures and operation implementations. (Detailed design)
- Iterative process; avoid revealing all variables through get/set — that hides little.

### 4.3 Inheritance and Generalization/Specialization

- **Inheritance**: Mechanism for sharing and reusing code between classes.
- **Superclass** (parent/base): Defines common attributes and operations.
- **Subclass** (child/derived): Inherits from parent; adapts by:
  - Adding new instance variables (attributes).
  - Adding new operations.
  - Redefining (overriding) existing operations.
- Creates **generalization/specialization hierarchies** (class hierarchies).
- Example: `Account` superclass → `CheckingAccount`, `SavingsAccount` subclasses.

### 4.4 Concurrent Processing

#### Sequential vs. Concurrent Applications
- **Sequential**: One thread of control; passive objects; synchronous communication only (procedure call).
- **Concurrent**: Multiple concurrent objects, each with own thread of control; supports asynchronous message communication.

#### Concurrent Objects
- Also called: active objects, concurrent processes/tasks, threads.
- Has own thread of control; executes independently.
- Passive objects execute within the thread of the invoking concurrent object.

#### Three Cooperation Problems
1. **Mutual exclusion**: Concurrent objects need exclusive access to shared resource/device.
2. **Synchronization**: Two concurrent objects must synchronize operations.
3. **Producer/consumer**: Passing data from producer to consumer concurrent object.

#### Event Synchronization
- Source task executes `signal(event)`; destination executes `wait(event)`.
- Asynchronous — source continues after signal; destination suspends until event signaled.

#### Message Communication
| Type | Behavior | Queue |
|------|----------|-------|
| **Asynchronous (loosely coupled)** | Producer sends, continues without waiting | FIFO message queue between producer and consumer |
| **Synchronous with reply (tightly coupled)** | Producer sends, waits for reply; consumer processes and replies | No queue for that pair |
| **Synchronous without reply** | Producer sends, waits (no response expected) | No queue |

### 4.5 Design Patterns

- **Pattern**: Describes a recurring design problem, its solution, and the context in which the solution works.
- Originated in building architecture (Alexander, 1979); popularized in software by the "Gang of Four" (Gamma et al., 1995).
- A design pattern is a **microarchitecture** — larger-grained reuse than a single class.

#### Categories of Reusable Patterns

| Category | Scope | Example Source |
|----------|-------|---------------|
| **Design patterns** | Small group of collaborating objects | Gamma et al. (1995) — 23 patterns |
| **Architectural patterns** | Structure of major subsystems | Buschmann et al. (1996) |
| **Analysis patterns** | Recurring patterns in OO analysis (static models) | Fowler (2002) |
| **Product line–specific patterns** | Domain-specific (e.g., factory automation, e-commerce) | Gomaa (2005) |
| **Idioms** | Low-level, language-specific (Java, C++) | — |

### 4.6 Software Architecture and Components

- **Component**: Self-contained, usually concurrent, object with well-defined interface; capable of being reused in different applications.
- Must specify both **provided** operations and **required** operations.
- **Connector**: Encapsulates the interconnection protocol between components (e.g., shared memory buffer for same-node async communication; network messaging for distributed async communication).

### 4.7 Software Quality Attributes (Nonfunctional Requirements)

| Attribute | Definition |
|-----------|-----------|
| **Maintainability** | Capable of being changed after deployment |
| **Modifiability** | Capable of being modified during/after initial development |
| **Testability** | Capable of being tested |
| **Traceability** | Products of each phase traceable to previous phases |
| **Scalability** | Capable of growing after initial deployment |
| **Reusability** | Capable of being reused |
| **Performance** | Meets throughput and response time goals |
| **Security** | Resistant to security threats |
| **Availability** | Capable of addressing system failure |

---

## 5. Overview of COMET Method (Ch 5)

### 5.1 COMET Use Case–Based Software Life Cycle

COMET is a **highly iterative**, use case–driven, object-oriented method. The use case is the unifying thread through all phases.

```
Requirements Modeling  →  Analysis Modeling  →  Design Modeling
         ↓                                             ↓
   Throwaway Prototyping                    Incremental Software Construction
    (if requirements unclear)                         ↓
                                           Incremental Software Integration
                                                    ↓
                                               System Testing
```

#### Phase 1: Requirements Modeling
- System treated as **black box**.
- Functional requirements → **actors + use cases**.
- Narrative description of each use case.
- Throwaway prototype if requirements are not well understood.
- Nonfunctional requirements also addressed.

#### Phase 2: Analysis Modeling
- Emphasis on **understanding the problem** (problem domain).
- **Static modeling**: Problem-specific classes, attributes, relationships (class diagrams).
- **Object structuring**: Determine objects per use case using structuring criteria:
  - Entity objects, boundary objects, control objects, application logic objects.
- **Dynamic interaction modeling**: Realize use cases — show participating objects and their interactions (communication/sequence diagrams).
- **Dynamic state machine modeling**: State-dependent objects defined with hierarchical statecharts.

#### Phase 3: Design Modeling
- Emphasis on **the solution** (solution domain).
- Map analysis model to operational environment.
- Key activities:
  1. Integrate object communication model.
  2. Structure into subsystems (subsystem structuring criteria).
  3. Select architectural and design patterns.
  4. Design class interfaces (information hiding classes, operations, parameters).
  5. Structure distributed application into configurable components.
  6. Decide active vs. passive objects; structure concurrent tasks.
  7. Decide message types (async vs. sync, with/without reply).

#### Phase 4: Incremental Software Construction
- Select subset of system (use cases + participating objects).
- Detailed design → coding → unit testing of classes in the subset.
- Phased approach: gradually construct and integrate.

#### Phase 5: Incremental Software Integration
- Integration testing of each increment based on selected use cases.
- White box testing of interfaces between objects in each use case.
- Each increment is an **incremental prototype**.
- Iterate back to requirements/analysis/design if significant problems detected.

#### Phase 6: System Testing
- Black box testing against functional requirements.
- Test cases built from black box use cases.
- Covers functional, load (stress), and performance testing.

### 5.2 COMET vs. Other Processes

#### COMET + USDP (RUP)
- COMET phases map to USDP workflows:
  - Requirements Modeling → Requirements workflow.
  - Analysis Modeling → Analysis workflow.
  - Design Modeling → Design workflow.
  - Incremental Software Construction → Implementation workflow.
  - Incremental Integration + System Testing → Test workflow.
- COMET separates integration testing (dev team) from system testing (independent test team).

#### COMET + Spiral Model
- COMET can be used within the spiral model's third quadrant (develop product).
- Spiral's risk analysis (second quadrant) and cycle planning (fourth quadrant) determine iterations through COMET phases.

### 5.3 COMET Modeling Activities Summary

| Phase | Activities | Output |
|-------|-----------|--------|
| **Requirements** | Use case modeling; address nonfunctional requirements | Use case model |
| **Analysis — Static** | Define classes, attributes, relationships | Class diagrams |
| **Analysis — Object structuring** | Determine objects (entity, boundary, control, app logic) | Object model |
| **Analysis — Dynamic interaction** | Realize use cases; develop communication/sequence diagrams | Interaction diagrams |
| **Analysis — Dynamic state machine** | Develop statecharts for state-dependent objects | Statecharts |
| **Design — Architecture** | Integrate communication model; structure subsystems; select patterns | Software architecture |
| **Design — Class interfaces** | Design information hiding classes; define operations | Class interface specifications |
| **Design — Distribution** | Structure into distributed components; define message interfaces | Component model |
| **Design — Concurrency** | Structure concurrent tasks; define task interfaces | Task architecture |

### 5.4 Types of Software Architectures Covered by COMET

1. **Object-Oriented Software Architectures** (Ch 14) — sequential systems, information hiding, classes, inheritance.
2. **Client/Server Software Architectures** (Ch 15) — one server, multiple clients; client/service patterns.
3. **Service-Oriented Architectures** (Ch 16) — distributed autonomous services; brokering, discovery, transaction patterns.
4. **Component-Based Software Architectures** (Ch 17) — distributed configurable components; ports, provided/required interfaces.
5. **Real-Time Software Architectures** (Ch 18) — concurrent, state-dependent, event-driven, control patterns.
6. **Software Product Line Architectures** (Ch 19) — families of products; commonality and variability modeling.

---

## Key Takeaways

1. **COMET** is a UML-based, use case–driven, iterative method spanning requirements → analysis → design.
2. **UML** provides standardized diagrams for multiple architectural views — use case, class, interaction, state machine, deployment.
3. **Software life cycles** evolved from waterfall (sequential, risky) → prototyping → incremental → spiral (risk-driven) → USDP/RUP (iterative, phase-based).
4. **Information hiding** is the cornerstone of modifiable design: encapsulate what changes behind a stable interface.
5. **Concurrent processing** introduces three core problems: mutual exclusion, synchronization, and producer/consumer — solved via event signaling and message communication (async/sync).
6. **Design patterns** capture recurring solutions at multiple scales: idioms → design patterns → architectural patterns.
7. **Software quality attributes** (maintainability, modifiability, testability, traceability, scalability, reusability, performance, security, availability) must be addressed at architecture time.
8. The **use case** is the unifying concept — it drives requirements, analysis (object realization), design (architecture), construction (increments), and testing.

---

## Related Notes

- [[Software Engineering Models and Methods Overview]] — parent overview
- *Static Modeling* — class diagrams, associations, generalization (Gomaa Ch 7–8)
- *Dynamic Interaction Modeling* — communication/sequence diagrams (Gomaa Ch 9, 11)
- *Finite State Machines and Statecharts* — state-dependent modeling (Gomaa Ch 10)
- *Software Architectural Patterns* — structure and communication patterns (Gomaa Ch 12, Appendix A)
