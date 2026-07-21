---
tags: [recording-design, mbd, uml, structural, behavioral, software-design, swebok]
---

# Recording Software Designs — Models, Views, and Descriptions

> *Source: SWEBOK v4 Chapter 03 — Software Design*

## Purpose

Designs must be communicated. Recording a design means representing it in forms that stakeholders can understand, analyze, implement, and maintain. A design that exists only in someone's head is not a design — it's a wish.

## Why Record Designs?

| Reason | Benefit |
|---|---|
| **Shared understanding** | Team alignment on what's being built |
| **Analysis** | Verify correctness, consistency, completeness before coding |
| **Implementation guidance** | Programmers know exactly what to build |
| **Maintenance** | Future developers understand the intent |
| **Traceability** | Link design decisions to requirements |
| **Compliance** | Regulatory requirements for documentation |

## Model-Based Design (MBD)

**Definition:** Use models as the primary design artifacts — not just documentation but executable, analyzable specifications.

**Key characteristics:**
- **Models are the source of truth** — code may be generated from them
- **Simulation and analysis** — Test the design before implementing
- **Continuous integration** — Models evolve with the system, not as separate artifacts
- **Tool-enabled** — MATLAB/Simulink, UML tools, domain-specific modeling tools

**Applicability:**
- Safety-critical systems (DO-178C for avionics, ISO 26262 for automotive)
- Embedded systems with hardware constraints
- Systems with complex state behavior

## Structural Descriptions

**Purpose:** Show what the system is made of — components, their relationships, and their static properties.

| Notation | What It Shows | When to Use |
|---|---|---|
| **Class Diagram (UML)** | Classes, attributes, methods, relationships | OO design at any level |
| **Component Diagram (UML)** | Components, interfaces, dependencies | High-level architecture |
| **Package Diagram (UML)** | Groupings, dependencies, layering | Module organization |
| **Deployment Diagram (UML)** | Nodes, artifacts, communication paths | Physical deployment |
| **Entity-Relationship Diagram (ERD)** | Entities, attributes, relationships | Database design |
| **CRC Cards** | Class-Responsibility-Collaboration on index cards | Brainstorming, low-tech design sessions |
| **Interface Description Language (IDL)** | Formal interface definitions | Cross-language, distributed systems |

### When to Use Each

```
System Scope → Component Diagram
    ↓
Module Organization → Package Diagram
    ↓
Class Design → Class Diagram
    ↓
Database Design → ERD
    ↓
Deployment → Deployment Diagram
```

## Behavioral Descriptions

**Purpose:** Show what the system does — how it behaves over time, how components interact, and how state changes.

| Notation | What It Shows | When to Use |
|---|---|---|
| **Sequence Diagram (UML)** | Message exchanges in time order | API interactions, use case flows |
| **Communication Diagram (UML)** | Object interactions with numbering | Alternative to sequence diagrams |
| **Activity Diagram (UML)** | Workflow, process flow, parallel branches | Business processes, complex algorithms |
| **State Diagram (UML)** | States, transitions, events, guards | Stateful components, protocols |
| **Data Flow Diagram (DFD)** | Data movement between processes | Information flow, data-centric systems |
| **Decision Table/Tree** | Condition-action logic | Complex business rules |
| **Pseudocode** | Algorithm steps in plain language | Algorithm design |
| **Flowchart** | Sequential logic with branching | Simple algorithms, decision logic |

### Selecting Behavioral Views

| What You Want to Show | Best Notation |
|---|---|
| Object message passing | Sequence or Communication Diagram |
| Business process flow | Activity Diagram |
| Component lifecycle | State Diagram |
| How data moves | DFD |
| Complex conditional logic | Decision Table |
| Algorithm details | Pseudocode or Flowchart |

## Choosing What to Record

> **"Record what you need, not everything you can."**

**Minimum Viable Design Documentation:**
1. **Architecture overview** — One-page diagram of major components
2. **Component interfaces** — API contracts, data formats
3. **Key design decisions** — What was chosen and why (ADR format)
4. **Critical algorithms** — Pseudocode for non-obvious logic
5. **Data models** — ERD or class diagram for persistent data

**The ADR (Architecture Decision Record) Format:**
```
Title: Use Event-Driven for Order Processing
Status: Accepted
Context: Orders need real-time processing with multiple downstream consumers
Decision: Use Kafka for event streaming between services
Consequences: Added operational complexity; gained scalability and loose coupling
```

## Essential Concepts

- **Models reduce ambiguity** — a picture is often worth 1024 words.
- **Don't over-document** — record what's needed, discard what's obvious.
- **Use multiple views** — no single diagram captures everything.
- **ADRs preserve rationale** — why, not just what.
- **MBD is for high-assurance systems** — not every project needs executable models.

## Related

- [[Software Design Note Overview]] — All design topics
- [[../02_Software_Architecture/07_Design_and_Documentation]] — Views and beyond (SAiP)
- [[Design Pattern/Design Pattern Overview|Design Patterns]] — Patterns as reusable design vocabulary
