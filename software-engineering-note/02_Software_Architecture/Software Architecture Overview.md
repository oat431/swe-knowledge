---
tags:
  - overview
  - swebok
  - software-architecture
  - architectural-design
  - design-patterns
---

# Software Architecture — Overview

> **Source:** SWEBOK v4 Chapter 02 *(NEW in v4)*
> **Purpose:** Cover the fundamental concepts, representation, process, and evaluation of software architectures — treating architecture as a distinct discipline from design.

## What Is This?

Software Architecture addresses the fundamental properties of a system in its environment: the structures needed to reason about it, and the decisions that shape it throughout the lifecycle. It operates at a higher level of abstraction than detailed design — defining the skeleton that constrains all downstream construction. Architecture decisions are the hardest to reverse: changing a foundational structural choice late in development is orders of magnitude more expensive than changing a module's internal logic.

SWEBOK v4 elevates Software Architecture to a full Knowledge Area (new in this edition), reflecting the discipline's maturity. Architecture exists in three senses: as a discipline (body of knowledge and practice), as a process (architecting activities), and as an outcome (architecture descriptions). The ISO/IEC/IEEE 42010 standard defines it as "the fundamental concepts or properties of a system in its environment, embodied in its elements, relationships, and design/evolution principles."

Architecture matters because it determines quality attributes — performance, scalability, reliability, maintainability, security — more than any single algorithm or data structure choice. A well-chosen architecture lets a system evolve over decades; a poor one makes every change a risk. Conway's Law reminds us that organizations design systems mirroring their communication structures, so architecture both shapes and is shaped by organizational structure.

## Knowledge Areas

### Software Architecture Fundamentals
- Three senses of architecture: discipline, process, and outcome (architecture descriptions)
- Stakeholders and concerns: each role has distinct quality attribute priorities that architecture must address
- Uses of architecture: shared understanding, analysis, reverse engineering, and communication

### Architecture Views and Viewpoints
- Views represent aspects addressing specific stakeholder concerns; viewpoints define conventions for constructing views
- Common viewpoints: logical, process, physical, development, scenarios/use cases, information, deployment
- Kruchten's 4+1 View Model; synthetic vs. projective approaches; viewtypes (module, C&C, allocation)

### Architecture Patterns, Styles, and Reference Architectures
- Styles: layered, client-server, microservices, MVC, pipes-and-filters, event-driven
- Patterns as reusable solutions at varying scales; reference architectures for domain-wide guidance
- Pattern selection driven by team size, scaling needs, deployment model, and quality attribute priorities

### Architecture Description Languages and Frameworks
- ADLs: UML (widely used), ArchiMate (enterprise), MetaH (style-specific)
- Frameworks: AUTOSAR (automotive), UAF (enterprise/SoS), ISO RM-ODP
- C4 model for developer-friendly architecture communication

### Architecture as Significant Decisions
- Architecture as a network of decisions profoundly affecting downstream development
- Rationale capture: why decisions were made, alternatives considered, trade-offs accepted
- Architectural technical debt: future cost incurred when expedient decisions compromise qualities

### Architectural Design Process
- Iterative cycle: analysis (identify ASRs) → synthesis (develop candidates) → evaluation (assess fitness)
- Architecture in different contexts: traditional lifecycle, agile, product lines, enterprise/SoS
- Conway's Law and the broader scope of architecting beyond a single design stage

### Architecture Evaluation
- ATAM (Architecture Tradeoff Analysis Method): quality-attribute utility trees and scenarios
- SAAM, QAW (Quality Attribute Workshops), SARA Report, Active Design Reviews
- Quantitative metrics: coupling, cohesion, cyclomatic complexity; DevOps metrics (deployment frequency, lead time, MTTR)

## My Notes

- [[Microservice/Microservice Overview|Microservice Overview]]
  - [[Microservice/01 Decomposition/|01 Decomposition]]
  - [[Microservice/02 Integration/|02 Integration]]
  - [[Microservice/03 Data/|03 Data]]
  - [[Microservice/04 Resilience/|04 Resilience]]
  - [[Microservice/05 Observability/|05 Observability]]
  - [[Microservice/06 Infrastructure/|06 Infrastructure]]
  - [[Microservice/07 Deployment/|07 Deployment]]

## What's Missing

- Architecture Fundamentals (three senses, stakeholders, ASRs)
- Architecture Views and Viewpoints (4+1, IEEE 42010)
- Architecture Patterns & Styles (layered, client-server, MVC, pipes-and-filters, event-driven, hexagonal)
- Architecture Description Languages (UML, ArchiMate, C4 model)
- Architecture Decisions & Technical Debt (ADRs, rationale capture)
- Architecture Design Process (analysis → synthesis → evaluation)
- Architecture Evaluation (ATAM, SAAM, fitness functions)

## Relationship to Other KAs

- **[[Software Requirements Overview|Software Requirements]]** — Architecture is driven by requirements; Architecturally Significant Requirements (ASRs) bridge requirements and architectural decisions.
- **[[Software Design Overview|Software Design]]** — Architecture defines the skeleton; design fills in the muscles. Architecture operates at higher abstraction than detailed design.
- **[[Software Construction Overview|Software Construction]]** — Architecture constrains construction: frameworks, libraries, coding standards, and build processes follow architectural decisions.
- **[[Software Testing Overview|Software Testing]]** — Architecture influences test strategy (contract testing for microservices, integration environments). Evaluation is a form of testing.
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — Deployment architecture (containers, orchestration, service mesh) is an architectural concern.
- **[[Software Quality Overview|Software Quality]]** — Quality attributes ("ilities") are the primary concerns driving architecture evaluation and trade-off analysis.
