---
tags:
  - overview
  - swebok
  - software-requirements
  - requirements-engineering
---

# Software Requirements — Overview

> **Source:** SWEBOK v4 Chapter 01
> **Purpose:** Define, document, and maintain the needs and constraints that a software system must satisfy throughout its service life.

## What Is This?

Software Requirements engineering is the foundation upon which all successful software systems are built. It encompasses the process of discovering, analyzing, documenting, validating, and managing the needs and constraints placed on a software product. The two primary real-world problems are **incompleteness** (stakeholder needs that never reach engineers) and **ambiguity** (requirements open to multiple interpretations) — getting requirements wrong triggers exponentially cascading rework into design, code, and testing.

Requirements engineering bridges the gap between stakeholders (users, customers, regulators, business analysts) and the technical team. It is inherently a communication discipline: translating fuzzy human intentions into precise, verifiable specifications. SWEBOK v4 treats requirements as a continuous activity spanning the full lifecycle — not a one-time phase. Whether using plan-driven or Agile approaches, the same fundamental activities apply: elicitation, analysis, specification, validation, and management.

The chapter also introduces key conceptual tools: the **Perfect Technology Filter** for separating functional from nonfunctional requirements, the **5-Whys technique** for uncovering real problems behind solution-shaped requests, and **ATDD/BDD** as the strongest defense against ambiguity by using precise test-case language as specification.

## Knowledge Areas

### Requirements Fundamentals
- Defines what a software requirement is and categorizes them (functional vs. nonfunctional, product vs. project)
- Introduces the Perfect Technology Filter: if a requirement would still exist on an infinitely fast, zero-cost, never-failing computer, it's functional
- Different categories need different elicitation, analysis, specification, and validation approaches

### Requirements Elicitation
- Surfacing candidate requirements from stakeholders and non-person sources (prior systems, documentation, competitive benchmarking)
- Techniques: interviews, JAD/JRP workshops, prototyping, user story mapping, design thinking, observation, apprenticing
- Emphasizes that elicitation is active work — stakeholders leave things unsaid; stakeholder analysis prevents bias

### Requirements Analysis
- Refines elicited statements into true requirements: unambiguous, testable, binding, atomic, acceptable to all stakeholders
- 5-Whys technique to uncover the real problem behind solution-shaped requirements
- Covers economics of quality-of-service constraints (perfection point, fail point, most cost-effective level) and conflict resolution

### Requirements Specification
- Recording requirements so they can be remembered and communicated — the most contentious topic in requirements engineering
- Spectrum of techniques: unstructured natural language → structured (use cases, user stories) → acceptance criteria-based (ATDD/BDD) → model-based (UML/SysML, formal Z/VDM)
- ATDD/BDD directly addresses ambiguity by using precise test-case language as specification

### Requirements Validation
- Gaining confidence that requirements represent true stakeholder needs
- Three methods: requirements reviews (multi-perspective), simulation/execution of formal specs, and prototyping
- Requirements scrubbing: eliminate out-of-scope, low-ROI requirements before stakeholder validation

### Requirements Management
- Maintaining the agreement on requirements over time: change control, scope matching, traceability
- Prioritization using Kano model (dissatisfaction matters more than satisfaction), value-risk-cost formulas, MoSCoW
- Functional Size Measurement (FSM) and story points for quantitative scope management

### Practical Considerations
- Requirements process is inherently iterative — breadth and depth expand in alternating cycles
- Requirements tracing supports consistency checking and impact analysis
- Stability assessment helps design for change-tolerant architectures

### Software Requirements Tools
- Requirements management tools (attributes, tracing, change control, document generation)
- Requirements modeling tools (visual creation, static analysis, simulation, theorem provers, model checkers)
- Functional test case generation tools (mechanically deriving test cases from formal or BDD specifications)

## My Notes

*(No additional notes in this folder yet)*

## What's Missing

- Requirements Fundamentals (types, categories, Perfect Technology Filter)
- Requirements Elicitation (interviews, workshops, prototyping, user story mapping)
- Requirements Analysis (5-Whys, conflict resolution, quality-of-service economics)
- Requirements Specification (natural language, use cases, user stories, BDD, model-based)
- Requirements Validation (reviews, simulation, prototyping)
- Requirements Management (change control, scope matching, traceability)
- Practical Considerations (prioritization, Kano model, functional size measurement)

## Relationship to Other KAs

- **[[Software Design Note Overview|Software Design]]** — Requirements feed directly into architectural and detailed design; design decisions may reveal missing or conflicting requirements.
- **[[Software Testing Overview|Software Testing]]** — Requirements are the basis for test cases and acceptance criteria. ATDD/BDD blurs the boundary between specification and testing.
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — Requirements drive SLAs, capacity planning, and operational constraints.
- **[[Software Construction Overview|Software Construction]]** — Construction translates requirements-based designs into code.
