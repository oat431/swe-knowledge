---
tags:
  - clean-code
  - overview
  - programming
---

# Clean Code Overview

Clean code is code that's easy to read, easy to change, and doesn't surprise the reader. It's not about clever one-liners — it's about making life easier for the next developer (including future you). These notes cover the most impactful clean code practices for daily work.

---

## Why Clean Code?

> "Any fool can write code that a computer can understand. Good programmers write code that humans can understand." — Martin Fowler

| Aspect | ❌ Dirty Code | ✅ Clean Code |
|--------|--------------|---------------|
| Reading speed | Minutes to understand a function | Seconds to grasp intent |
| Bug fixing | Fear of touching anything | Confident, safe changes |
| Onboarding | Weeks to become productive | Days to contribute |
| Refactoring | "Don't touch it, it works" | Continuous improvement |

---

## Topics in This Section

| File | Focus | Key Idea |
|------|-------|----------|
| [[01 Naming & Functions]] | Naming rules & function design | Names reveal intent; functions do one thing |
| [[02 Code Smells & Refactoring]] | Recognizing and fixing bad code | Spot smells early; refactor in small steps |
| [[03 Comments & Documentation]] | When and how to document | Code explains *how*, comments explain *why* |

---

## Core Principles

1. **Readability > Cleverness** — Write code for humans first, compiler second.
2. **DRY** — Don't Repeat Yourself. Duplication is the root of all evil in code.
3. **Single Responsibility** — Every module, class, and function should have one reason to change.
4. **Boy Scout Rule** — Leave the code cleaner than you found it.
5. **Fail Fast** — Surface errors as early as possible.

---

## Related Topics

- [[06 Object-Oriented Programming]] — encapsulation, polymorphism, SOLID principles
- [[05 Functions & Methods]] — deeper dive into function design and composition
- [[09 Functional Programming]] — immutability, pure functions, composition

---

## Quick Checklist

Before merging any PR, ask yourself:

- [ ] Are names descriptive and searchable?
- [ ] Are functions under 20 lines?
- [ ] Is there any duplicated logic?
- [ ] Are there meaningful tests?
- [ ] Did I remove dead code and commented-out blocks?
- [ ] Would a new team member understand this in 6 months?

---

**Sources:** Robert C. Martin, *Clean Code* (2008); Martin Fowler, *Refactoring* (2018)
