---
title: "Software Security"
tags: [software-security, cyber-security, cybok]
source: "CyBOK v1.1 — Chapter 15 (Software Security Knowledge Area)"
author: "Frank Piessens, KU Leuven"
date: 2021-07
---

# Software Security (CyBOK Ch15)

## Overview

The **Software Security Knowledge Area** provides a structured overview of known categories of software implementation vulnerabilities, and of techniques that can be used to **prevent** or **detect** such vulnerabilities, or to **mitigate** their exploitation. It covers secure coding practices, vulnerability taxonomies, static analysis, dynamic analysis, and runtime mitigations.

> **Scope distinction:** This KA focuses on *implementation vulnerabilities* and their countermeasures. Broader process-level concerns are covered in the **Secure Software Lifecycle** (Ch17). Platform-specific issues are covered in **Web & Mobile Security** (Ch16).

---

## 1. Categories of Vulnerabilities

Implementation vulnerabilities (security bugs) are often caused by insecure programming practices and can be understood as **violations of a partial specification** of a software sub-component. The **CVE** (Common Vulnerabilities and Exposures) lists close to 100,000 such vulnerabilities. The **CWE** (Common Weakness Enumeration) provides a community-developed classification.

### 1.1 Memory Management Vulnerabilities

Occur primarily in **memory-unsafe languages** (C, C++), where the language specification leaves correct memory handling to the programmer. Undefined behaviour on violation enables powerful attacks.

| Type | Description | Example |
|------|-------------|---------|
| **Spatial** | Indexing out-of-bounds into a valid memory range | **Buffer Overflow** — accessing array beyond its bounds |
| **Temporal** | Accessing memory that was deallocated | **Use-After-Free** / dereferencing a dangling pointer |

**Attack techniques exploiting memory vulnerabilities:**
- **Code corruption attack** — modify compiled program code to attacker-specified code
- **Control-flow hijack** — modify a code pointer (return address, function pointer) to execute attacker code:
  - *Direct code injection* — inject and execute attacker-provided code
  - *Code-reuse attack* — reuse existing code (e.g., **Return-to-Libc**, **Return-Oriented Programming (ROP)**)
- **Data-only attack** — modify data variables to escalate privileges
- **Information leak** — read memory to exfiltrate secrets or runtime metadata (e.g., addresses for bypassing **ASLR**)

### 1.2 Structured Output Generation Vulnerabilities

When programs construct structured output (SQL, HTML, shell commands) via **string manipulation** with user-supplied input, the intended structure is left implicit, enabling injection.

Also known as ****Injection Vulnerabilities****:

| Vulnerability | Structured Output | Context |
|--------------|-------------------|---------|
| ****SQL Injection**** | SQL code | Server-side web apps interacting with databases |
| ****Command Injection**** | Shell commands | OS command execution |
| ****Cross-Site Scripting (XSS)**** | JavaScript/HTML | Client-side browser execution |
| XPath injection, HTML injection, CSS injection, PostScript injection | Various | Various contexts |

**Key challenges:**
- Structured output languages may support sub-languages (e.g., HTML contains JavaScript, CSS, SVG)
- **Higher-order / stored injection** — output from one phase is stored and later used as input for another phase (e.g., stored XSS, second-order SQL injection)

### 1.3 Race Condition Vulnerabilities

Occur when a program shares resources with concurrent actors and the program's assumptions about the environment are violated. This introduces **non-determinism** — behaviour depends on timing/interleaving.

- ****TOCTOU** (Time-of-Check, Time-of-Use)** — the attacker invalidates a condition between when it is checked and when it is used
- Common in: filesystem access by privileged programs, multi-threaded web servers, session state in web applications

### 1.4 API Vulnerabilities

Violations of **API contracts** — using an API incorrectly can put the system in an error state exploitable by attackers.

- Particularly dangerous with **security-sensitive APIs**: cryptographic libraries, access control mechanisms
- Empirical evidence shows developers frequently misuse cryptographic APIs, introducing vulnerabilities
- **Secure implementation** of crypto APIs is covered in **Applied Cryptography** (Ch10)

### 1.5 Side-Channel Vulnerabilities

A **side-channel** is an information channel that communicates about program execution via effects from which the program's code abstracts (e.g., power consumption, EM radiation, execution time, micro-architectural state like caches).

- **Software-based side-channels** — observable from software running on the same hardware (e.g., cache timing)
- Most commonly a **confidentiality threat** (leaking information), but can also be an integrity threat (**fault injection**)
- Example: **Rowhammer** — crafted memory access patterns flip bits in DRAM
- Closely related: **covert channels** — attacker controls both the leaking program and the observing program

### 1.6 Discussion

