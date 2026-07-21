---
tags:
  - legacy-code
  - maintenance
  - change-algorithm
  - testing
  - software-maintenance
---

# Changing Software

> **Source:** Michael Feathers, *Working Effectively with Legacy Code*, Chapters 1–2
> **Purpose:** Understand why we change software, the risks involved, and how testing provides the feedback needed to change legacy code safely.

## Four Reasons to Change Software

All software changes fall into one of four categories:

1. **Adding a feature** — delivering new behavior users request
2. **Fixing a bug** — correcting incorrect existing behavior
3. **Improving the design (refactoring)** — restructuring without changing behavior
4. **Optimizing resource usage** — improving time/memory while preserving behavior

### The Behavior Distinction

The critical technical distinction is not "feature vs. bug" (which is often subjective and organizational) but **adding new behavior vs. changing existing behavior**. Users depend on existing behavior; changing or removing it breaks trust. Adding a method to a class doesn't change behavior *unless the method is called*. Nearly all additions change behavior to some degree — even adding a button subtly alters rendering time.

### What Changes in Each Type

| Change Type | Structure | New Functionality | Existing Functionality | Resource Usage |
|---|---|---|---|---|
| Adding a Feature | Changes | Changes | — | — |
| Fixing a Bug | Changes | — | Changes | — |
| Refactoring | Changes | — | — | — |
| Optimizing | — | — | — | Changes |

The common thread across all four types: **we want to change a small amount of behavior while preserving much more**. Preserving existing behavior is one of the largest challenges in software development.

## Risky Change

To mitigate risk when changing code, ask three questions:

1. **What changes do we have to make?**
2. **How will we know that we've done them correctly?**
3. **How will we know that we haven't broken anything?**

### The Cost of Avoiding Change

Teams often try to minimize risk by minimizing changes ("if it's not broke, don't fix it"). This backfires:

- Existing classes and methods grow larger and harder to understand
- Developers get rusty at making structural changes
- **Fear of change** accumulates, compounding daily
- The gap between "figuring things out" and "making the change" feels like jumping off a cliff

The alternative is not just "try harder with more scrutiny." You need **tests as a safety net**.

## Working with Feedback

### Edit and Pray vs. Cover and Modify

| Approach | Description |
|---|---|
| **Edit and Pray** | Plan carefully, make changes, then manually poke around to see if anything broke. The industry standard — but safety isn't solely a function of care. |
| **Cover and Modify** | Cover code with tests *before* changing it. Tests act as a **software vise** — clamping behavior in place so you change only what you intend to. |

> *"Working with care doesn't do much for you if you don't use the right tools and techniques."*

### Testing to Detect Change

Traditional testing aims to "show correctness." The alternative is **testing to detect change** (regression testing). When tests surround the area you're modifying, they give feedback in **seconds or minutes**, not overnight.

**Thought experiment:** Make a change in a complex function → wait for overnight regression run → next morning, two tests failed, unclear whose change caused it. vs. Make a change, run 20 unit tests instantly, one fails, roll back and fix in under a minute.

## What Is Unit Testing?

Unit tests test the most atomic behavioral units in isolation. Two essential qualities:

1. **They run fast** — a unit test taking 1/10th of a second is *slow*. At that speed, 30,000 tests take an hour. At 1/100th of a second, the same suite takes ~5 minutes.
2. **They help localize problems** — when a test fails immediately after a small change, you know exactly what broke.

### A Test Is NOT a Unit Test If It:

- Talks to a database
- Communicates across a network
- Touches the file system
- Requires special environment configuration to run

Tests that do these things are still valuable, but they should be separated from true unit tests so you always have a fast suite to run on every change.

## Test Coverings and Dependencies

Covering code with tests before changing it catches mistakes. But legacy code presents a chicken-and-egg problem.

### The Legacy Code Dilemma

> When we change code, we should have tests in place. To put tests in place, we often have to change code.

This is the central tension of legacy code work. Classes that depend directly on things hard to use in a test (databases, servlets, GUI classes) are hard to modify. **Dependency is one of the most critical problems in software development** — much legacy code work involves breaking dependencies so that change becomes easier.

### Breaking Dependencies Conservatively

When introducing tests into untested code, use conservative, mechanical refactorings like **Primitivize Parameter** and **Extract Interface**. These initial dependency-breaking changes may leave scars in the code — suspension of aesthetic judgment is necessary:

> *"They are like the incision points in surgery: There might be a scar left in your code after your work, but everything beneath it can get better."*

Once the area is covered by tests, you can heal those scars through further refactoring.

## The Legacy Code Change Algorithm

When making a change in a legacy code base:

1. **Identify change points** — where the code needs to change
2. **Find test points** — where to write tests that will catch regressions
3. **Break dependencies** — make the code testable (conservatively, mechanically)
4. **Write tests** — cover the existing behavior around the change points
5. **Make changes and refactor** — deliver the feature with tests as a safety net

The day-to-day goal: make functional changes that deliver value **while bringing more of the system under test**. Over time, tested areas surface like islands, then continents — work in tested code becomes dramatically easier.

## Key Takeaways

- All software change is about preserving most behavior while altering a small portion
- **Edit and Pray** is the industry default; **Cover and Modify** is the professional alternative
- Unit tests must be fast (<< 0.1s each) and isolated from external resources
- The Legacy Code Dilemma forces us to break dependencies before we can test — do it conservatively
- The five-step algorithm above is the core workflow for any legacy code change
- Fear of change is the real enemy; tests are the antidote


## Related

- [[Software Maintenance Overview]] — All maintenance topics
- [[02_Sensing_and_Seams]] — Seam model and dependency breaking
- [[03_Adding_Features]] — Safe feature addition techniques
- [[04_Getting_Tests_in_Place]] — Getting classes under test
