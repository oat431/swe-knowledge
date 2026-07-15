---
tags:
  - overview
  - swebok
  - software-architecture
  - architectural-design
  - design-patterns
---

# Software Architecture — Overview

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 02 — Software Architecture *(NEW in v4)*
> **Purpose:** Define the high-level structure of a software system — its components, their relationships, and the principles guiding its design and evolution.

## What Is This?

Software architecture is the set of fundamental decisions about a system's organization: the selection of structural elements and their interfaces, the behavior as specified in collaborations among those elements, and the composition of these elements into progressively larger subsystems. Architecture is the bridge between requirements (what the system must do) and construction (how it is built line by line).

Architecture matters because it determines a system's quality attributes — performance, scalability, reliability, maintainability, security — more than any single algorithm or data structure choice. A well-chosen architecture lets a system evolve over decades; a poor one makes every change a risk. Architecture decisions are also the hardest to reverse: changing a foundational structural choice late in development is orders of magnitude more expensive than changing a module's internal logic.

SWEBOK v4 elevates Software Architecture to a full Knowledge Area (new in this edition), reflecting the discipline's maturity and its central role in modern software-intensive systems — from microservices and cloud-native platforms to embedded systems and AI pipelines.

## The 6 Topic Areas

### 1. [[Architecture Fundamentals]]
- Definitions: architecture vs. design, architectural significance
- Stakeholders and their concerns (developers, operations, business, regulators)
- Architectural drivers: quality attributes, constraints, business goals
- The role of the architect and architecture in the development lifecycle
- **Book:** *Software Architecture in Practice, 4th Ed.* — Bass, Clements & Kazman

### 2. [[Views and Viewpoints (4+1)]]
- Kruchten's 4+1 model: Logical, Development, Process, Physical + Scenarios
- Views and Beyond approach: documenting with multiple concurrent views
- Viewpoint catalogs (Rozanski & Woods, IEEE 1471/42010)
- Consistency and completeness across views
- Architecture documentation best practices
- **Book:** *Documenting Software Architectures: Views and Beyond, 2nd Ed.* — Clements et al.

### 3. [[Architectural Patterns and Styles]]
- Layered architecture, Client-Server, Pipe-and-Filter
- Microservices, Event-Driven Architecture (EDA), Service-Oriented Architecture (SOA)
- Space-Based Architecture, CQRS, Hexagonal / Ports & Adapters
- Monolith vs. distributed trade-offs
- Pattern selection criteria: team size, scaling needs, deployment model
- **Book:** *Patterns of Enterprise Application Architecture* — Martin Fowler

### 4. [[Architecture Description Languages (ADLs)]]
- Formal notations for architecture: ACME, Wright, ArchiMate, UML component/deployment diagrams
- ADLs vs. informal diagrams: precision, analysis, tool support
- The role of ADLs in model-driven development
- Industry adoption: ArchiMate for enterprise architecture, C4 model for developers
- **Book:** *Software Architecture in Practice, 4th Ed.* — Bass, Clements & Kazman

### 5. [[Architecture Evaluation (ATAM/SAAM)]]
- Architecture Tradeoff Analysis Method (ATAM): scenarios, utility trees, sensitivity/ tradeoff points
- Software Architecture Analysis Method (SAAM)
- Cost-Benefit Analysis Method (CBAM)
- Lightweight evaluation: architecture decision records (ADRs), fitness functions
- Risk identification and mitigation through architectural analysis
- **Book:** *Software Architecture in Practice, 4th Ed.* — Bass, Clements & Kazman

### 6. [[Architecture and Quality Attributes]]
- Quality attribute scenarios: stimulus, response, environment, artifact, measure
- Performance, scalability, availability, modifiability, security, testability
- Tactics: architectural strategies to achieve quality attributes (e.g., redundancy for availability, caching for performance)
- Trade-offs and sensitivity points
- Quality attribute requirements as architectural drivers
- **Book:** *Designing Data-Intensive Applications* — Martin Kleppmann

## Recommended Books (Priority Order)

| # | Book | Author(s) | Pages | Priority |
|---|------|-----------|:-----:|:--------:|
| 1 | *Software Architecture in Practice, 4th Ed.* (2021) | Len Bass, Paul Clements & Rick Kazman | 464 | 🔴 Essential |
| 2 | *Documenting Software Architectures: Views and Beyond, 2nd Ed.* (2011) | Clements et al. | 576 | 🔴 Essential |
| 3 | *Designing Data-Intensive Applications* (2017) | Martin Kleppmann | 616 | 🟡 Recommended |
| 4 | *Patterns of Enterprise Application Architecture* (2002) | Martin Fowler | 560 | 🟡 Recommended |

## Relationship to Other KAs

- **[[Software Requirements Overview|Software Requirements]]** — Architecture is driven by functional requirements and, critically, by quality attribute requirements. The transition from requirements to architecture is the most consequential design decision.
- **[[Software Design Overview|Software Design]]** — Architecture operates at a higher level of abstraction than detailed design. Architecture defines the skeleton; design fills in the muscles and organs.
- **[[Software Construction Overview|Software Construction]]** — Architecture constrains construction: frameworks, libraries, coding standards, and build processes are chosen based on architectural decisions.
- **[[Software Testing Overview|Software Testing]]** — Architecture influences test strategy (e.g., contract testing for microservices, integration test environments). Architecture evaluation is itself a form of testing.
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — Deployment architecture (containers, orchestration, service mesh) is an architectural concern. DevOps practices shape and are shaped by architecture.
- **[[Software Quality Overview|Software Quality]]** — Architecture is the primary lever for achieving quality attributes. Quality assurance activities include architecture reviews and evaluations.

## Related
- [[SWEBOK v4 - Overview]]
- [[03_Software_Design/Software Design Overview|Software Design Overview]]
- [[04_Software_Construction/Software Construction Overview|Software Construction Overview]]
