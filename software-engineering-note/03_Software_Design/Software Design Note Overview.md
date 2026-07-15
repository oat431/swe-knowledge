---
tags:
  - overview
  - swebok
  - software-design
  - software-engineering
---

# Software Design — Overview

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 03 — Software Design
> **Purpose:** The transformation of requirements into implementable specifications through design principles, design strategies, and quality evaluation.

## What Is This?

Software Design is the process of defining the architecture, components, interfaces, and other characteristics of a system or component. It bridges the gap between *what* the system should do (requirements) and *how* it will do it (construction). Good design makes software easier to build, maintain, extend, and reason about; bad design makes everything harder — exponentially so as the codebase grows.

SWEBOK distinguishes three levels of design: **architectural design** (high-level structure, now its own KA in v4), **high-level design** (modules, components, their interfaces), and **detailed design** (algorithms, data structures, internal logic). This KA focuses on the latter two, plus the principles, strategies, and methods that guide design decisions.

The vault's notes in this folder come primarily from four canonical sources: Clean Architecture (Robert C. Martin), Clean Code (Robert C. Martin), Design Patterns (GoF), and Human-Computer Interaction. These cover the practical craft of software design — from SOLID principles and component architecture to UX laws and usability testing.

## The 6 Topic Areas

### 1. [[01_Design_Fundamentals]]

- Design as a problem-solving activity
- Separation of concerns, abstraction, encapsulation
- Coupling and cohesion as design quality measures
- SOLID principles: Single Responsibility, Open-Closed, Liskov Substitution, Interface Segregation, Dependency Inversion
- Design by Contract (preconditions, postconditions, invariants)
- **Book:** *Clean Code* (2009) — Robert C. Martin
- **Book:** *Clean Architecture* (2017) — Robert C. Martin

### 2. [[02_Design_Strategies]]

- Function-oriented design (structured design)
- Object-oriented design (responsibility-driven, data-driven)
- Data-structured design (Jackson's method)
- Aspect-oriented design
- Domain-Driven Design (DDD) — bounded contexts, ubiquitous language
- **Book:** *Clean Architecture* (2017) — Robert C. Martin
- **Book:** *Domain-Driven Design* (2003) — Eric Evans

### 3. [[03_Design_Patterns]]

- Creational patterns: Factory, Builder, Singleton, Prototype, Abstract Factory
- Structural patterns: Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy
- Behavioral patterns: Strategy, Observer, Command, State, Template Method, Iterator, Chain of Responsibility, Mediator, Memento, Visitor
- Pattern classification and when to apply each
- **Book:** *Design Patterns: Elements of Reusable Object-Oriented Software* (1994) — Gamma, Helm, Johnson & Vlissides (GoF)

### 4. [[04_Design_Quality_Evaluation]]

- Design reviews and walkthroughs
- Design metrics: coupling metrics, cohesion metrics, complexity metrics
- Architecture trade-off analysis
- Code smells as design quality indicators
- Refactoring as design improvement
- **Book:** *Clean Code* (2009) — Robert C. Martin
- **Book:** *Refactoring: Improving the Design of Existing Code, 2nd Ed.* (2018) — Martin Fowler

### 5. [[05_Component_Design]]

- Component principles: REP, CCP, CRP (cohesion), ADP, SDP, SAP (coupling)
- Component-based development
- Interface design and API contracts
- Dependency management and the Dependency Inversion Principle
- **Book:** *Clean Architecture* (2017) — Robert C. Martin

### 6. [[06_Human_Computer_Interaction]]

- User-centered design principles
- Gestalt laws of perception (proximity, similarity, continuity, closure, common region)
- UX laws (Fitts', Hick-Hyman, Jakob's, Miller's, Tesler's, Doherty Threshold)
- Usability heuristics (Nielsen's 10)
- UI design: typography, color, spacing, responsive design, dark mode
- User research methods: interviews, personas, journey maps, empathy maps
- Usability testing and A/B testing
- **Book:** *Don't Make Me Think, Revisited* (2014) — Steve Krug
- **Book:** *The Design of Everyday Things, Revised Ed.* (2013) — Don Norman

---

## Recommended Books (Priority Order)

| # | Book | Author(s) | Pages | Priority |
|---|------|-----------|:-----:|:--------:|
| 1 | Clean Code (2009) | Robert C. Martin | 464 | 🔴 Core |
| 2 | Clean Architecture (2017) | Robert C. Martin | 432 | 🔴 Core |
| 3 | Design Patterns (GoF) (1994) | Gamma, Helm, Johnson & Vlissides | 395 | 🔴 Core |
| 4 | The Design of Everyday Things, Revised Ed. (2013) | Don Norman | 368 | 🟡 Supplementary |
| 5 | Don't Make Me Think, Revisited (2014) | Steve Krug | 216 | 🟡 Supplementary |
| 6 | Refactoring, 2nd Ed. (2018) | Martin Fowler | 448 | 🟡 Supplementary |
| 7 | Domain-Driven Design (2003) | Eric Evans | 560 | 🟢 Deep Dive |
| 8 | A Pattern Language (1977) | Christopher Alexander et al. | 1216 | 🟢 Deep Dive |

---

## Existing Vault Coverage

This KA has **107 files** across four major book note collections:

| Subtopic | Book / Source | Files | Location |
|----------|--------------|:-----:|----------|
| Architecture Principles | Clean Architecture — Robert C. Martin | 20 | `Clean Architecture/` |
| Code Quality & Design | Clean Code — Robert C. Martin | 18 | `Clean Code/` |
| Design Patterns | Design Patterns (GoF) | 31 | `Design Pattern/` |
| HCI & UX | Various (Gestalt, UX Laws, UI Design) | 37 | `Human Computer Interaction/` |

**Total: 107 files** — This is the second most thoroughly covered KA in the vault.

---

## Relationship to Other KAs

- [[02_Software_Architecture]] — Architecture defines the high-level structure; Design refines it into implementable specifications. In v4, Architecture is its own KA (split from Design).
- [[01_Software_Requirements]] — Design transforms requirements into blueprints for construction
- [[04_Software_Construction]] — Design produces the specifications that construction implements
- [[12_Software_Quality]] — Design quality directly determines software quality (coupling, cohesion, complexity)
- [[07_Software_Maintenance]] — Good design makes maintenance cheaper; bad design makes it exponentially expensive
- [[11_Software_Engineering_Models_and_Methods]] — Modeling techniques (UML, ERD, state diagrams) are the language of design

## Related

- [[SWEBOK v4 - Overview]]
- [[Software Engineering Note Content]]
