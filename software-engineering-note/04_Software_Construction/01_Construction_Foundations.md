---
tags:
  - construction
  - fundamentals
  - prerequisites
  - code-complete
  - software-engineering
source: "McConnell, Code Complete 2nd Edition, Chapters 1–4"
created: 2026-07-21
---

# 01 Construction Foundations

> **Source:** McConnell, *Code Complete* (2nd Ed.), Chapters 1–4
> **Purpose:** Establish what software construction is, the mental models (metaphors) that shape how we think about it, the upstream prerequisites that must be in place before coding begins, and the key decisions every developer and technical lead must make at the start of construction.

---

## Chapter 1 — What Is Software Construction?

### Definition

Software construction is the **central activity** in software development. It is primarily **coding and debugging** but also encompasses:

- Detailed design
- Construction planning
- Unit testing
- Integration and integration testing
- Code review
- Developer testing
- Performance tuning

Construction is **not** requirements development, software architecture (high-level design), system testing (independent QA), user-interface design, management, or corrective maintenance — though it interacts with all of them.

### Why Construction Matters

| Reason | Explanation |
|--------|-------------|
| **It's the largest single activity** | Construction typically consumes **30–80%** of total project time |
| **It's the central activity** | Requirements and architecture feed *into* it; system testing verifies *what comes out* of it |
| **Individual productivity varies enormously** | Studies show 10–20× differences among programmers during construction (Sackman, Erikson, Grant 1968; confirmed by Curtis, Mills, DeMarco, Boehm) |
| **Source code is often the only accurate description** | Requirements and design docs go stale; the code is always current |
| **It's the only activity guaranteed to happen** | Even projects that skip requirements and testing still construct code |

### Specific Construction Tasks

- Verifying that groundwork (prerequisites) is laid so construction can succeed
- Determining how code will be tested
- Designing and writing classes and routines
- Creating and naming variables and named constants
- Selecting control structures and organizing blocks of statements
- Unit testing, integration testing, and debugging your own code
- Reviewing team members' low-level designs and code
- Polishing code (formatting, commenting)
- Integrating separately created software components
- Tuning code for speed and resource use

---

## Chapter 2 — Metaphors for Software Development

Metaphors are **heuristics, not algorithms**. They don't give you the answer — they tell you *how to look* for it. Good metaphors are simple, relate well to other metaphors, and explain observed phenomena. Software development is too young to have standard metaphors, so using the right ones (and knowing their limits) shapes how effectively you think about the craft.

### Common Software Metaphors

| Metaphor | Core Idea | Strengths | Weaknesses |
|----------|-----------|-----------|------------|
| **Writing Code (Penmanship)** | Software is like writing a letter — sit down and write start to finish | Fine for solo, small-scale projects | Implies a one-person, one-pass activity; ignores maintenance (90% of effort is post-release); dismisses planning |
| **Farming (Growing a System)** | Design a piece, code a piece, test a piece — incremental growth | Captures incremental technique | Weak analogy; suggests you lack control over development (crop yields, weather); hard to extend productively |
| **Oyster Farming (Accretion / Incremental Development)** | Gradually add small amounts, like an oyster forming a pearl | Directly maps to incremental/iterative delivery; doesn't overpromise | Narrower than construction metaphor; mostly about the *pace* of development |
| **Building Construction** | Software is like building a structure — planning, preparation, and execution scale with size | **Most powerful metaphor.** Explains why small and large projects differ, why planning pays off, why buying beats building, why change costs differ | Like any metaphor, can be overextended |
| **Intellectual Toolbox** | Effective developers collect techniques the way a craftsman collects tools | Keeps all methods in perspective; no single methodology fits every problem | Requires judgment to know which tool for which job |

### Building Construction → Software Parallels

| Building | Software |
|----------|----------|
| Deciding what kind of house → | **Problem definition** |
| Architect's general design → | **Software architecture** |
| Detailed blueprints, contractor → | **Detailed design** |
| Site prep, foundation, framing, roofing, plumbing, wiring → | **Software construction** |
| Landscaping, painting, decorating → | **Software optimization** |
| Inspector walkthroughs → | **Reviews and inspections** |
| Buying pre-made cabinets and appliances → | **Using libraries and off-the-shelf components** |
| Custom cabinetry for high-end homes → | **Building custom components for first-class products** |

> **Key Insight:** A doghouse needs a different approach than a skyscraper. The penalty for failure scales with size — over-engineering is warranted in extremely large structures. The same is true for a 1,000-line script vs. a 1,000,000-line system.

### Key Point: Combining Metaphors

Metaphors are not mutually exclusive. Use whatever combination stimulates your thinking. The construction metaphor and the toolbox metaphor are especially complementary — one explains *process structure*, the other explains *technique selection*.

---

## Chapter 3 — Upstream Prerequisites: Measure Twice, Cut Once