- **Information flow security**: Some security objectives cannot be expressed as properties of single executions. e.g., requiring that confidential inputs never influence public outputs across *any* pair of executions.
- **Side-channel uniqueness**: Side-channel vulnerabilities are not violations of source-code-level specifications — they use effects the source code abstracts from.
- **Vulnerabilities as faults**: Implementation vulnerabilities can be understood through the lens of **Dependable Computing** fault taxonomies.

---

## 2. Prevention of Vulnerabilities

The most effective approach is to **eradicate vulnerability categories by design** of the programming language or API. The key insight: make it impossible for **untrapped errors** to occur (errors that go unnoticed, allowing attackers to steer behaviour).

### 2.1 Language Design and Type Systems

A programming language can prevent categories of vulnerabilities by:
1. Making it possible to **express the specification** within the language
2. Ensuring there can be **no untrapped execution errors** with respect to that specification

#### Memory Safety

A language is **memory-safe** if its definition implies no untrapped memory management errors.

- **Memory-safe languages**: Java, C#, Python, Haskell, Rust, SPARK (via a combination of static/dynamic checks and garbage collection)
- **Not memory-safe**: C, C++ (language definition allows untrapped errors; safe implementations exist at performance cost)
- **Rust** achieves memory safety without garbage collection via an **ownership type system** — the compiler statically reasons about pointer lifetimes

#### Structured Output Generation

- **Regular expression types** — XML documents as first-class values with typed structure guarantees
- ****LINQ**** (Language Integrated Query) — query expressions as language syntax rather than string concatenation

#### Race Conditions

- **Ownership types** prevent data races: while multiple aliases can exist, only one *owns* the resource
- **Rust's ownership regime**: (1) aliases only if they go out of scope before owner, (2) aliases read-only, (3) owner writes only when no aliases exist
- Research languages extend this to support **Information Flow Security**

### 2.2 API Design

APIs should be designed to **avoid untrapped execution errors** — make it hard to violate the API contract, and trap violations when they occur.

| Approach | Examples |
|----------|----------|
| Safer memory APIs for C/C++ | Fat pointers, smart pointers, garbage collection libraries |
| Safer structured output APIs | **Prepared Statements**, ORM frameworks, LINQ libraries |
| Safer cryptography APIs | Simplification, secure defaults, better documentation, auxiliary task support |
| **Design by contract** | Explicit pre-conditions/post-conditions, defensive programming |
| **Object-capability systems** | Language + API supporting least privilege — each code part only has needed privileges |

### 2.3 Coding Practices

Secure coding guidelines reduce the likelihood of introducing vulnerabilities. Key standards:

| Standard | Scope |
|----------|-------|
| **SEI CERT C / Java** | Heuristic, community-vetted rules for general-purpose development |
| **MISRA** | Stricter coding standard for critical systems in C |
| **SPARK** | Ada subset designed for formal verification of vulnerability absence |

