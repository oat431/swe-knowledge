---
tags:
  - math
  - foundations
  - swebok
  - overview
  - software-engineering
  - discrete-mathematics
---

# Mathematical Foundations — Overview

> **Source:** SWEBOK v4 Chapter 17 — Mathematical Foundations
> **Purpose:** Logic, proof techniques, set theory, graphs & trees, FSMs, grammars, probability, numerical methods, and algebraic structures — the formal bedrock for rigorous specification and verification.

## What Is This?

Mathematical Foundations covers the discrete mathematics and formal reasoning that underpin software engineering. These aren't math for math's sake — they're the tools for specifying systems precisely, proving properties about algorithms, reasoning about state machines, analyzing complexity, and managing uncertainty. Every time you write a loop invariant, design a state machine, or estimate a probability, you're using these foundations.

The twelve knowledge areas form a progression: logic and proof techniques give you the language and methods for rigorous argument; sets, relations, and functions provide the structures; graphs, trees, and FSMs model system behavior; grammars define languages; counting and probability quantify uncertainty; numerical methods handle real-world computation limits; and algebraic structures abstract recurring patterns into reusable frameworks.

Together, these topics equip a software engineer to read and write formal specifications, verify correctness claims, analyze algorithm complexity, design robust protocols, and reason about security and reliability — moving beyond intuition into demonstrable, repeatable engineering.

## Knowledge Areas

### Basic Logic
- Propositional logic: connectives, truth tables, tautologies, equivalences (De Morgan's, distributive)
- Predicate logic: quantifiers (∀, ∃), predicates, inference rules
- Applications: formal specification, verification, program correctness

### Proof Techniques
- Direct proof, proof by contradiction, proof by induction (weak/strong)
- Proof by contrapositive, constructive vs non-constructive
- Applications: algorithm correctness, loop invariants, termination proofs

### Sets, Relations & Functions
- Set operations, relations (reflexive, symmetric, transitive), equivalence relations, partial orders
- Functions: injective, surjective, bijective, composition, inverse
- Cardinality, countability

### Graphs & Trees
- Graph types, representations (adjacency matrix/list), paths, cycles
- Trees: binary, BST, spanning trees, traversals
- Applications: dependency graphs, call graphs, state transitions, network topology

### Finite State Machines
- DFA, NFA, regular expressions and their equivalence
- State tables, state diagrams, information capacity
- Applications: parsers, protocol design, UI state management

### Grammars & the Chomsky Hierarchy
- Type 0-3 grammars: phrase structure, context-sensitive, context-free, regular
- Production rules, terminals/nonterminals
- Regular expressions (union, product, Kleene star)

### Number Theory
- Number types, divisibility, modular arithmetic, primes
- GCD, coprime/relatively prime
- Applications: cryptography (public-key schemes)

### Basics of Counting
- Sum rule, product rule, inclusion-exclusion principle
- Recursion: defining algorithms in terms of themselves
- Permutations and combinations

### Discrete Probability
- Sample space, events, random variables, PDF
- Mean, variance, standard deviation
- Permutations (nPr), combinations (nCr)

### Numerical Precision, Accuracy & Error
- Finite representation of numbers, overflow
- Fixed-point vs floating-point, rounding vs chopping
- Absolute/relative error, significant digits

### Algebraic Structures
- Groups, Abelian groups, semigroups, monoids
- Rings and fields
- Applications: cryptography, coding theory, abstract data types

### Engineering Calculus
- Limits, continuity, differentiation, integration
- Transcendental functions, vector calculus
- Applications: data analysis, optimization, extrapolation

## My Notes

- [[Basic Logic]] — Propositional and predicate logic, truth tables, inference
- [[Proof Techniques]] — Direct, contradiction, induction, contrapositive
- [[Set, Relations, Functions]] — Set operations, relations, functions, cardinality
- [[Graphs and Trees]] — Graph types, representations, trees, traversals
- [[Finite State Machines]] — DFA, NFA, regular expressions, state diagrams
- [[Grammars]] — Chomsky hierarchy, production rules, formal languages
- [[Number Theory]] — Divisibility, modular arithmetic, primes, GCD
- [[Basics of Counting]] — Sum/product rule, permutations, combinations, recursion
- [[Discrete Probability]] — Sample space, random variables, mean/variance
- [[Numerical Precision, Accuracy, and Errors]] — Floating-point, error types
- [[Algebraic Structures]] — Groups, rings, fields, monoids
- [[Calculus]] — Limits, differentiation, integration, vector calculus

## What's Missing

- New Advancements (computational neuroscience, genomics — SWEBOK ch17 section 13)

## Relationship to Other Foundations

- **[[Computing Foundation Overview|Computing Foundations]]** — Algorithms and data structures directly use discrete math; complexity analysis uses counting and probability
- **[[Engineering Foundation Overview|Engineering Foundations]]** — Statistical methods and measurement theory build on probability and calculus
- **Software Engineering KAs** — Formal methods, testing (mutation/coverage), security (cryptography) all depend on mathematical foundations
