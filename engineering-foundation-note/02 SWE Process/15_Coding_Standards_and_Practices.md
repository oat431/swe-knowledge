---
title: Coding Standards and Practices
tags:
  - coding
  - standards
  - documentation
  - software-engineering
source: SWE Theory & Practice Ch 7
---

# Coding Standards and Practices

> **Source:** *Software Engineering: Theory and Practice* — Chapter 7: Writing the Programs

## Overview

Writing programs is the critical step where design becomes executable software. This chapter addresses how to write code that is:

- **Understandable** — to yourself during testing and to others as the system evolves
- **Reusable** — taking advantage of design organization, data structure, and language constructs
- **Consistent** — following standards that enable team collaboration and maintenance

The chapter does not teach programming; rather, it explains the **software engineering practices** that should guide code writing.

## 1. Programming Standards and Procedures

Throughout a career, developers write code across many projects, domains, and tools. They also evaluate existing code for replacement, modification, or reuse, and participate in formal and informal code reviews. Standardized practices ensure consistency and quality across all this work.

### Why Standards Matter

- **Consistency** across team members and projects
- **Readability** for code review, testing, and maintenance
- **Reduced defects** through disciplined coding habits
- **Knowledge transfer** when team members change

### Types of Programming Standards

| Category | Examples |
|---|---|
| **Naming Conventions** | Variable/function/class naming rules, casing styles |
| **Code Formatting** | Indentation, line length, brace placement |
| **Comment Conventions** | Header blocks, inline comments, documentation strings |
| **Error Handling** | Exception patterns, return code conventions |
| **File Organization** | Module structure, import ordering, separation of concerns |
| **Language Usage** | Approved/disallowed features, idiomatic patterns |

## 2. Guidelines for Reuse

Code reuse is a fundamental software engineering goal. Key principles:

- **Design for reuse** — write modules with clear interfaces and minimal dependencies
- **Library and toolkit usage** — leverage existing, tested components rather than reinventing
- **Framework integration** — use frameworks as partially completed applications, filling in specialized modules
- **Component catalogs** — maintain searchable repositories of reusable code

### Reuse Hierarchy

```
Frameworks (high-level architecture reuse)
  └── Toolkits (related reusable classes)
       └── Libraries (lower-level software units)
            └── Individual functions/methods
```

## 3. Using Design to Frame the Code

The program design acts as a bridge from architectural design to code. Key connections:

- **Classes and interfaces** from design map directly to code structures
- **Design patterns** (Composite, Visitor, Observer, Decorator, etc.) guide implementation structure
- **Module interface specifications** define what programmers implement
- **Contracts** (preconditions, postconditions, invariants) become assertions in code

### From Design to Code

1. Review the class diagram and identify classes to implement
2. Follow interface specifications for each module
3. Implement design patterns as specified in the architecture
4. Add assertions from design contracts as runtime checks (where supported)
5. Maintain traceability from requirements → design → code

## 4. Internal and External Documentation

### Internal Documentation

Code-level documentation embedded within the source:

- **Header comments** — module purpose, author, date, revision history
- **Function/method comments** — parameters, return values, side effects, preconditions
- **Inline comments** — explaining non-obvious logic, algorithms, workarounds
- **Assertions** — formal preconditions, postconditions, and invariants

**Best practices:**
- Comment *why*, not *what* — the code shows what, comments explain intent
- Keep comments synchronized with code changes
- Use consistent comment style throughout the project

### External Documentation

Documentation outside the code that supports understanding and use:

- **Software Architecture Document (SAD)** — system-level design decisions
- **Program design documents** — module-level specifications
- **API documentation** — generated from code comments (e.g., Javadoc, docstrings)
- **User guides** — how to use the software
- **README files** — project overview, setup, contribution guidelines

### Design by Contract

A documentation approach where each module has an interface specification that precisely describes what the module does:

- **Preconditions** (`require`) — what must be true before the module executes
- **Postconditions** (`ensure`) — what the module guarantees after execution
- **Invariants** — properties that hold after every method invocation

Example contract for a dictionary module:

```
insert(elem: Element; key: String)
  require: count < capacity and key.valid() and not has(key)
  ensure:  has(key) and retrieve(key) == elem

retrieve(key: String): Element
  require: has(key)
  ensure:  result = dictionary(key)

invariant: forall (key1, Element1) and (key2, Element2) in dictionary,
           if key1<>key2 then Element1<>Element2
```

Contracts help ensure:
- Modules interoperate correctly
- Substitutability — any implementation adhering to the contract can replace another
- Testing basis — assertions provide clear test targets
- Change evaluation — each assertion can be checked when design changes

## 5. Exception Handling in Code

Exception handling separates error checking from normal program logic:

- Use **try–rescue** constructs to isolate unsafe code
- **Weaken preconditions** by converting them to exceptions — the module checks validity itself rather than trusting callers
- Provide **targeted recovery** schemes in rescue clauses
- Raise **new exceptions** to propagate errors to calling code when recovery fails

**Example — network transmission with retry:**

```
attempt_transmission(message: STRING) raises TRANSMISSIONEXCEPTION
  local failures: INTEGER
  try
    unsafe_transmit(message)
  rescue
    failures := failures + 1
    if failures < 100 then retry
    else raise TRANSMISSIONEXCEPTION
  end
```

## 6. Key Principles Summary

| Principle | Description |
|---|---|
| **Design for change** | All systems change; write code that accommodates it |
| **High cohesion** | Each module should have a single, well-defined purpose |
| **Low coupling** | Minimize dependencies between modules |
| **Clear interfaces** | Expose what users need; hide implementation details |
| **Reuse first** | Leverage existing libraries, frameworks, and components |
| **Document intent** | Code shows *how*; documentation explains *why* |
| **Contract adherence** | Use preconditions, postconditions, and invariants |

## See Also

- [[01 Physics and Math/01_Dimensions_and_Measurement|01_Dimensions_and_Measurement]] — Engineering measurement fundamentals
- Software Architecture Document (SAD) — Chapter 5 design documentation
- Design Patterns — Composite, Visitor, Observer, Decorator
- OO Measurement — Chidamber–Kemerer metrics, Lorenz and Kidd measures