> The overarching goal of preparation is **risk reduction**. Clear out major risks as early as possible so the bulk of the project proceeds smoothly. The most common risks are **poor requirements** and **poor project planning**.

### The Three Prerequisites

```
Problem Definition  →  Requirements  →  Architecture  →  Construction
   (what problem?)      (what system?)    (how to build?)
```

### 3.1 Problem Definition

A clear statement of the problem **without reference to any solution**. It should be 1–2 pages, in the user's language, from the user's point of view.

| Good (Problem) | Bad (Solution in Disguise) |
|---|---|
| "We can't keep up with orders for the Gigatron" | "We need to optimize our automated data-entry system" |

> If you don't define the problem, you may waste time solving the **wrong** problem — and you also fail to solve the **right** one.

### 3.2 Requirements

Explicit requirements ensure the **user** drives functionality, not the programmer. They minimize arguments and reduce costly downstream changes.

#### Why Requirements Matter — The Cost of Defects

| Defect Introduced In → Detected In | Architecture | Construction | System Test | Post-Release |
|------------------------------------|:-----------:|:-----------:|:----------:|:----------:|
| **Requirements** | 3× | 5–10× | 10× | 10–100× |
| **Architecture** | — | 10× | 15× | 25–100× |
| **Construction** | — | — | 10× | 10–25× |

> A requirements error caught post-release can cost **100×** more to fix than if caught during requirements development.

#### The Myth of Stable Requirements

- Average project experiences **~25% requirements change** during development
- Requirements changes account for **70–85%** of rework
- Customers learn what they need *by seeing the software* — changes are inevitable

#### Handling Requirements Changes

1. **Use the requirements checklist** — if requirements aren't good enough, stop and fix them
2. **Make everyone know the cost** — "schedule" and "cost" are sobering words for feature-intoxicated stakeholders
3. **Establish a change-control procedure** — review proposed changes at defined times
4. **Use iterative/evolutionary approaches** — short cycles, fast feedback
5. **Dump the project** if requirements are too bad or volatile
6. **Keep your eye on the business case** — evaluate features by "incremental business value"

#### Requirements Checklist Highlights

- All inputs/outputs specified (source, accuracy, range, frequency, format)
- All external interfaces and communication protocols specified
- Nonfunctional requirements: response time, security, reliability, memory, maintainability
- Requirements are in the **user's language**, testable, and at a consistent level of detail
- Every requirement is **relevant and traceable** to its origin in the problem domain

### 3.3 Architecture (High-Level Design)

Architecture is the **frame** that holds the more detailed parts of the design. Its quality determines the **conceptual integrity** of the system. Good architecture makes construction easy; bad architecture makes it almost impossible.

#### Architectural Components to Verify

| Component | What to Look For |
|-----------|-----------------|
| **Program Organization** | Clear overview; well-defined building blocks (classes/subsystems); communication rules between blocks |
| **Major Classes** | 80/20 rule — specify the 20% of classes that account for 80% of behavior; rationale for chosen organization |
| **Data Design** | Major files and table designs with justification; which subsystem owns which data |
| **Business Rules** | Impact of key business rules on system design |
| **User Interface** | Modularized so UI can be swapped without affecting business logic |
| **Resource Management** | Plan for scarce resources (threads, DB connections, handles, memory) |
| **Security** | Threat model; buffer handling; rules for untrusted data; encryption |
| **Performance** | Goals; time/space budgets per class; risk areas identified |
| **Error Processing** | Consistent, systemwide strategy (corrective vs. detective, active vs. passive, propagation, logging, exceptions) |
| **Fault Tolerance** | Detection, recovery, containment; voting algorithms; degraded operation |
| **Overengineering** | Explicit guidance on whether to err on robust or simple |
| **Buy vs. Build** | Justification for custom code over off-the-shelf components |
| **Change Strategy** | How likely enhancements map to easy-to-change areas; delayed-commitment techniques |

#### Architecture Quality Signals

- "Hangs together" conceptually — looks natural, not forced
- Objectives are clearly stated (modifiability vs. performance vs. other goals)
- All major decisions have **stated motivations** (no "we've always done it that way")
- Largely machine- and language-independent
- Explicitly identifies **risky areas** and mitigation steps
- Contains **multiple views** (like house plans: elevations, floor plan, electrical)
- You, the implementer, feel **comfortable** with it

### 3.4 Time Allocation & Project Type

| Factor | More Sequential (Up-Front) | More Iterative (As-You-Go) |
|--------|---------------------------|---------------------------|
| Requirements stability | Stable | Unstable / poorly understood |
| Design complexity | Straightforward, understood | Complex, challenging |
| Team experience | Familiar with domain | Unfamiliar with domain |
| Risk level | Low | High |
| Change cost downstream | High | Low |
| Predictability need | Important | Less important |

