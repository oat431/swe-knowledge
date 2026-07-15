---
tags:
  - programming
  - fundamental
  - concepts
---

# 01 Core Concepts

A program is a set of instructions that transforms input into output. Understanding how source code becomes an executing process — and the paradigms that shape how we write it — is the first step toward mastering any language.

---

## What Is a Program?

At its simplest, a program follows this pipeline:

1. **Source code** — human-readable text written in a programming language.
2. **Translation** — compiled, interpreted, or JIT-compiled into something the CPU can execute.
3. **Execution** — the translated instructions run on a processor, manipulating data in memory.

---

## Compilation vs Interpretation vs JIT

| Approach | How It Works | Startup Time | Runtime Performance | Examples |
|---|---|---|---|---|
| **Compiled** | Source → machine code (ahead of time) | Slow (compile step) | Fast (native code) | C, C++, Rust, Go |
| **Interpreted** | Source → executed line-by-line by an interpreter | Fast | Slower (overhead per instruction) | Python, Ruby, PHP |
| **JIT (Hybrid)** | Source → bytecode → machine code at runtime (hot paths) | Medium | Fast after warm-up | Java, C#, JavaScript (V8) |

### Compiled Languages

The compiler translates the entire source file into a platform-specific binary before anything runs. This enables heavy optimisation but produces a platform-dependent executable.

```java
// Java compiles to bytecode (.class), not native machine code
javac Main.java    # compile
java Main          # run on JVM
```

```bash
# C compiles directly to machine code
gcc main.c -o main
./main
```

### Interpreted Languages

The interpreter reads and executes the source (or an intermediate representation) directly. No separate compilation step — just run.

```python
# Python — interpreted (CPython compiles to .pyc bytecode internally)
python main.py
```

### JIT (Just-In-Time) Compilation

Languages like Java and C# compile to an intermediate bytecode, which the runtime then compiles to native code at execution time. Hot code paths are optimised aggressively, combining portability with near-native speed.

---

## The Execution Model

Whether compiled or interpreted, most languages pass through a similar pipeline internally:

```
Source Code
  → Lexer (tokenisation)
    → Parser (syntax analysis)
      → AST (Abstract Syntax Tree)
        → IR (Intermediate Representation)
          → Machine Code / Bytecode
```

| Stage | Responsibility |
|---|---|
| **Lexer** | Breaks raw text into tokens (`if`, `(`, `x`, `)`, `{`, `}`) |
| **Parser** | Builds a tree structure respecting grammar rules |
| **AST** | Represents the program's structure semantically |
| **IR** | A language- and platform-independent representation optimised for analysis |
| **Code generation** | Translates IR into target machine code or bytecode |

Understanding this pipeline helps when debugging cryptic compiler errors — the error always references a specific stage.

---

## Programming Paradigms

| Paradigm | Core Idea | Focus | Examples |
|---|---|---|---|
| **Imperative** | Describe *how* — step-by-step state changes | Mutation, sequencing | C, Pascal |
| **Object-Oriented** | Encapsulate state + behaviour in objects | Abstraction, polymorphism | Java, C#, C++ |
| **Functional** | Describe *what* — pure transformations, no mutation | Immutability, composition | Haskell, Clojure, (partial: JS, Python) |
| **Declarative** | Describe the desired result, not the steps | Intent over implementation | SQL, HTML, Prolog |

Most modern languages are **multi-paradigm**. Java supports imperative + OOP (and increasingly functional via lambdas). Python supports all four. Choosing the right paradigm for the problem is a hallmark of mature engineering.

> For a deep dive into OOP specifically, see [[06 Object-Oriented Programming]].

---

## Common Pitfalls

- **Confusing compilation with interpretation** — Java is *compiled* (to bytecode) then *interpreted/JIT-compiled* (by the JVM). It is not purely either.
- **Thinking paradigms are mutually exclusive** — real-world code mixes paradigms constantly.
- **Ignoring the execution model** — understanding lexing/parsing helps you read compiler errors and write cleaner code.

---

## Sources

- *Crafting Interpreters* — Robert Nystrom (craftinginterpreters.com)
- *Compilers: Principles, Techniques, and Tools* — Aho, Lam, Sethi, Ullman
- *Structure and Interpretation of Computer Programs* (SICP)
- Oracle JVM Specification
