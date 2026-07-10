---
tags:
  - design-patterns
  - overview
  - programming
---

# Design Pattern Overview

Design patterns are reusable solutions to common software design problems. They aren't finished code — they're templates you adapt to your context. Knowing *when* to apply a pattern matters more than knowing the pattern itself.

---

## The Gang of Four (GoF)

The original 23 patterns from *Design Patterns: Elements of Reusable Object-Oriented Software* (1994) by Gamma, Helm, Johnson, Vlissides. Grouped into three categories:

| Category | Purpose | Covers |
|----------|---------|--------|
| **Creational** | How objects are created | [[01 Creational Patterns\|Factory, Builder, Singleton, Prototype, Abstract Factory]] |
| **Structural** | How classes/objects are composed | [[02 Structural Patterns\|Adapter, Decorator, Facade, Proxy, Composite, Bridge]] |
| **Behavioral** | How objects communicate | [[03 Behavioral Patterns\|Strategy, Observer, Command, State, Template Method, Iterator, Chain of Responsibility]] |

---

## When to Use: Decision Table

| Situation | Consider | Category |
|-----------|----------|----------|
| Need to create objects without hardcoding the class | Factory Method | Creational |
| Construction has many optional parameters | Builder | Creational |
| Exactly one instance must exist | Singleton | Creational |
| Clone existing objects instead of creating from scratch | Prototype | Creational |
| Two incompatible interfaces must work together | Adapter | Structural |
| Add behavior to objects without subclassing | Decorator | Structural |
| Hide complex subsystem behind one entry point | Facade | Structural |
| Control access to an object (lazy, cached, remote) | Proxy | Structural |
| Swap algorithms at runtime | Strategy | Behavioral |
| One-to-many notification between objects | Observer | Behavioral |
| Object behavior changes based on internal state | State | Behavioral |
| Define skeleton algorithm, defer steps to subclasses | Template Method | Behavioral |
| Operations need undo/redo capability | Command | Behavioral |
| Pass request through a chain of handlers | Chain of Responsibility | Behavioral |

---

## GoF vs Modern Patterns

| Aspect | GoF (1994) | Modern |
|--------|------------|--------|
| Focus | Object-oriented design | System architecture |
| Scope | Single application | Distributed systems |
| Examples | Singleton, Observer | Circuit Breaker, Event Sourcing, CQRS |
| Language assumptions | C++ / Smalltalk | Any paradigm |

---

## Prerequisites

Solid understanding of OOP fundamentals is essential. See [[06 Object-Oriented Programming]] for:
- Encapsulation, inheritance, polymorphism
- SOLID principles
- Abstract classes vs interfaces

---

## Deep Dives in software-engineering-note

For book-level depth on each pattern, see the **Design Pattern** section in `software-engineering-note/Software Design/Design Pattern/`:

| Pattern | Book Summary |
|---------|-------------|
| Singleton, Factory Method, Abstract Factory, Builder, Prototype | [[02-Creational/]] — each pattern with intent, structure, Java examples |
| Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy | [[03-Structural/]] — each pattern with UML, real-world usage |
| Chain of Responsibility, Command, Iterator, Mediator, Memento, Observer, State, Strategy, Template Method, Visitor | [[04-Behavioral/]] — each pattern with participants, collaboration diagrams |
| Design Principles & SOLID | [[design-principles]], [[solid-principles]] |

> The notes here are **quick references** — pattern-per-use-case with short examples. The book summaries are **deep dives** with UML, trade-offs, and implementation details.

---

## Sources

- Gamma, Helm, Johnson, Vlissides — *Design Patterns* (1994)
- Refactoring Guru — https://refactoring.guru/design-patterns
- Head First Design Patterns — Freeman & Robson
