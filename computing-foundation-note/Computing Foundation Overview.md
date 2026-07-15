---
tags:
  - computing
  - foundations
  - swebok
  - overview
  - computer-science
---

# Computing Foundations — Overview

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 16 — Computing Foundations
> **Purpose:** The computing bedrock under all software engineering — architecture, data structures, algorithms, operating systems, databases, networks, HCI, and AI/ML.

## What Is This?

Computing Foundations covers the fundamental computer science knowledge that every software engineer needs. These aren't software engineering topics per se — they're the underlying science and technology that SE builds upon. You can't design good software without understanding what the machine is doing, how data is organized, how networks communicate, or how users think.

## The 10 Topic Areas

### 1. [[01_Computer_Architecture]]

- Processor architecture and instruction sets
- Memory hierarchy (registers → cache → RAM → disk)
- Pipelining, parallelism, and concurrency at the hardware level
- I/O systems and buses
- **Book:** *Computer Systems: A Programmer's Perspective* — Bryant & O'Hallaron

### 2. [[02_Data_Structures]]

- Arrays, linked lists, stacks, queues, hash tables
- Trees (binary, B-trees, red-black, AVL)
- Graphs and graph representations
- Heaps and priority queues
- **Book:** *Introduction to Algorithms (CLRS), 4th Ed.* — Cormen, Leiserson, Rivest & Stein

### 3. [[03_Algorithms]]

- Sorting, searching, and complexity analysis (Big-O)
- Divide and conquer, dynamic programming, greedy algorithms
- Graph algorithms (shortest path, MST, network flow)
- NP-completeness and approximation
- **Book:** *Introduction to Algorithms (CLRS), 4th Ed.* — Cormen et al.
- **Book:** *Algorithm Design* — Kleinberg & Tardos

### 4. [[04_Programming_Languages_and_Paradigms]]

- Imperative, object-oriented, functional, declarative paradigms
- Type systems (static vs. dynamic, strong vs. weak)
- Memory management (stack vs. heap, GC vs. manual)
- Language design trade-offs
- **Book:** *Concepts of Programming Languages, 12th Ed.* — Sebesta

### 5. [[05_Operating_Systems]]

- Processes, threads, and scheduling
- Memory management (virtual memory, paging, segmentation)
- File systems and storage
- Synchronization, deadlocks, IPC
- **Book:** *Operating System Concepts, 10th Ed.* — Silberschatz, Galvin & Gagne
- **Book:** *Modern Operating Systems, 4th Ed.* — Tanenbaum

### 6. [[06_Database_Systems]]

- Relational model, SQL, normalization
- Transaction processing and ACID
- Indexing and query optimization
- NoSQL (document, key-value, column-family, graph)
- **Book:** *Database System Concepts, 7th Ed.* — Silberschatz, Korth & Sudarshan

### 7. [[07_Computer_Networks]]

- OSI and TCP/IP models
- Protocols (TCP, UDP, HTTP, DNS, TLS)
- Routing, switching, and addressing
- Network security fundamentals
- **Book:** *Computer Networking: A Top-Down Approach, 8th Ed.* — Kurose & Ross

### 8. [[08_Human_Computer_Interaction]]

- Cognitive models and mental models
- Usability principles and heuristics
- Interaction design patterns
- Accessibility and inclusive design
- **Book:** *Designing the User Interface, 6th Ed.* — Shneiderman et al.
- **Book:** *Don't Make Me Think, Revisited* — Steve Krug

### 9. [[09_Security_Fundamentals]]

- Cryptography (symmetric, asymmetric, hashing)
- Authentication and authorization
- Common vulnerabilities (OWASP Top 10)
- Secure coding basics
- **Book:** *Security Engineering, 3rd Ed.* — Ross Anderson

### 10. [[10_AI_and_Machine_Learning]]

- Supervised, unsupervised, reinforcement learning
- Neural networks and deep learning fundamentals
- Model evaluation and bias
- AI/ML in software engineering (testing, code generation)
- **Book:** *Hands-On Machine Learning with Scikit-Learn, Keras & TensorFlow, 3rd Ed.* — Aurélien Géron

---

## Relationship to Your Vault

These topics already have learning notes in the `programming-note/` vault:

| Topic Area | Existing Notes | Location |
|-----------|---------------|----------|
| Data Structures & Algorithms | ✅ 32 files | `programming-note/Algorithm/` + `Algorithm_advance/` |
| Operating Systems | ✅ 10 files | `programming-note/Operating Systems/` |
| Database Systems | ✅ 10 files | `programming-note/Database/` |
| Computer Networks | ✅ 10 files | `programming-note/Computer Networks/` |
| Security Fundamentals | ✅ 13 files | `programming-note/Cybersecurity/` |
| HCI | ✅ 34 files | `software-engineering-note/03_Software_Design/Human Computer Interaction/` |
| Programming Fundamentals | ✅ 15 files | `programming-note/Fundamental/` |
| Computer Architecture | ❌ Missing | — |
| Programming Languages & Paradigms | ⚠️ Partial | `Fundamental/06 Object-Oriented Programming.md` only |
| AI & Machine Learning | ❌ Missing | — |

## Reading Path

1. **Start with** [[01_Computer_Architecture]] — understand the machine
2. **Then** [[02_Data_Structures]] → [[03_Algorithms]] — the core CS foundation
3. **Then** [[04_Programming_Languages_and_Paradigms]] — how we express solutions
4. **Then** [[05_Operating_Systems]] → [[06_Database_Systems]] → [[07_Computer_Networks]] — the platform stack
5. **Then** [[08_Human_Computer_Interaction]] — the user interface
6. **Then** [[09_Security_Fundamentals]] → [[10_AI_and_Machine_Learning]] — cross-cutting concerns

---

## Related

- [[SWEBOK v4 - Overview]]
- [[Programming Note Content]]
- [[Math For SE Note Overview]]
