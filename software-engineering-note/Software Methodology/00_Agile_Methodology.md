---
tags: [agile, scrum, xp, tdd, methodology, software-methodology]
---

# Agile Methodology

> *Source: Clean Agile: Back to Basics by Robert C. Martin (2019)*

## Purpose

Agile is a set of values and principles for software development that emphasizes adaptability, collaboration, and delivering working software in short iterations. This note covers the Agile Manifesto, Scrum framework, Extreme Programming (XP) practices, and the technical discipline that makes Agile sustainable.

## The Agile Manifesto

### Four Values
> We are uncovering better ways of developing software by doing it and helping others do it. Through this work we have come to value:

| Value | Over |
|---|---|
| **Individuals and interactions** | processes and tools |
| **Working software** | comprehensive documentation |
| **Customer collaboration** | contract negotiation |
| **Responding to change** | following a plan |

That is, while there is value in the items on the right, we value the items on the left more.

### Twelve Principles
1. **Customer satisfaction** through early and continuous delivery of valuable software
2. **Welcome changing requirements**, even late in development
3. **Deliver working software frequently** (weeks rather than months)
4. **Business people and developers** must work together daily
5. **Build projects around motivated individuals** — give them the environment and support they need
6. **Face-to-face conversation** is the most efficient and effective method of conveying information
7. **Working software** is the primary measure of progress
8. **Sustainable development** — sponsors, developers, and users should maintain a constant pace indefinitely
9. **Continuous attention** to technical excellence and good design
10. **Simplicity** — maximizing the amount of work not done
11. **Self-organizing teams** produce the best architectures, requirements, and designs
12. **Regular reflection** on how to become more effective, then tune and adjust behavior

## Scrum Framework

### Roles
- **Product Owner** — Owns the product backlog, prioritizes features, represents stakeholders
- **Scrum Master** — Facilitates Scrum events, removes impediments, coaches the team
- **Development Team** — Self-organizing, cross-functional, 5–9 members

### Artifacts
- **Product Backlog** — Ordered list of everything needed in the product; dynamic, always evolving
- **Sprint Backlog** — Items selected for the current sprint + plan for delivering them
- **Increment** — The sum of all backlog items completed during a sprint and all prior sprints

### Events
- **Sprint** — Timeboxed iteration (1–4 weeks) that produces a potentially releasable increment
- **Sprint Planning** — Team selects items from the product backlog for the sprint
- **Daily Scrum** — 15-minute daily synchronization (What did I do? What will I do? Any impediments?)
- **Sprint Review** — Demonstrate the increment to stakeholders; get feedback
- **Sprint Retrospective** — Inspect how the team worked; identify improvements

### The Iron Triangle
Traditional project management: scope, schedule, cost are fixed; quality varies.
Agile inversion: **quality is fixed**; scope, schedule, and cost are negotiable.

## Extreme Programming (XP) Practices

### Planning
- **User Stories** — Short descriptions of functionality from the user's perspective: "As a [role], I want [feature] so that [benefit]"
- **Story Points** — Relative sizing of stories (Fibonacci: 1, 2, 3, 5, 8, 13, 21)
- **Velocity** — How many story points the team completes per sprint; used for forecasting
- **Trivariate Analysis** — Estimating with best-case, worst-case, and most-likely scenarios

### Pair Programming
- Two developers, one computer — one writes code, other reviews in real-time
- Pairs rotate frequently (every 1–2 hours or daily)
- Benefits: immediate code review, knowledge sharing, better design decisions, reduced defects

### Test-Driven Development (TDD)
1. **Red** — Write a failing test that defines the desired behavior
2. **Green** — Write the minimum code to make the test pass
3. **Refactor** — Improve the code while keeping all tests green

This cycle repeats every few minutes. The result: a comprehensive test suite that documents the code and enables fearless refactoring.

### Continuous Integration (CI)
- Integrate code into the main branch multiple times per day
- Each integration triggers automated build and test
- Catches integration problems early; enables continuous delivery

### Refactoring
- Improving the internal structure of code without changing its external behavior
- Done continuously, not in a separate "refactoring phase"
- Tests provide the safety net; CI catches regressions
- The Boy Scout Rule: leave the code cleaner than you found it

## The Agile Hangover

Robert C. Martin's critique of how Agile has been corrupted:

> "What is in the world of Agile development is nothing compared to what should be."

Common corruptions:
- **Certification mills** — Two-day Scrum Master certifications replacing years of experience
- **Process over craft** — Scrum without technical practices (TDD, CI, refactoring)
- **Velocity gaming** — Story points become a management metric, not a planning tool
- **No technical discipline** — "We're Agile" without tests, without refactoring, without CI

The fix: return to the basics. Agile without technical discipline is just chaos with standups.

## Agile vs. Waterfall vs. V-Model

| Aspect | Waterfall | V-Model | Agile |
|---|---|---|---|
| **Approach** | Sequential phases | Sequential with verification at each level | Iterative, incremental |
| **Requirements** | Fixed upfront | Fixed upfront | Evolving |
| **Testing** | After implementation | Paired with each phase | Continuous (TDD) |
| **Delivery** | End of project | End of project | Every sprint |
| **Risk** | Late discovery | Early verification, late delivery | Early and continuous |
| **Best for** | Fixed scope, regulated, safety-critical | Safety-critical, regulated | Evolving requirements, fast feedback |
| **Documentation** | Heavy | Heavy | Lightweight, working code as documentation |
| **Change cost** | High | High | Low (embraced) |

## Related

- [[00_Lean_Methodology]] — Lean principles that underpin Agile
- [[02_Methodologies_Overview]] — Comparison across all methodologies
- [[03_Kanban_and_Flow]] — Kanban as an alternative/complement to Scrum
