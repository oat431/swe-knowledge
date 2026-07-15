---
tags:
  - programming
  - fundamental
  - overview
---

# Programming Fundamentals — Overview

Fundamentals are the bedrock every engineer stands on, regardless of experience level. Senior developers revisit them to sharpen mental models; juniors learn them to build correct intuition. Skipping this foundation is the fastest way to accumulate technical debt in your own skill set.

This section covers the eight core topics every programmer should master.

---

## Topics

> [[01 Core Concepts]] — Programs, execution models, compilation vs interpretation, and programming paradigms.

> [[02 Variables & Data Types]] — Primitive and reference types, type coercion, scope, naming conventions, and common pitfalls.

> [[03 Operators & Expressions]] — Arithmetic, comparison, logical, bitwise, and assignment operators with precedence rules.

> [[04 Control Flow]] — Conditionals, loops, pattern matching, guard clauses, and recursion basics.

> [[05 Functions & Methods]] — Function anatomy, parameter passing, overloading, higher-order functions, closures, and the call stack.

> [[06 Object-Oriented Programming]] — The four pillars, constructors, access modifiers, SOLID, and composition vs inheritance.

> [[07 Error Handling]] — Exceptions, try/catch/finally, resource management, defensive programming, and logging strategies.

> [[08 Memory Management]] — Stack vs heap, garbage collection, manual memory management, memory leaks, and immutability.

> [[09 Functional Programming]] — Pure functions, immutability, higher-order functions, map/filter/reduce, closures, and FP vs OOP.

> [[10 Data Formats & Serialization]] — JSON, XML, Protocol Buffers, Avro, Java Serializable, when to use which format.

> [[11 Logging & Structured Logging]] — Log levels, structured JSON logging, correlation IDs, MDC, SLF4J/Logback.

> [[12 Regex Cheatsheet]] — Syntax reference, character classes, quantifiers, groups, common patterns, Java/TypeScript usage.

> [[13 CI CD Pipelines]] — CI vs CD vs CD, GitHub Actions, GitLab CI, Jenkins, pipeline patterns, environment promotion, secrets management.

> [[14 Docker & Containerization]] — VM vs Container, Dockerfile best practices, multi-stage builds, Compose, networking, volumes, security.

---

## When to Review This

| Situation | Review These Notes |
|---|---|
| Preparing for a technical interview | All 8 — skim once, deep-dive weak areas |
| Debugging a type-related bug | [[02 Variables & Data Types]], [[03 Operators & Expressions]] |
| Onboarding to a new language | [[01 Core Concepts]], [[02 Variables & Data Types]], [[05 Functions & Methods]] |
| Refactoring legacy OOP code | [[06 Object-Oriented Programming]], [[07 Error Handling]] |
| Investigating a memory leak or performance issue | [[08 Memory Management]] |
| Writing a library or public API | [[05 Functions & Methods]], [[06 Object-Oriented Programming]], [[07 Error Handling]] |
| Code review — spotting common mistakes | [[03 Operators & Expressions]], [[04 Control Flow]], [[07 Error Handling]] |
| Mentoring a junior developer | Start with whichever topic they struggle with; these notes are self-contained |
| Working with streams, lambdas, async pipelines | [[09 Functional Programming]], [[05 Functions & Methods]] |

---

## Sources

- *Clean Code* — Robert C. Martin
- *Effective Java* — Joshua Bloch
- *Structure and Interpretation of Computer Programs* (SICP) — Abelson & Sussman
- *The Pragmatic Programmer* — Andrew Hunt & David Thomas
- Oracle Java Language Specification
- MDN Web Docs (developer.mozilla.org)