> Well-run projects devote **10–20%** of effort and **20–30%** of schedule to requirements, architecture, and up-front planning. Most projects benefit from iterative approaches, but iterative ≠ skipping prerequisites.

### Upstream Prerequisites Checklist

- Have you identified the **kind of project** and tailored your approach?
- Are **requirements** well defined and stable enough to begin?
- Is **architecture** well defined enough to begin?
- Have other **unique risks** been addressed so construction isn't exposed to unnecessary risk?

---

## Chapter 4 — Key Construction Decisions

Once prerequisites are in place, the focus shifts to construction-specific choices.

### 4.1 Choice of Programming Language

Language choice affects **productivity and quality** in measurable ways:

| Factor | Finding |
|--------|---------|
| **Familiarity** | Programmers with 3+ years in a language are ~30% more productive than those new to it (Cocomo II) |
| **Expressiveness** | High-level languages (C++, Java, Python, Smalltalk) improve productivity **5–15×** over C/assembly |
| **Thinking** | The Sapir-Whorf hypothesis applied: the language you use shapes the thoughts you can express |

#### Language Expressiveness (Relative to C)

| Language | Level Relative to C |
|----------|:-------------------:|
| C | 1 |
| Fortran 95 | 2 |
| C++ | 2.5 |
| Java | 2.5 |
| Visual Basic | 4.5 |
| Perl | 6 |
| Python | 6 |
| Smalltalk | 6 |

> **"Programming into a language" vs. "Programming in a language":** Decide what thoughts you want to express first, *then* figure out how to express them in the available language. Don't let primitive tooling limit your thinking.

### 4.2 Programming Conventions

High-quality software requires **conceptual integrity** at both the architectural level and the low-level implementation level. Establish conventions **before construction begins** — they are nearly impossible to retrofit:

- Variable naming conventions
- Class and routine naming conventions
- Formatting (layout) conventions
- Commenting conventions
- Error-handling conventions
- Interface conventions

> Without unifying conventions, your program becomes a collage of arbitrary style variations that tax the brain for no reason. One key to successful programming is **avoiding arbitrary variations** so your brain can focus on variations that are *really needed*.

### 4.3 Your Location on the Technology Wave

| | Early Wave | Late Wave |
|---|---|---|
| **Language choices** | Few, buggy, poorly documented | Many, stable, well-documented |
| **Tools** | Primitive debuggers, no optimizers | Comprehensive, integrated |
| **Reference material** | Sparse, unreliable | Vendor docs, third-party books, FAQs, training |
| **Day-to-day** | Working around tool bugs, library defects | Steadily writing new functionality |
| **Innovation** | Highest potential for breakthrough apps | Focus on refinement and productivity |

If you're in an early-wave environment, the practices in this book help **even more** — compensate for weak tools with strong discipline.

### 4.4 Selection of Major Construction Practices

No single project can use every good practice. **Consciously choose** which to emphasize:

| Category | Decisions to Make |
|----------|------------------|
| **Coding** | How much design up front vs. at the keyboard? What naming/comment/layout conventions? How will error handling, security, class interfaces, and reuse standards work? |
| **Teamwork** | Integration procedure (check-in steps)? Pair programming, solo, or both? |
| **Quality Assurance** | Test-first development? Unit tests? Debugger step-through? Integration tests before check-in? Code reviews or inspections? |
| **Tools** | Revision control? Language/compiler version? Framework (J2EE, .NET)? Nonstandard language features? Editor, debugger, test framework, refactoring tool, syntax checker? |

---

## Key Takeaways (Chapters 1–4)

1. **Construction is the central, guaranteed activity** — improving it improves any project, no matter how abbreviated.
2. **Metaphors shape how you think.** The building-construction metaphor is the most powerful, and the intellectual-toolbox metaphor keeps methods in perspective. Use metaphors as heuristics, not algorithms.
3. **Upstream quality is cheaper.** Fixing a requirements defect post-release costs 10–100× more than fixing it during requirements. The food chain matters: bad requirements → bad architecture → bad construction.
4. **Tailor prerequisites to your project type.** Life-critical systems demand more sequential, rigorous prerequisites; business systems benefit from iterative approaches. But iterative ≠ skipping preparation.
5. **Establish conventions before you code.** Naming, formatting, error handling — retrofitting is nearly impossible. Avoid arbitrary variation.
6. **Program *into* a language, not *in* it.** The language constrains syntax, not your design thinking.
7. **Consciously choose your practices.** Pair programming, TDD, inspections, CI — pick what fits your project, team, and technology wave. No single set is universally right.

---

## Related Notes

- [[Software Construction Overview|Software Construction Overview (SWEBOK)]]
- Code Complete Ch 5–9 → *Design in Construction* (detailed design, working classes, high-quality routines)
- [[../Software Construction Overview|← Back to Construction Overview]]
