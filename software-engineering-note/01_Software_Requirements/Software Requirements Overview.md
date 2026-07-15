---
tags:
  - overview
  - swebok
  - software-requirements
  - requirements-engineering
---

# Software Requirements — Overview

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 01 — Software Requirements
> **Purpose:** Define, document, and maintain the requirements for a software system throughout its lifecycle.

## What Is This?

Software Requirements engineering is the foundation upon which all successful software systems are built. It encompasses the process of discovering, analyzing, documenting, validating, and managing the needs and constraints that a software system must satisfy. Poor requirements are consistently ranked among the top causes of project failure — studies repeatedly show that fixing a requirements defect discovered after delivery costs 50–200× more than catching it during elicitation.

Requirements engineering bridges the gap between stakeholders (users, customers, regulators, business analysts) and the technical team that will design, build, and test the system. It is inherently a communication discipline: translating fuzzy human intentions into precise, verifiable specifications that developers can implement and testers can validate.

SWEBOK v4 treats requirements as a continuous activity spanning the full lifecycle — not a one-time phase. Modern agile and iterative approaches embed requirements work into every sprint, while still relying on the same fundamental KA topics: elicitation, analysis, specification, validation, and management.

## The 5 Topic Areas

### 1. [[Requirements Elicitation]]
- Techniques for discovering stakeholder needs: interviews, workshops, observation, surveys, prototyping, document analysis
- Identifying and prioritizing stakeholders (users, sponsors, regulators, affected parties)
- Tacit vs. explicit requirements — surfacing what stakeholders don't know they need
- Context of the organization, domain constraints, and existing system interfaces
- **Book:** *Mastering the Requirements Process, 4th Ed.* — Suzanne & James Robertson

### 2. [[Requirements Analysis]]
- Negotiating conflicts between stakeholder groups
- Classifying requirements: functional, non-functional (quality attributes), constraints
- Modeling requirements: use cases, user stories, data flow diagrams, state machines
- Feasibility analysis and trade-off decisions
- Prioritization techniques (MoSCoW, Kano, cost-value analysis)
- **Book:** *Software Requirements, 3rd Ed.* — Karl Wiegers & Joy Beatty

### 3. [[Requirements Specification]]
- Documenting requirements in structured formats: SRS (IEEE 830/29148), user stories, use case specifications
- Attributes of good requirements: unambiguous, verifiable, traceable, feasible, necessary
- Natural language vs. formal specification trade-offs
- Agile documentation: lightweight artifacts, living documents, acceptance criteria
- **Book:** *Writing Effective Use Cases* — Alistair Cockburn

### 4. [[Requirements Validation]]
- Techniques: reviews, inspections, prototyping, model validation, acceptance test design
- Ensuring requirements are complete, consistent, correct, and feasible
- Requirements validation vs. software verification (V&V distinction)
- Stakeholder sign-off and baselining
- **Book:** *Requirements Engineering Fundamentals* — Klaus Pohl & Chris Rupp

### 5. [[Requirements Management]]
- Traceability: linking requirements to design, code, tests, and business objectives
- Change control: impact analysis, change request workflows, configuration management
- Versioning and baselining of requirement sets
- Requirements status tracking and metrics
- Tools: DOORS, Jama, Jira, Azure DevOps
- **Book:** *Software Requirements, 3rd Ed.* — Karl Wiegers & Joy Beatty

## Recommended Books (Priority Order)

| #   | Book                                                 | Author(s)                 | Pages |     Priority     |
| --- | ---------------------------------------------------- | ------------------------- | :---: | :--------------: |
| 1   | *Software Requirements, 3rd Ed.* (2013)              | Karl Wiegers & Joy Beatty |  672  |   🔴 Essential   |
| 2   | *Mastering the Requirements Process, 4th Ed.* (2024) | Suzanne & James Robertson |  576  |   🔴 Essential   |
| 3   | *Requirements Engineering Fundamentals* (2009)       | Klaus Pohl & Chris Rupp   |  248  |  🟡 Recommended  |
| 4   | *User Stories Applied* (2004)                        | Mike Cohn                 |  304  |  🟡 Recommended  |
| 5   | *Writing Effective Use Cases* (2001)                 | Alistair Cockburn         |  256  | 🟢 Supplementary |

## Relationship to Other KAs

- **[[Software Design Overview|Software Design]]** — Requirements feed directly into architectural and detailed design; design decisions may reveal missing or conflicting requirements.
- **[[Software Testing Overview|Software Testing]]** — Requirements are the basis for test cases, acceptance criteria, and validation strategies. Traceability links requirements to test coverage.
- **[[Software Quality Overview|Software Quality]]** — Non-functional requirements (reliability, performance, security) are quality attributes that drive SQA processes.
- **[[Software Engineering Management Overview|Software Engineering Management]]** — Requirements scope drives project planning, estimation, and resource allocation.
- **[[Software Security Overview|Software Security]]** — Security requirements (confidentiality, integrity, authorization) must be elicited alongside functional needs, not bolted on later.

## Related
- [[SWEBOK v4 - Overview]]
- [[02_Software_Architecture/Software Architecture Overview|Software Architecture Overview]]
- [[05_Software_Testing/Software Testing Overview|Software Testing Overview]]
