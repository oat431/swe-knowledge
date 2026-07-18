---
tags:
  - computing
  - foundations
  - swebok
  - overview
  - computer-science
---

# Computing Foundations — Overview

> **Source:** SWEBOK v4 Chapter 16 — Computing Foundations
> **Purpose:** The fundamental computer science knowledge every software engineer needs — architecture, data structures, algorithms, OS, databases, networks, HCI, and AI/ML.

## What Is This?

Computing Foundations covers the fundamental computer science knowledge that every software engineer needs. These aren't software engineering topics per se — they're the underlying science and technology that SE builds upon. You can't design good software without understanding what the machine is doing, how data is organized, how networks communicate, or how users think.

The breadth of this chapter spans the entire computing stack: from hardware architecture and how processors execute instructions, through data structures and algorithms that form the core of problem-solving, up to operating systems, databases, and networks that provide the platform for modern software. It also encompasses human-computer interaction — how people use systems — and the increasingly important field of AI and machine learning.

Understanding these foundations gives software engineers the vocabulary and mental models to make informed design decisions, diagnose performance problems, choose appropriate technologies, and communicate effectively with specialists across every layer of the stack. Without this knowledge, software engineering becomes superficial — you might build something that works, but you won't understand *why* it works or how to fix it when it breaks.

## Knowledge Areas

### Basic Concepts of a System
- Problem-to-solution mapping, modular/cohesive/loosely-coupled subsystems
- Functional requirements, performance, security, durability

### Computer Architecture and Organization
- Von Neumann vs Harvard architecture, RISC vs CISC
- Flynn's Taxonomy (SISD, SIMD, MISD, MIMD)
- Memory hierarchy, I/O systems, microarchitecture

### Data Structures and Algorithms
- Basic types, compound structures (arrays, lists, stacks, queues, hash tables, trees, graphs)
- Algorithm families: divide & conquer, DP, greedy, backtracking
- Complexity analysis: Big-O, Big-Ω, Θ

### Programming Fundamentals and Languages
- Paradigms: imperative, OOP, functional, declarative
- Type systems (static/dynamic), memory management
- Language translation: compilers, interpreters, linking

### Operating Systems
- Process/thread management, CPU scheduling, IPC
- Memory management (virtual memory, paging, segmentation)
- File systems, device management, network management

### Database Management
- Relational, NoSQL (key-value, document, columnar, graph)
- ACID vs BASE, normalization, schema design
- Transactions, concurrency control, query processing

### Computer Networks
- OSI and TCP/IP models, protocols (HTTP, TCP, UDP, DNS)
- Network security, load balancing, proxies
- Distributed systems communication

### Human Factors and HCI
- Usability, accessibility, cognitive load
- UI design principles, user interaction patterns
- User-centered design process

### AI and Machine Learning
- Supervised, unsupervised, reinforcement learning
- Neural networks, NLP, computer vision
- AI/ML integration in software systems

## My Notes

- [[Computer Oraganization/Computer Organization Overview|Computer Organization]] — Abstractions, ISA, arithmetic, processor design, memory hierarchy, I/O, parallel computing
- [[Programming Language Theory/Programming Language Theory Overview|Programming Language Theory]] — Expressions, binding, data types, syntax, type systems, operational semantics
- [[Algorithm Overview|Algorithm]] — Data structures, sorting, searching, DP, greedy, divide & conquer
- [[Algorithm Advance - Overview|Algorithm (Advanced)]] — B-Trees, Fibonacci heaps, shortest paths, MST, max flow, NP-completeness
- [[Fundamental Overview|Programming Fundamentals]] — Core concepts, variables, control flow, functions, OOP, FP, error handling
- [[Operating Systems Overview|Operating Systems]] — Processes, memory, concurrency, file systems, IPC
- [[Database Overview|Database]] — SQL, normalization, NoSQL, indexing, transactions, scaling
- [[Computer Networks Overview|Computer Networks]] — OSI/TCP-IP, HTTP, DNS, TCP/UDP, load balancing, security
- [[Design Pattern Overview|Design Patterns (Simplified)]] — Creational, structural, behavioral patterns
- [[Clean Code Overview|Clean Code (Simplified)]] — Naming, functions, code smells, refactoring
- [[HCI Overview|HCI (Simplified)]] — Usability principles, cognitive load, accessibility basics

## What's Missing

- AI and Machine Learning (supervised/unsupervised/RL, neural networks, NLP, ML integration in SE)

## Relationship to Other Foundations

- **[[Math For SE Note Overview|Mathematical Foundations]]** — Provides formal reasoning (logic, proofs, discrete structures) that underpin algorithms and verification
- **[[Engineering Foundation Overview|Engineering Foundations]]** — Provides methodology (problem-solving, measurement, standards) for applying CS knowledge
- **Software Engineering KAs** — Computing Foundations is the technology base that all SE knowledge areas build upon
