---
title: Real-Time & Product Lines — Gomaa Ch 18-20
tags: [modeling, real-time, product-lines, quality-attributes, software-engineering]
source: "Gomaa, H. (2011). Software Modeling and Design: UML, Use Cases, Patterns, and Software Architectures. Cambridge University Press. Chapters 18–20."
created: 2026-07-21
aliases:
  - Gomaa Ch18-20
  - Real-Time Software Architectures
  - Software Product Lines
  - Software Quality Attributes
---

# Real-Time & Product Line Architectures — Gomaa Ch 18–20

## Overview

Chapters 18–20 of Gomaa cover three advanced topics in software architecture design: **(Ch 18) concurrent and real-time software architectures** (task structuring, control patterns, communication/synchronization), **(Ch 19) software product line architectures** (variability modeling, feature-driven evolution), and **(Ch 20) software quality attributes** (maintainability, modifiability, testability, traceability, scalability, reusability, performance, security, availability).

---

## Chapter 18: Designing Concurrent and Real-Time Software Architectures

### 18.1 Concepts, Architectures, and Patterns

- **Real-time architectures** are concurrent architectures dealing with multiple independent streams of input events.
- They are typically **state-dependent** with either centralized or decentralized control.
- During concurrent software design, a system is structured into **concurrent tasks** (active objects with a single thread of control), and interfaces between tasks are defined.
- **Task structuring criteria** are heuristics that capture expert knowledge for mapping analysis model objects to concurrent design tasks.
- Concurrent tasks participate in architectural patterns (Layered, Client/Service) and control patterns.

### 18.2 Characteristics of Real-Time Systems

- Real-time systems are **concurrent systems with timing constraints**.
- They interface with **sensors** (input) and **actuators** (output) via specialized device drivers.
- Event arrival rates are often **unpredictable**, and input loads vary significantly.
- **Hard real-time**: time-critical deadlines — missing a deadline causes catastrophic failure.
- **Soft real-time**: missing deadlines is undesirable but tolerable.

### 18.3 Control Patterns

| Pattern | Description |
|---|---|
| **Centralized Control** | One control component executes a statechart, receives input events, and triggers actions/outputs. (e.g., Microwave Oven Control with one MicrowaveControl component) |
| **Distributed Control** | Multiple control components, each with its own state machine. Peer-to-peer communication for event notification. No single component has overall control. |
| **Hierarchical Control** | A coordinator component provides high-level control, issuing commands to distributed controllers. Distributed controllers handle low-level control and report status back. |

### 18.4 Concurrent Task Structuring

