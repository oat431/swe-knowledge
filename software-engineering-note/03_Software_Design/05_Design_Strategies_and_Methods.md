---
tags: [design-strategies, ood, ddd, event-driven, software-design, swebok, design-methods]
---

# Design Strategies and Methods

> *Source: SWEBOK v4 Chapter 03 — Software Design*

## Purpose

Design strategies are the high-level approaches to structuring a system. Choosing the right strategy for the problem domain is one of the most consequential design decisions.

## Strategy Overview

| Strategy | Core Idea | Best For |
|---|---|---|
| **Object-Oriented Design (OOD)** | Model the world as objects with state and behavior | Most general-purpose software |
| **Domain-Driven Design (DDD)** | Align design with the business domain model | Complex business domains |
| **Function-Oriented Design** | Decompose by functions and data flows | Data processing, ETL pipelines |
| **Data-Centered Design** | Center design around shared data stores | Data-intensive applications |
| **Component-Based Design** | Assemble from reusable, independently deployable components | Large-scale, distributed systems |
| **Event-Driven Architecture** | Communicate through events, not direct calls | Asynchronous, scalable systems |
| **Aspect-Oriented Design** | Separate cross-cutting concerns from core logic | Logging, security, transactions |
| **Constraint-Based Design** | Define what's allowed, let system derive the solution | Configuration, scheduling, optimization |

## Object-Oriented Design (OOD)

**Core concepts:**
- **Objects** — Entities with state (attributes) and behavior (methods)
- **Classes** — Templates for creating objects
- **Inheritance** — "Is-a" relationships; reuse through hierarchy
- **Polymorphism** — Same interface, different behavior
- **Encapsulation** — Hide internal state behind methods

**Core principles (SOLID):**
- Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion

**Design process:**
1. Identify objects and classes from requirements (noun extraction)
2. Define relationships (inheritance, composition, association)
3. Define interfaces (public methods and their contracts)
4. Implement internal behavior

## Domain-Driven Design (DDD)

**Core idea:** The software design should mirror the business domain's conceptual model. Ubiquitous language between developers and domain experts.

**Key concepts:**

| Concept | Description |
|---|---|
| **Entity** | Object with a distinct identity (e.g., Customer, Order) |
| **Value Object** | Object defined by its attributes, not identity (e.g., Address, Money) |
| **Aggregate** | Cluster of entities treated as a unit; one is the root |
| **Repository** | Abstraction over data access for aggregates |
| **Domain Service** | Stateless operation that doesn't fit in an entity |
| **Bounded Context** | Explicit boundary where a model applies |
| **Domain Event** | Something important that happened in the domain |
| **Ubiquitous Language** | Shared vocabulary between developers and domain experts |

**Strategic design patterns:**
- **Context Map** — How bounded contexts relate to each other
- **Shared Kernel** — Shared subset of the domain model
- **Anti-Corruption Layer** — Translate between different models
- **Open Host Service** — Well-defined protocol for integration

## Function-Oriented Design

**Core idea:** Decompose the system into functions that transform data.

**Process:**
1. Identify inputs and outputs
2. Define the data transformations between them
3. Decompose top-level functions into sub-functions
4. Design data structures for the intermediate representations

**Tools:** DFDs, Structure Charts, Stepwise Refinement

**Best for:** Data processing, scientific computing, ETL pipelines, compilers.

## Event-Driven Architecture

**Core idea:** Components communicate by producing and consuming events. No direct coupling between producers and consumers.

**Patterns:**
- **Event Notification** — "Something happened" (minimal payload)
- **Event-Carried State Transfer** — Full data in the event (consumer doesn't call back)
- **Event Sourcing** — Events as the source of truth; state rebuilt from event history
- **CQRS** — Separate read and write models, optimized independently

**Trade-offs:**
| Pro | Con |
|---|---|
| Loose coupling | Eventual consistency |
| Independent scalability | Harder to reason about |
| Natural audit trail | Debugging complexity |
| Flexible consumers | Schema evolution challenges |

## Component-Based Design

**Core idea:** Build systems from independently developed, deployable, and replaceable components.

**Key properties:**
- **Independently deployable** — Can be updated without rebuilding the system
- **Well-defined interfaces** — Clear contracts for interaction
- **Replaceable** — Can swap implementations without affecting clients
- **Technology-agnostic** — Components may use different technologies

## Choosing a Strategy

| If Your Problem Is... | Consider... |
|---|---|
| Complex business rules | DDD |
| Data transformation pipeline | Function-Oriented |
| Real-time, scalable processing | Event-Driven |
| General-purpose application | OOD |
| Integrating legacy systems | Component-Based |
| Cross-cutting concerns everywhere | Aspect-Oriented |
| Configurable, rule-driven system | Constraint-Based |

> **Most real systems use multiple strategies.** A single web application might use OOD internally, event-driven between services, DDD for the core domain, and function-oriented for its data pipeline.

## Related

- [[Software Design Note Overview]] — All design topics
- [[01_Design_Fundamentals_and_Principles]] — Core design principles
- [[Clean Architecture/Design Principles/SOLID Design Principles]] — SOLID in depth
- [[Design Pattern/Design Pattern Overview|Design Patterns]] — Patterns for OOD
