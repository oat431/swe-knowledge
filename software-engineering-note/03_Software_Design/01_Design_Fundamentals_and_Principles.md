---
tags: [design-fundamentals, design-principles, software-design, swebok]
---

# Design Fundamentals and Principles

> *Source: SWEBOK v4 Chapter 03 — Software Design*

## Purpose

Software design fundamentals are the enduring principles that govern good design — independent of language, framework, or methodology. Every software engineer must internalize these as second nature.

## Design Thinking

Design is a **five-step problem-solving process**:

1. **Crystallize Purpose** — Understand what the design must accomplish. Clarify goals, constraints, and context before sketching solutions.
2. **Formulate Concept** — Generate the core organizing idea. What's the central metaphor or structure?
3. **Devise Mechanism** — Design the concrete structures, algorithms, and interactions that realize the concept.
4. **Introduce Notation** — Choose the representation: diagrams, code, models. Notation shapes thought — pick the right vocabulary.
5. **Apply** — Implement, test, and iterate. Design doesn't end at the whiteboard.

> **Design is vocabulary creation** — you're building the language to express both the problem and the solution.

## Core Design Principles

### 1. Abstraction

**Definition:** Focusing on essential features while ignoring irrelevant details.

**Levels:** System → subsystem → component → module → function. Each level hides complexity below it.

**Types:**
- **Data abstraction** — What data represents, not how it's stored (e.g., Stack operations without knowing internal array/linked list)
- **Procedural abstraction** — What a function does, not how (e.g., `sort()`, `encrypt()`)

> "The purpose of abstraction is not to be vague, but to create a new semantic level in which one can be absolutely precise." — Edsger Dijkstra

### 2. Separation of Concerns

**Definition:** Divide a system into distinct sections, each addressing a separate concern.

**Examples:**
- MVC: Model (data), View (presentation), Controller (logic)
- Layered architecture: Presentation → Business → Data
- Microservices: Each service owns one business capability

**Anti-pattern:** Mixing business logic with UI code, or database access with validation.

### 3. Modularization

**Definition:** Decompose the system into cohesive, loosely-coupled modules.

**A module should:**
- Have a single, well-defined purpose
- Be independently understandable, testable, and replaceable
- Communicate through well-defined interfaces

**Metrics:**
- **Coupling** — Degree of inter-module dependency. Minimize it.
- **Cohesion** — Degree of intra-module relatedness. Maximize it.

### 4. Coupling (Keep It Low)

| Coupling Type | Description | Quality |
|---|---|---|
| **Content** | One module modifies another's internals | 🔴 Worst |
| **Common** | Modules share global data | 🔴 Bad |
| **Control** | One module controls another via flags | 🟡 Acceptable |
| **Stamp** | Passing complex data structures | 🟡 Acceptable |
| **Data** | Passing only necessary primitive data | 🟢 Best |

### 5. Cohesion (Keep It High)

| Cohesion Type | Description | Quality |
|---|---|---|
| **Coincidental** | Elements grouped arbitrarily | 🔴 Worst |
| **Logical** | Related only by logic category (e.g., all I/O in one module) | 🔴 Bad |
| **Temporal** | Elements activated at same time (e.g., init) | 🟡 Acceptable |
| **Procedural** | Elements in a specific sequence | 🟡 Acceptable |
| **Communicational** | Elements operate on same data | 🟢 Good |
| **Sequential** | Output of one feeds input of another | 🟢 Good |
| **Functional** | All elements contribute to a single function | 🟢 Best |

### 6. Encapsulation and Information Hiding

**Definition:** Hide design decisions that are likely to change behind stable interfaces.

**What to hide:**
- Data representation (how data is stored)
- Algorithm choices (which algorithm is used)
- External dependencies (which library, which API version)
- Implementation details (anything that can change without affecting clients)

> **Parnas's Law:** Only what you hide can be changed freely. Expose it, and you're locked in.

### 7. SOLID Principles

| Principle | Description |
|---|---|
| **S**ingle Responsibility | A class should have only one reason to change |
| **O**pen/Closed | Open for extension, closed for modification |
| **L**iskov Substitution | Subtypes must be substitutable for their base types |
| **I**nterface Segregation | Many client-specific interfaces > one general-purpose interface |
| **D**ependency Inversion | Depend on abstractions, not concretions |

## Key Design Issues

Every design must address:

| Issue | Questions to Answer |
|---|---|
| **Quality Attributes** | What performance, security, reliability, usability levels are needed? |
| **Component Organization** | How are responsibilities distributed? What owns what? |
| **Crosscutting Concerns** | Logging, security, transactions — how are they applied consistently? |
| **Concurrency** | What runs in parallel? How is shared state protected? |
| **Persistence** | What data survives? Where? How is it accessed? |
| **Distribution** | What runs where? How do components communicate? |
| **Error Handling** | What can fail? How does the system detect, report, and recover? |
| **Variability** | What might change? How is the design prepared for it? |

## Essential Concepts

- **Design is an iterative, creative activity** — not a mechanical translation of requirements.
- **Abstraction is the primary tool** for managing complexity.
- **Low coupling + high cohesion** = the cardinal rule of modular design.
- **Information hiding creates flexibility** — hide what changes, expose what's stable.
- **SOLID is practical daily guidance** — not academic theory.
- **Every design decision is a trade-off** — know what you're trading.

## Related

- [[Software Design Note Overview]] — All design topics
- [[Clean Architecture/Foundations/What Is Architecture]] — Architecture as a design discipline
- [[Clean Code/Clean Code Principles]] — Practical design at the code level
