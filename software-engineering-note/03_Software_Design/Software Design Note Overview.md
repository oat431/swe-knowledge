---
tags:
  - overview
  - swebok
  - software-design
  - software-engineering
---

# Software Design — Overview

> **Source:** SWEBOK v4 Chapter 03
> **Purpose:** Transform requirements into implementable specifications through design principles, strategies, methods, and quality evaluation.

## What Is This?

Software Design is the process of defining the architecture, components, interfaces, and other characteristics of a system. It bridges the gap between *what* the system should do (requirements) and *how* it will do it (construction). Good design makes software easier to build, maintain, extend, and reason about; bad design makes everything harder — exponentially so as the codebase grows.

SWEBOK v4 distinguishes three stages of design: **architectural design** (system-wide fundamentals, now its own KA), **high-level design** (outward-facing: top-level structure, components, interfaces, data formats), and **detailed design** (inward-facing: algorithms, data structures, module internals). Design is fundamentally a problem-solving and vocabulary-creation activity — creating the language to express both problem and solution.

The chapter covers foundational principles (abstraction, separation of concerns, encapsulation, coupling/cohesion, SOLID), multiple design strategies (OOD, DDD, event-driven, component-based), recording approaches (UML, design patterns, DSLs, design rationale), and quality evaluation techniques (reviews, metrics, static analysis, verification vs. validation).

## Knowledge Areas

### Software Design Fundamentals
- Design thinking as a five-step problem-solving process: crystallize purpose, formulate concept, devise mechanism, introduce notation, apply
- Core principles: abstraction, separation of concerns, modularization, encapsulation/information hiding, coupling (low) and cohesion (high)
- Key issues every design must address: quality attributes, component organization, and crosscutting aspects (properties affecting systemic behavior)

### Software Design Processes
- High-level design (outward-facing): top-level structure, major components, external interfaces, data formats, timing, persistence strategies
- Detailed design (inward-facing): algorithms, data structures, module states, scope/visibility, user interfaces, inter-module interactions
- Detailed design must be sufficient for programmers to code without reading source of peer components

### Software Design Qualities
- Concurrency, control/event handling, data persistence, distribution of components
- Error handling, exception handling, and fault tolerance
- Integration/interoperability, security/safety assurance, and variability (product lines via feature models)

### Recording Software Designs
- Model-Based Design (MBD): shift from documents to tool-enabled models supporting simulation, analysis, and CI
- Structural descriptions: class/component/deployment diagrams, ERDs, CRC cards, IDLs
- Behavioral descriptions: activity/interaction/state diagrams, DFDs, decision tables, pseudocode
- Design patterns (GoF 23): creational, structural, behavioral — reusable solutions as shared vocabulary

### Software Design Strategies and Methods
- General strategies: divide-and-conquer, stepwise refinement, top-down vs. bottom-up, iterative/incremental
- Object-Oriented Design: objects, methods, attributes, inheritance, polymorphism, SOLID principles
- Other methods: function-oriented, data-centered, user-centered, component-based, event-driven, aspect-oriented, constraint-based, domain-driven design

### Software Design Quality Analysis and Evaluation
- Design reviews and audits (informal, rigorous, scenario-based, requirements tracing)
- Quality attributes: runtime (performance, security, availability, usability) and non-runtime (modifiability, portability, testability)
- Measures and metrics: function-based (structure charts) and OO-based (coupling, cohesion, complexity)

## My Notes

### SWEBOK Design Foundations
- [[01_Design_Fundamentals_and_Principles]] — Abstraction, coupling/cohesion, SOLID, design thinking
- [[02_Design_Processes]] — High-level vs detailed design, iterative design, MBD
- [[03_Design_Qualities]] — Concurrency, persistence, distribution, error handling
- [[04_Recording_Software_Designs]] — MBD, UML, structural/behavioral descriptions, ADRs
- [[05_Design_Strategies_and_Methods]] — OOD, DDD, event-driven, function-oriented, component-based
- [[06_Design_Quality_Analysis]] — Reviews, inspections, metrics, static analysis

### Design Practice
- [[Clean Code/|Clean Code]]
- [[Clean Architecture/|Clean Architecture]]
- [[Design Pattern/|Design Pattern]]
- [[Human Computer Interaction/|Human Computer Interaction]]

## Relationship to Other KAs

- **[[Software Requirements Overview|Software Requirements]]** — Design transforms requirements into blueprints for construction
- **[[Software Construction Overview|Software Construction]]** — Design produces the specifications that construction implements
- **[[Software Testing Overview|Software Testing]]** — Testing validates design; design provides the foundation for test strategy
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — Deployment architecture is shaped by operational needs

---

## SWEBOK v4 Coverage Map

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 03 | **Last analyzed:** 2026-07-21 | **Coverage:** ~55%

| # | SWEBOK Topic | Status | Vault File(s) | Notes |
|---|---|---|---|---|
| 1 | Design Fundamentals | ⚠️ | `01_Design_Fundamentals_and_Principles.md` (6 KB) | Covers abstraction, SOLID, coupling/cohesion but critically thin |
| 2 | Design Processes | ⚠️ | `02_Design_Processes.md` (4.6 KB) | High-level vs detailed design mentioned but very thin |
| 3 | Design Qualities | ⚠️ | `03_Design_Qualities.md` (5.5 KB) | 8 quality topics present but brief |
| 4 | Recording Software Designs | ⚠️ | `04` + `Design Pattern/` subfolder | MBD, UML, patterns via subfolder; DSLs and design rationale thin |
| 5 | Design Strategies and Methods | ✅ | `05_Design_Strategies_and_Methods.md` (5.8 KB) | OOD, DDD, event-driven, component-based, AOD all present |
| 6 | Design Quality Analysis | ⚠️ | `06_Design_Quality_Analysis.md` (6 KB) | Reviews, metrics, static analysis mentioned but thin |

### Gaps to Fill

| Priority | Gap | SWEBOK Topic | What's Missing |
|----------|-----|-------------|----------------|
| 🔴 High | DEPTH across all 6 main files | All | Files are 4-6 KB each (10x smaller than other KAs); needs 3-5x expansion |
| 🟡 Medium | Design Rationale | Recording Designs | ADRs, why-decisions, alternatives, trade-offs, rejected options |
| 🟡 Medium | Model-Based Design (MBD) | Recording Designs | Evolution from document-based to model-based artifacts |
| 🟡 Medium | DSLs | Recording Designs | Domain-Specific Language design and application |
| 🟡 Medium | Variability & Feature Models | Design Qualities | Product line variability, feature modeling |
| 🟢 Low | Aspect-Oriented Design (AOD) | Design Strategies | Crosscutting concerns implementation via aspects |
