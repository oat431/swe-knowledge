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
- [[00 Basic Concepts of a System|Basic Concepts of a System]] — Problem-to-solution mapping, modularity, cohesion, coupling, system decomposition, integration, quality attributes

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
- **Compiler Design:** Full compilation pipeline, lexing, parsing, semantic analysis, SSA, optimization, code generation, JIT

### Operating Systems
- Process/thread management, CPU scheduling, IPC
- Memory management (virtual memory, paging, segmentation)
- File systems, device management, network management
- **Distributed Systems:** CAP theorem, consensus (Paxos/Raft), consistent hashing, CRDTs, fault tolerance patterns, MapReduce

### Database Management
- Relational, NoSQL (key-value, document, columnar, graph)
- ACID vs BASE, normalization, schema design
- Transactions, concurrency control, query processing
- **Data Warehousing & Mining:** OLAP vs OLTP, star/snowflake schemas, ETL/ELT pipelines, data mining (association, clustering, classification)

### Computer Networks
- OSI and TCP/IP models, protocols (HTTP, TCP, UDP, DNS)
- Network security, load balancing, proxies
- **Wireless & Mobile:** WiFi 802.11, Bluetooth, cellular 1G-5G, FDMA/TDMA/CDMA/OFDMA, Mobile IP, WPA3 security

### Human Factors and HCI
- Usability, accessibility, cognitive load
- UI design principles, user interaction patterns
- User-centered design process

### AI and Machine Learning
- Supervised, unsupervised, reinforcement learning
- Neural networks, NLP, computer vision
- **AI ↔ SE Intersection:** AI for SE (defect prediction, test generation, code review), SE for AI (ML pipelines, MLOps, model monitoring, responsible AI)

## My Notes

### Core Topics
- [[Computer Oraganization/Computer Organization Overview|Computer Organization]] — Abstractions, ISA, arithmetic, processor design, memory hierarchy, I/O, parallel computing
- [[Programming Language Theory/Programming Language Theory Overview|Programming Language Theory]] — Expressions, binding, data types, syntax, type systems, operational semantics
- [[Artificial_Intelligence/AI Overview|Artificial Intelligence]] — Search, logic, uncertainty, ML, reinforcement learning, NLP, vision, ethics
- [[Algorithm Overview|Algorithm]] — Data structures, sorting, searching, DP, greedy, divide & conquer
- [[Algorithm Advance - Overview|Algorithm (Advanced)]] — B-Trees, Fibonacci heaps, shortest paths, MST, max flow, NP-completeness
- [[Fundamental Overview|Programming Fundamentals]] — Core concepts, variables, control flow, functions, OOP, FP, error handling
- [[00 Basic Concepts of a System|Basic Concepts of a System]] — Modularity, cohesion, coupling, system decomposition, integration, quality attributes
- [[Operating Systems Overview|Operating Systems]] — Processes, memory, concurrency, file systems, IPC
- [[Database Overview|Database]] — SQL, normalization, NoSQL, indexing, transactions, scaling
- [[Computer Networks Overview|Computer Networks]] — OSI/TCP-IP, HTTP, DNS, TCP/UDP, load balancing, security
- [[Design Pattern Overview|Design Patterns (Simplified)]] — Creational, structural, behavioral patterns
- [[Clean Code Overview|Clean Code (Simplified)]] — Naming, functions, code smells, refactoring
- [[HCI Overview|HCI (Simplified)]] — Usability principles, cognitive load, accessibility basics

### Gap-Filling Topics (Added 2026-07-21)
- [[Programming Language Theory/08_Compiler_Design|Compiler Design]] — Compilation pipeline, lexing, parsing, semantic analysis, SSA, optimization, code generation, JIT
- [[Operating Systems/04 Distributed Systems/04_Distributed_Systems|Distributed Systems]] — CAP theorem, Paxos/Raft, consistent hashing, CRDTs, fault tolerance, MapReduce
- [[Computer Networks/02 Protocols/02_Wireless_and_Mobile|Wireless & Mobile Networks]] — WiFi, Bluetooth, cellular 1G-5G, FDMA/TDMA/CDMA/OFDMA, Mobile IP, WPA3
- [[Database/04 Data Warehousing/04_Data_Warehousing_and_Mining|Data Warehousing & Mining]] — OLAP vs OLTP, star/snowflake schemas, ETL/ELT, data mining techniques
- [[Artificial_Intelligence/09_AI_SE_Intersection|AI ↔ SE Intersection]] — AI for SE (defect prediction, test generation), SE for AI (ML pipelines, MLOps, responsible AI)
- [[00 Basic Concepts of a System|Basic Concepts of a System]] — System definition, problem-to-solution mapping, modularity/cohesion/coupling, system lifecycle

## Coverage Status

> **100% SWEBOK v4 Chapter 16 Coverage Achieved**

| Area | Status | Notes |
|------|--------|-------|
| Computer Architecture & Organization | ✅ Complete | 7 files + overview |
| Data Structures & Algorithms | ✅ Complete | 25+ files (basic + advanced) |
| Programming Fundamentals & Languages | ✅ Complete | 12 Fundamental + 8 PLT files |
| Operating Systems | ✅ Complete | 9 files + Distributed Systems |
| Database Management | ✅ Complete | 8 files + Data Warehousing |
| Computer Networks | ✅ Complete | 8 files + Wireless/Mobile |
| Human Factors & HCI | ✅ Complete | 11 files (HCI, Clean Code, Design Patterns) |
| AI & Machine Learning | ✅ Complete | 9 files + AI-SE Intersection |
| Basic Concepts of a System | ✅ Complete | 1 file (10_Basic_Concepts_of_a_System) |

**Total:** 119 files covering all 9 SWEBOK Computing Foundations knowledge areas.

## Relationship to Other Foundations

- **[[Math For SE Note Overview|Mathematical Foundations]]** — Provides formal reasoning (logic, proofs, discrete structures) that underpin algorithms and verification
- **[[Engineering Foundation Overview|Engineering Foundations]]** — Provides methodology (problem-solving, measurement, standards) for applying CS knowledge
- **Software Engineering KAs** — Computing Foundations is the technology base that all SE knowledge areas build upon