**Rule categories:**
- Avoid dangerous API functions (e.g., `system()` in C)
- Avoid undefined behaviour / untrapped errors
- Mitigate language runtime issues (e.g., secrets in Java Strings)
- Proactive defensive rules (e.g., exclude user input from format strings)
- Side-channel avoidance (e.g., don't branch on secrets)

> **Challenge:** The number of rules grows over time. Tool support (static analysis) is critical for checking compliance.

---

## 3. Detection of Vulnerabilities

Detection techniques operate on existing source code and must trade off between:

| Property | Meaning | Approach |
|----------|---------|----------|
| **Soundness** | No false negatives (finds all vulnerabilities) | Static analysis — covers all executions via abstraction |
| **Completeness** | No false positives (all reported are real) | Dynamic testing — concrete executions as witnesses |

**Rice's Theorem** implies no technique can be both sound and complete for non-trivial categories.

### 3.1 Static Detection

Analyses program code (source or binary) without execution. Covers all possible program paths in a single analysis run.

#### 3.1.1 Heuristic Static Detection

Detects violations of rules that formalise secure programming heuristics.

- Builds a semantic model: abstract syntax tree, data flow, control flow
- Flags simple syntactic violations (e.g., "don't use this dangerous function")
- ****Taint Analysis**** (Flow Analysis): tracks whether untrusted **sources** influence values at risky **sinks**
  - Can also detect confidential data flowing to public outputs
  - **Sanitisation**: tainted values processed by validator functions have taint removed
  - Challenge: manual configuration of sources, sinks, and sanitisers

#### 3.1.2 Sound Static Verification

Aims to be sound for well-defined vulnerability categories (defined as specification violations).

| Technique | Description |
|-----------|-------------|
| **Program Verification** | Uses program logics (e.g., **Separation Logic**) with programmer-provided invariants, pre/post-conditions. Typically interactive, not automatic. |
| **Abstract Interpretation** | Maps concrete values to finite abstract domains; automatic for imperative programs without dynamic allocation/recursion. |
| **Model Checking** | Exhaustive state exploration; limited by state explosion. **Bounded model checking** limits loop iterations — unsound but finds many vulnerabilities. |

> **"Soundiness"**: In practice, implementations under-approximate some language features to reduce false positives, sacrificing soundness for usefulness. See the [Soundiness Manifesto](https://soundiness.org/).

### 3.2 Dynamic Detection

Executes the program and monitors for vulnerability indicators. Two aspects: **how to monitor** and **what executions to run**.

#### 3.2.1 Monitoring

- **Memory management**: Complete monitors possible but expensive; modern C compilers include monitoring options (compiler-instrumented)
- **Structured output**: No explicit specification to monitor — relies on heuristics like fine-grained dynamic taint analysis tracking untrusted input's impact on parse trees
- **API contracts**: Design-by-contract assertions compiled into runtime checks
- **Race conditions**: Monitoring for consistent locking disciplines

#### 3.2.2 Generating Relevant Executions — Fuzzing

****Fuzz Testing**** generates inputs to discover new vulnerabilities:

| Type | Approach |
|------|----------|
| **Black-box fuzzing** | Depends only on I/O behaviour: random, model-based (grammar-driven), or mutation-based |
| **White-box fuzzing** | Analyses internal program structure. Key technique: ****Dynamic Symbolic Execution**** — builds path conditions (logical constraints) and solves for inputs that drive new execution paths |

---

## 4. Mitigating Exploitation of Vulnerabilities

Even with prevention and detection, legacy code with vulnerabilities will remain. Mitigation techniques are implemented in the execution infrastructure (hardware, OS, VM, compiler).

### 4.1 Runtime Detection of Attacks

Detect attacks by monitoring execution for property violations:

| Technique | What It Detects |
|-----------|----------------|
| ****Stack Canaries**** | Corruption of activation records; detects return address modification |
| **NX (No-eXecute) / DEP** | Direct code injection — prevents execution from data memory |
| ****Control-Flow Integrity (CFI)**** | Code-reuse attacks — monitors whether runtime control flow complies with expected CFG |

On detection, the typical response is **program termination** (protects against further damage but impacts availability).

### 4.2 Automated Software Diversity

Exploitation often relies on implementation details (memory layout, database specifics). **Diversifying** these details raises the bar:

- ****ASLR** (Address Space Layout Randomization)** — randomises code, stack, and heap layout
  - Coarse-grained: random base addresses for segments
  - Fine-grained: random addresses for individual functions, stack frames, or heap objects
- Compilation-time / installation-time diversification
- Challenge: complicates software maintenance (bug reports, updates)

### 4.3 Limiting Privileges

Constrain what exploited software can do:

- ****Sandboxing**** — execute software in a controlled environment with resource access policies (VM, OS process, jails, Java sandbox)
- **Compartimentalisation** — divide software into compartments, each with bounded privileges (e.g., browser rendering engine denied filesystem access)
- **Object-capability systems** — each application-level object is its own protection domain (finest granularity)
- For side-channels: **isolation** on separate cores or hardware

### 4.4 Software Integrity Checking

Under the umbrella of ****Trusted Computing****: measure system state and enforce access only from trusted states.

- **Trusted Boot** — accumulates measurements for each executed program; modifications (e.g., from attacks) produce different measurements
- Secret keys accessible only from a state with a specified measurement

---

## 5. Summary: Vulnerability Categories vs. Countermeasures

| Vulnerability Category | Prevention | Detection | Mitigation |
|------------------------|-----------|-----------|------------|
| **Memory Management** | Memory-safe languages, fat/smart pointers, coding rules | Static + dynamic detection techniques | Stack canaries, NX, CFI, ASLR, sandboxing |
| **Structured Output Generation** | Regular expression types, LINQ, Prepared Statements | Taint analysis | Runtime detection |
| **Race Conditions** | Ownership types, coding guidelines | Static + dynamic detection | Sandboxing |
| **API Vulnerabilities** | Contracts, usable APIs, defensive implementations | Runtime checking of pre/post-conditions, static contract verification | Compartimentalisation |
| **Side-Channel Vulnerabilities** | Coding guidelines | Static detection | Isolation |

---

## Further Reading

- **Building Secure Software** (Viega & McGraw, 2002) — first book on developing secure programs
- **24 Deadly Sins of Software Security** (Howard, LeBlanc & Viega) — updated catalogue of vulnerabilities
- **The Art of Software Security Assessment** (Dowd, McDonald & Schuh) — detailed vulnerability class descriptions
- **Surreptitious Software** (Collberg & Nagra) — obfuscation, watermarking, tamperproofing
- ****OWASP** Resources** — practice-oriented guides, tools, and awareness instruments for application security
