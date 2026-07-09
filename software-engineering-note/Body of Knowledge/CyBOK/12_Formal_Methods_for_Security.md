---
tags: [formal-methods, cyber-security, cybok]
---

# 12 — Formal Methods for Security

> *Source: CyBOK v1.1 by NCSC, Chapter 13 — Formal Methods for Security*

## Purpose

Formal methods apply mathematical rigor to the specification, design, and verification of security properties in systems. They provide the highest level of assurance by proving that a system satisfies its security requirements under all possible conditions — essential for high-assurance systems (avionics, cryptographic protocols, safety-critical).

## Key Concepts

### Formal Specification
Defining system behavior and security properties in a mathematically precise language. Common formalisms: temporal logic (LTL, CTL), set theory, process algebras (CSP, π-calculus), state machines.

### Model Checking
Exhaustively exploring all possible states of a finite-state model to verify that specified properties hold. Tools: SPIN, NuSMV, UPPAAL. Highly automated but limited by state-space explosion.

### Theorem Proving
Using logical deduction to prove that a system satisfies its specification. Interactive (Isabelle/HOL, Coq) or automated (Z3, Vampire). Handles infinite state spaces but requires significant expertise.

### Static Analysis
Automated analysis of source code or models without execution. Includes: abstract interpretation, data-flow analysis, type systems. Used to detect buffer overflows, injection vulnerabilities, information leaks.

### Applications in Security
- **Protocol verification** — Proving cryptographic protocols satisfy authentication and secrecy properties
- **Hardware verification** — Ensuring hardware implementations match specifications
- **Software verification** — Proving absence of specific vulnerability classes
- **Security policy analysis** — Verifying access control policies for consistency and completeness

## Related Chapters

- [[09_Software_Security]] — Static analysis and bug detection
- [[14_Secure_Software_Lifecycle]] — Where formal methods fit in the SDLC
- [[16_Applied_Cryptography]] — Cryptographic protocol verification
