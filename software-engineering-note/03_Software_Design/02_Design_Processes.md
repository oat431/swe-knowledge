---
tags: [design-process, high-level-design, detailed-design, software-design, swebok]
---

# Design Processes — High-Level and Detailed Design

> *Source: SWEBOK v4 Chapter 03 — Software Design*

## Purpose

Design isn't a single step — it unfolds in layers of increasing detail. High-level design defines what the system looks like from the outside; detailed design defines how each piece works on the inside.

## High-Level Design (Outward-Facing)

**Goal:** Define the system's top-level structure, major components, and external relationships.

**Deliverables:**

| Artifact | Purpose |
|---|---|
| **Component diagram** | Major components, their responsibilities, and dependencies |
| **Interface specifications** | APIs, contracts, data formats — how components talk |
| **Data architecture** | Data stores, schemas, flows, ownership |
| **Deployment architecture** | Where each component runs, scaling strategy |
| **Technology stack decisions** | Languages, frameworks, databases, infrastructure |
| **Persistence strategy** | How data survives, what's transactional, what's cached |

**Key questions at this level:**
- What are the system's major components?
- What interfaces do they expose?
- What data formats do they use?
- How are cross-cutting concerns (auth, logging, monitoring) handled?
- What timing constraints exist?

**High-level design is sufficient when** a developer can understand the system's overall shape and start implementing without reading source code of unrelated components.

## Detailed Design (Inward-Facing)

**Goal:** Define how each component works internally — algorithms, data structures, state management, error handling.

**Deliverables:**

| Artifact | Purpose |
|---|---|
| **Class/module designs** | Internal structure of each component |
| **Algorithm descriptions** | Pseudocode or flowcharts for complex logic |
| **Data structures** | Internal representations, invariants, performance characteristics |
| **State machines** | Component states, transitions, conditions |
| **Error handling plans** | What goes wrong and how it's handled |
| **Detailed interface contracts** | Preconditions, postconditions, invariants |

**Key questions at this level:**
- What algorithms implement each operation?
- What data structures support the required operations?
- How does the component manage its internal state?
- What errors can occur, and how are they handled?
- How are edge cases addressed?

**Detailed design is sufficient when** a programmer can implement the component without making design decisions — only coding decisions remain.

## Design Process Models

### Traditional (Sequential)

```
High-Level Design → Detailed Design → Implementation → Testing
```

Best for: Well-understood domains, stable requirements.

### Iterative Design

```
High-Level → Detail → Implement → Evaluate → Refine → ...
     ↑______________________________________________|
```

Best for: Complex domains, uncertain requirements, agile teams.

### Model-Based Design (MBD)

```
Models (simulate/analyze) → Generate Code → Integrate → Test
     ↑__________________________________________|
```

Best for: Safety-critical systems, embedded systems (MATLAB/Simulink, UML tools).

### Architecture-First (Agile)

```
Architecture Skeleton → Slice-by-slice Design → Continuous Refinement
```

Best for: Most modern software projects.

## Key Principles

| Principle | Meaning |
|---|---|
| **Design to interfaces, not implementations** | Stable contracts > volatile internals |
| **Defer detailed decisions** | Decide at the last responsible moment |
| **Design for change** | Identify what's likely to change and isolate it |
| **Design for testability** | Make components independently testable |
| **Design for understanding** | A design that can't be communicated is a bad design |
| **Progressive elaboration** | Start broad, refine as understanding grows |

## Essential Concepts

- **High-level design = "what" the system is.** Detailed design = "how" each part works.
- **Interfaces are the most important design artifact** — they define the contract between components.
- **Detailed design must be precise enough for implementation** — ambiguity here causes rework later.
- **Design is iterative** — you'll revisit high-level decisions when detailed work reveals gaps.

## Related

- [[Software Design Note Overview]] — All design topics
- [[01_Design_Fundamentals_and_Principles]] — Core design principles
- [[../02_Software_Architecture/01_Architecture_Fundamentals]] — Architecture vs. design distinction
