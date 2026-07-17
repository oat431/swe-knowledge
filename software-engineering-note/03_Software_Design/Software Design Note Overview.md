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

- [[Clean Code/|Clean Code]]
- [[Clean Architecture/|Clean Architecture]]
- [[Design Pattern/|Design Pattern]]
- [[Human Computer Interaction/|Human Computer Interaction]]

## What's Missing

- Software Design Fundamentals (design thinking, principles as standalone note)
- Software Design Processes (high-level vs detailed design)
- Software Design Qualities (concurrency, persistence, distribution, fault tolerance as design issues)
- Recording Software Designs (MBD, structural/behavioral descriptions)
- Design Strategies & Methods (OOD, DDD, function-oriented, component-based, event-driven)
- Design Quality Analysis (reviews, audits, metrics)

## Relationship to Other KAs

- **[[Software Requirements Overview|Software Requirements]]** — Design transforms requirements into blueprints for construction
- **[[Software Construction Overview|Software Construction]]** — Design produces the specifications that construction implements
- **[[Software Testing Overview|Software Testing]]** — Testing validates design; design provides the foundation for test strategy
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — Deployment architecture is shaped by operational needs