- A **concurrent task** is an active object with one thread of control (process or thread).
- Analysis model objects are evaluated: which can execute concurrently? Which must be sequential?
- Objects in the analysis model are categorized with **two stereotypes**: the role criterion (from object structuring) and the concurrency criterion (event-driven, periodic, demand-driven).
- A composite task can contain **passive objects** (operations execute sequentially within the task's thread).

### 18.5 I/O Task Structuring Criteria

| Type | Activation | Use Case |
|---|---|---|
| **Event-Driven I/O** | Interrupt from event-driven (interrupt-driven) I/O device | Sensors that generate interrupts; external systems via proxy tasks |
| **Periodic I/O** | Timer event at regular intervals | Passive sensors needing periodic sampling (digital/analog sensors) |
| **Demand-Driven I/O** | Message/request from another task | Overlapping computation with I/O; typically for output devices |

**Event-Driven Proxy Task**: interfaces to an external system (not a device) via messages.

### 18.6 Internal Task Structuring Criteria

| Type | Stereotype | Activation |
|---|---|---|
| **Periodic** | `«periodic»` | Timer event — executes activity, waits for next timer |
| **Demand-Driven** | `«demand»` | Arrival of internal message/event from another task (aperiodic) |
| **Control** | `«state-dependent control»` | Demand-driven; executes a sequential statechart; one task per control object |
| **User Interaction** | `«event driven» «user interaction»` | Sequential user activity; one task per sequential user activity (one per window) |

### 18.7 Developing the Concurrent Task Architecture

Apply criteria in order:

1. **I/O tasks** — determine event-driven, periodic, or demand-driven based on I/O device characteristics.
2. **Control tasks** — state-dependent control objects and coordinators → demand-driven control/coordinator tasks.
3. **Periodic tasks** — internal periodic activities → periodic tasks.
4. **Other internal tasks** — activated by internal events → demand-driven tasks.

**Mapping summary (Table 18.1)**:

| Analysis Model (Object) | Design Model (Task) |
|---|---|
| User interaction | Event-driven user interaction |
| Input/Output (I/O) | Event-driven I/O; Periodic I/O; Demand-driven I/O |
| Proxy | Event-driven proxy |
| Timer | Periodic timer |
| State-dependent control | Demand-driven state-dependent control |
| Coordinator | Demand-driven coordinator |
| Algorithm | Demand-driven algorithm; Periodic algorithm |

Result: **initial concurrent communication diagram** showing all tasks and simple message interfaces. Then refine with task communication/synchronization.

### 18.8 Task Communication and Synchronization

#### 18.8.1 Asynchronous (Loosely Coupled) Message Communication
- Producer sends message, continues without waiting.
- Best for event-driven input tasks that must service interrupts quickly.
- Consumer can wait on a queue from multiple sources.

#### 18.8.2 Synchronous (Tightly Coupled) with Reply
- Producer sends message to consumer, **waits for reply**.
- Used when producer needs response before continuing (e.g., Vehicle Control → Motor Interface: `startMotor` / `started`).

#### 18.8.3 Synchronous (Tightly Coupled) without Reply
- Producer sends message, **waits for acceptance** (not reply).
- Acts as a **brake** on the producer — prevents queue build-up.
- Enables overlapping computation with I/O.

#### 18.8.4 Event Synchronization

| Event Type | Description |
|---|---|
| **External event** | Hardware interrupt from I/O device; activates event-driven task |
| **Timer event** | Generated by external timer; activates periodic task |
| **Internal event** | Synchronization signal between tasks (no data); e.g., `partReady` / `partCompleted` |

#### 18.8.5 Task Interaction via Information Hiding Object
- Tasks access passive objects via **operation calls** (synchronous message notation).
- Different semantics from synchronous message between concurrent tasks — this is a simple method call in the caller's thread.

#### 18.8.6 Revised Concurrent Communication Diagram
- Updated from initial diagram to show specific interface types (async, sync, event signal, operation call).

### 18.9 Task Interface Specification (TIS) and Task Behavior Specification (TBS)

**TIS** defines:
- Name, information hidden, structuring criteria (role + concurrency), assumptions, anticipated changes
- Task interface: message inputs/outputs (type, name, parameters), events signaled, external I/O, passive objects referenced, errors detected

**TBS**: describes event sequencing logic — how the task responds to each input and what output results.

### 18.10 Implementation in Java

```java
public class ATMControl extends Thread {
    public void run() {
        while (true) {
            // task body — wait for external event or message
        }
    }
}
```

Tasks are implemented as Java `Thread` subclasses with a `run()` method containing the event loop.

---

## Chapter 19: Designing Software Product Line Architectures

### 19.1 Evolutionary Software Product Line Engineering

- A **Software Product Line (SPL)** is a family of software systems sharing common functionality with variable functionality.
- SPL engineering involves developing **requirements, architecture, and component implementations for the family**, from which individual products are derived/configured.
- The **PLUS** (Product Line UML-based Software engineering) method extends COMET with variability management across all modeling views.
- The process is **highly iterative**, eliminating the traditional distinction between development and maintenance.

### 19.2 Requirements Modeling for SPLs

#### Feature Modeling
- **Features** are used alongside use cases for product line engineering.
- Feature categories: **common** (all members), **optional** (some members), **alternative** (choice of features).
- **Feature dependencies**: `requires` relationships (e.g., Greeting requires Language).
- **Feature groups**: constraints on feature selection — `«exactly-one-of feature group»` for mutually exclusive features, `«zero-or-one-of feature group»`.
- **Parameterized features**: have configurable default values (e.g., Max PIN Attempts default = 3).
- A **feature/use case relationship table** maps each feature to the use case or variation point it relates to.

#### Use Cases and Variation Points
- **Kernel use cases**: required by all members.
- **Optional use cases**: only in some members.
- **Variation points**: locations within a use case where variation can occur (e.g., display language, greeting message).

### 19.3 Analysis Modeling for SPLs

#### 19.3.1 Static Modeling
- Classes get **two orthogonal stereotypes**: role (entity, control, boundary) + **reuse category** (kernel, optional, variant).
- **Kernel classes**: required by all members (e.g., ATM Card, ATM Cash).
- **Optional classes**: correspond to optional features (e.g., ATM Greeting).
- **Variant classes**: alternative implementations (e.g., English Display Prompts, French Display Prompts) — use an abstract kernel superclass with variant subclasses.

#### 19.3.2 Dynamic Interaction Modeling — Evolutionary Dynamic Analysis
- Start with **kernel system** (minimal member) using kernel use cases.
- Iteratively add **optional/variant objects** by analyzing each variable use case and variation point.
- New optional/variant objects are added; existing kernel objects may need adaptation.
- Result captured in a **feature/class dependency table**: for each feature, lists the classes that realize it, their reuse category, and class parameters.

### 19.4 Dynamic State Machine Modeling for SPLs

- Two approaches: **specialization** (inheritance) vs. **parameterization**.
- **Inheritance leads to combinatorial explosion** — N features → 2^N variant state machines.
- **Parameterized state machines** are more manageable:
  - Feature-dependent states, events, and transitions.
  - Boolean **feature conditions** guard optional transitions and actions.
  - If feature condition is `True` (feature selected) → transition/action enabled; `False` → disabled.

Example: `Minute Pressed [minuteplus]` — only transitions if the Minute Plus feature is selected.

### 19.5 Design Modeling for SPLs

#### 19.5.1 Component-Based Software Architectures
- Design **kernel, optional, and variant components**.
- **Plug-compatible components**: all variants implement the same provided interface, so the producer's required interface never changes.
- **Component interface inheritance**: when plug-compatibility isn't feasible, specialize interfaces via inheritance.
- Example: `CustomerInteraction` connects to any `DisplayPrompts` variant (English, French, Spanish, German) through the common `IDisplayPrompt` interface.

#### 19.5.2 Architectural Patterns for SPLs
- **Layered pattern**: supports extension/contraction — add/remove components at higher layers.
- **Client/Service pattern**: services can evolve; new services can be discovered.
- **Broker/Discovery/Subscription-Notification**: decouple components, enabling independent evolution.
- Goal: design architectures that support **variability and evolution**.

---

## Chapter 20: Software Quality Attributes

Software quality attributes are **non-functional requirements** that profoundly affect product quality. Some are purely software in nature (maintainability, modifiability, testability, traceability); others are system quality attributes needing both hardware and software (performance, availability, security).

### 20.1 Maintainability
- **Definition**: The extent to which software is capable of being changed after deployment.
- Reasons for change: fixing remaining errors, addressing performance issues, changing requirements.
- COMET supports maintainability through comprehensive **design documentation**, **stereotypes** capturing design decisions, and **traceability** from use cases to implementation.
- Example: Banking System internationalization — add prompt table with language-specific prompts, minimizing changes to `CustomerInteraction` object.

### 20.2 Modifiability
- **Definition**: The extent to which software is capable of being modified during and after initial development.
- Based on Parnas's **information hiding** principle — each module hides a secret that can change independently.
- COMET supports modifiability through:
  - Encapsulating FSMs in separate classes.
  - Encapsulating external interfaces in boundary classes.
  - Encapsulating data structures in data abstraction classes.
  - Component-based architecture enabling deployment to different configurations.
- Example: `DisplayPrompts` abstract superclass with language-specific subclasses (English, French, Spanish, German) — extendable to new languages.

### 20.3 Testability
- **Definition**: The extent to which software is capable of being tested.
- Integration with COMET's phases:
  - **Requirements phase**: functional (black-box) test cases from use case descriptions (main + alternative sequences).
  - **Architectural design**: integration test cases via scenario-based testing from interaction diagrams.
  - **Detailed design/coding**: white-box test cases (code coverage, decision coverage).
- Example: Validate PIN test — simulate card reader and user PIN input, test against Debit Card table.

### 20.4 Traceability
- **Definition**: The extent to which products of each phase can be traced back to products of previous phases.
- COMET is use case–based: each requirement traces from use case → interaction diagram → software architecture → implementation.
- **Impact analysis**: determine the effect of a requirement change by following the trace.
- Example: Adding multilingual prompts — trace from `ValidatePIN` use case → `CustomerInteraction` → `DisplayPrompts` object.

### 20.5 Scalability
- **Definition**: The extent to which the system is capable of growing after initial deployment.
- Distributed component-based architectures scale better than centralized designs.
- Multiple instances of components can be deployed to different nodes.
- COMET supports scalability through **distributed component-based** and **service-oriented architectures**.
- Example: Emergency Monitoring System — add more sensors, service instances (Monitoring Data, Alarm, Reporting), operator clients.

### 20.6 Reusability
- **Definition**: The extent to which software is capable of being reused.
- **Architecture reuse** (large-grained) has greater potential than individual component reuse.
- **Software Product Line architecture** explicitly captures commonality and variability for a family of products.
- **Application engineering**: tailoring/configuring the product family architecture to create a specific application.
- PLUS extends COMET for designing SPL architectures.

### 20.7 Performance
- **Definition**: Concerned with meeting performance goals (throughput, response times).
- Methods: **queuing modeling**, **simulation modeling**, **real-time scheduling**, **event sequence analysis**.
- Real-time scheduling is especially appropriate for **hard real-time systems** with deadlines.
- Example: Banking System — queuing model for response time and transaction throughput under various workloads; compare sequential vs. concurrent designs, single vs. dual processor configurations.

### 20.8 Security

**Potential Threats**:

| Threat | Countermeasure |
|---|---|
| System penetration | Encrypt messages between client and server |
| Authorization violation | Maintain access logs; audit trails |
| Confidentiality disclosure | Access control with privilege levels |
| Integrity compromise | Access control preventing unauthorized data changes |
| Repudiation | Transaction logs for verification |
| Denial of service | Intrusion detection and rejection |

- COMET extends use case descriptions to include non-functional/security requirements.

### 20.9 Availability
- **Definition**: Addresses system failure and its impact.
- Solutions:
  - **Hot standby**: backup server ready for rapid deployment (e.g., Bank Server in centralized client/server).
  - **Distributed system without single point of failure**: replicate components across nodes; operate in **degraded mode** on partial failure.
- COMET supports availability through distributed designs with replicated control, data, and services.
- Example: Emergency Monitoring System — multiple instances of each service and client component; local network failures don't disable the entire system.
- **Triple redundancy and voting systems** for the most critical (and expensive) applications.

---

## Summary Table

| Chapter | Focus | Key Concepts |
|---|---|---|
| **Ch 18** | Real-Time Architectures | Task structuring (event-driven, periodic, demand-driven), control patterns (centralized/distributed/hierarchical), task communication (async, sync+reply, sync−reply, event sync), TIS/TBS |
| **Ch 19** | Product Line Architectures | Feature modeling (common/optional/alternative), PLUS method, evolutionary dynamic analysis, parameterized statecharts, plug-compatible components, architectural patterns for SPLs |
| **Ch 20** | Quality Attributes | Maintainability, modifiability, testability, traceability, scalability, reusability, performance, security, availability — COMET support for each |


## Related

- [[Software Engineering Models and Methods Overview]] — All models and methods topics
- [[04_Architectural_Design]] — Architecture patterns
- [[05_Distributed_and_Component]] — Distributed architectures
