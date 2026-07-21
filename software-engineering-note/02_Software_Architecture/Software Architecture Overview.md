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

### SAiP (Software Architecture in Practice)
- [[01_Architecture_Fundamentals]] — What architecture is, structures & views, contexts & stakeholders
- [[02_Quality_Attributes_Overview]] — QA scenarios, specifying QAs, tactics framework
- [[03_Availability_and_Interoperability]] — Fault detection/recovery, service discovery, mediation
- [[04_Modifiability_and_Performance]] — Coupling/cohesion, resource management, concurrency, caching
- [[05_Security_and_Testability]] — Authenticate/authorize, confidentiality, test interfaces, observability
- [[06_Tactics_and_Patterns]] — Full tactics catalog + architecture pattern reference
- [[07_Design_and_Documentation]] — ASRs, Attribute-Driven Design (ADD), views & beyond
- [[08_Architecture_in_Agile]] — Agile architecting, just-in-time design, guidelines
- [[09_Evaluation_and_Governance]] — Reconstruction, ATAM, management, governance
- [[10_Economics_and_Product_Lines]] — CBAM, architecture competence, software product lines
- [[11_Cloud_and_Edge_Architecture]] — Cloud-native patterns, edge systems, Metropolis model

### Microservice
- [[Microservice/Microservice Overview|Microservice Overview]]
  - [[Microservice/01 Decomposition/|01 Decomposition]]
  - [[Microservice/02 Integration/|02 Integration]]
  - [[Microservice/03 Data/|03 Data]]
  - [[Microservice/04 Resilience/|04 Resilience]]
  - [[Microservice/05 Observability/|05 Observability]]
  - [[Microservice/06 Infrastructure/|06 Infrastructure]]
  - [[Microservice/07 Deployment/|07 Deployment]]

## Relationship to Other KAs

- **[[Software Requirements Overview|Software Requirements]]** — Architecture is driven by requirements; Architecturally Significant Requirements (ASRs) bridge requirements and architectural decisions.
- **[[Software Design Overview|Software Design]]** — Architecture defines the skeleton; design fills in the muscles. Architecture operates at higher abstraction than detailed design.
- **[[Software Construction Overview|Software Construction]]** — Architecture constrains construction: frameworks, libraries, coding standards, and build processes follow architectural decisions.
- **[[Software Testing Overview|Software Testing]]** — Architecture influences test strategy (contract testing for microservices, integration environments). Evaluation is a form of testing.
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — Deployment architecture (containers, orchestration, service mesh) is an architectural concern.
- **[[Software Quality Overview|Software Quality]]** — Quality attributes ("ilities") are the primary concerns driving architecture evaluation and trade-off analysis.

---

## SWEBOK v4 Coverage Map

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 02 *(NEW in v4)* | **Last analyzed:** 2026-07-21 | **Coverage:** ~85%

| # | SWEBOK Topic | Status | Vault File(s) | Notes |
|---|---|---|---|---|
| 1 | Architecture Fundamentals | ✅ | `01_Architecture_Fundamentals.md` (28 KB) | Three senses, stakeholders, ISO 42010 |
| 2 | Views and Viewpoints | ✅ | `07_Design_and_Documentation.md` (27 KB) | 4+1 model, views & beyond, viewtypes |
| 3 | Patterns, Styles, Reference Architectures | ✅ | `06_Tactics_and_Patterns.md` (25 KB) | Full pattern catalog |
| 4 | ADLs and Frameworks | ⚠️ | `01`, `07`, overview | Mentioned but no dedicated file; ArchiMate, AUTOSAR, UAF, RM-ODP |
| 5 | Architecture as Significant Decisions | ⚠️ | `07`, `09` | Rationale and tech debt covered but scattered; not standalone KA |
| 6 | Architectural Design Process | ✅ | `07`, `08` | ADD, agile architecting, iterative cycle |
| 7 | Architecture Evaluation | ✅ | `09_Evaluation_and_Governance.md` (23 KB) | ATAM, SAAM, QAW, governance |

### Gaps to Fill

| Priority | Gap | SWEBOK Topic | What's Missing |
|----------|-----|-------------|----------------|
| 🟡 Medium | ADLs | Architecture Views & Viewpoints | Dedicated treatment of ADLs, when to use formal ADLs vs UML |
| 🟡 Medium | Architecture Frameworks | Architecture Views & Viewpoints | AUTOSAR (automotive), UAF (enterprise), ISO RM-ODP |
| 🟢 Low | Architecture as Significant Decisions (standalone) | Architecture as Decisions | ADRs, decision analysis frameworks, tech debt management strategies |
