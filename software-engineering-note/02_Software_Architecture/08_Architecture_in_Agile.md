---
tags:
  - architecture
  - agile
  - methodology
  - software-architecture
  - SAiP
source: "Bass, Clements & Kazman — Software Architecture in Practice, Chapter 15"
---

# 08 — Architecture in Agile Projects

> **Source:** Bass, Clements & Kazman, *Software Architecture in Practice* (SAiP), Chapter 15
> **Purpose:** Understand how architectural practices and Agile methodologies can be blended — finding the "sweet spot" between up-front planning and iterative delivery, and equipping the architect with practical guidelines for Agile contexts.

---

## 15.1 How Much Architecture?

### The Garden Shed vs. the Skyscraper

The chapter opens with an analogy: building a *garden shed* requires little up-front planning — a mental image, some wood, hammers, and saws. The process is agile: self-organizing, close to the customer, accommodating late-breaking feature requests. Would you build a 20-story office building this way? No — that demands BDUF (Big Design Up Front): architects, structural engineers, soil analysis, snow-load calculations.

So too with software: **the amount of up-front planning and analysis should be justified by the potential risks.** Everything in architecture is about cost/benefit tradeoffs.

### User Stories and Cross-Cutting Concerns

A tension exists between Agile's user-story-driven approach and architectural thinking:

- **User stories** describe features visible to the user — demonstrating progress to the customer.
- This can lead to an architecture where every feature is independently designed and implemented.
- **Cross-cutting concerns** (shared utility functions, common infrastructure) become hard to capture without coordination across feature teams.
- In a geographically distributed, large system, feature-first delivery causes **massive coordination problems**.
- An architecture-centric project uses **layered architecture** to solve this — features on upper layers using shared lower-layer functionality — but this requires up-front planning, design, and feature analysis.

> Successful projects need a successful **blend** of the two approaches. This is not and never should be an either/or choice.

- **Too much up-front planning** → stifles creativity, inability to adapt to changing requirements.
- **Too much agility** → chaos.
- **The goal:** George Fairbanks' *"just enough architecture"* — doing the right amount of architecture work, at the right time.

### An Analytic Perspective: Boehm & Turner's COCOMO II Analysis

Boehm and Turner analyzed historical data from **161 industrial projects**, examining the effects of up-front architecture and risk resolution effort (the COCOMO II scale factor called **RESL** — architecture and risk resolution).

Two activities add time to the project schedule:

1. **Up-front design work** on architecture and risk identification/planning/resolution
2. **Rework** due to fixing defects and addressing modification requests

These trade off against each other: the more you invest in planning, the less (hopefully) rework is needed.

#### The Sweet Spot Graph (Figure 15.1)

Boehm and Turner plotted three hypothetical projects against architecture effort (5–50% of project schedule):

| Project Size | Sweet Spot (Architecture % of Schedule) | Interpretation |
|---|---|---|
| **10 KSLOC** | ~5% (far left) | Up-front architecture work is mostly a *waste* for small projects |
| **100 KSLOC** | ~20% | Moderate up-front investment pays off |
| **10,000 KSLOC** | ~40% | Large, complex systems need substantial up-front architecture |

> A project with a million lines of code is enormously complex — Agile principles alone cannot cope with this complexity without architecture to guide and organize the effort.

**No single answer fits all situations.** Beyond lines of code, other determinants include:
- Domain complexity
- Required reliability/safety
- Development team experience

> The whole point of budgeting for architecture is to **reduce risk** — financial, political, operational, reputational, or risks involving human life.

---

## 15.2 Agility and Architecture Methods

The authors' architecture methods (design, analysis, documentation) are:
- Based on **essential elements** needed to perform the activity
- Driven by motivation to **reduce risk**
- Governed by **cost/benefit considerations**

The greatest Agile friction is expected around **documentation** and **evaluation**.

### Architecture Documentation and YAGNI

The **Views and Beyond** approach to documentation (Chapter 18) aligns with Agile through one core principle:

> **If information isn't needed, don't spend the resources to document it.**

This is the same idea as **YAGNI** ("You Ain't Gonna Need It") — only implement or document something when you actually have the need. Do not spend time anticipating all possible needs.

**Views and Beyond principles:**
- Select a view **if and only if** it addresses substantial concerns of an important stakeholder community.
- Documentation is **not a monolithic activity** that holds up all progress — produce views in **prioritized stages** for stakeholders who need them *now*.
- Document what you need to **teach newcomers**, what embodies **significant risks**, and what you need to **change frequently**.
- The reader might be a **maintainer** assigned years after the original team has disbanded — this extends beyond classic Agile's minimum-documentation-for-current-team philosophy.

### Architecture Evaluation in Agile

An architecture evaluation is **perfectly Agile-consistent** because meeting stakeholders' important concerns is a cornerstone of Agile philosophy.

The **ATAM** (Architecture Tradeoff Analysis Method, Chapter 21):
- Does **not** analyze all or even most of an architecture.
- Focus is determined by **quality attribute scenarios** representing the *most important* stakeholder concerns.
- "Most important" = highest value to stakeholders OR greatest risk in achieving the scenario.
- After scenarios are elicited, validated, and prioritized → an **evaluation agenda** based on what matters most.
- Only delves into areas that pose **high risk** for achieving main functions and qualities.
- Can be tailored to a **lightweight** version for quicker, less-costly analysis and feedback at any point in the project.

---

## 15.3 A Brief Example of Agile Architecting — WebArrow

The **WebArrow** web-conferencing system illustrates agile architecting in practice.

### Domain Challenges

Web-conferencing systems are complex and demanding:
- Must work on a wide variety of hardware/software platforms (not under architect's control)
- Require **low-latency** real-time responsiveness (VoIP, screen sharing)
- Need **high security** over unknown network topologies and firewall policies
- Must be **easily modifiable** and integrable into diverse environments
- Must be **highly usable** for users with widely varying IT skills

These goals **trade off** against each other: security (encryption) vs. real-time performance; modifiability vs. time-to-market; availability/performance vs. cost.

Additionally:
- Stringent **time-to-market** constraints prevented exhaustive requirements analysis.
- Trying to support all possible uses was **intractable**.
- Users were poorly equipped to envision all potential uses.
- Vendor APIs sometimes didn't work as specified, or critical APIs were missing → workarounds needed *fast*.

### The Two-Mode Approach

The WebArrow team worked in **two modes simultaneously**:

| Mode | Activity |
|---|---|
| **Top-down** | Designing and analyzing architectural structures to meet demanding QA requirements and tradeoffs |
| **Bottom-up** | Analyzing implementation-specific and environment-specific constraints and fashioning solutions |

### Spikes as the Key Mechanism

The team adopted an **agile architecture discipline combined with a rigorous program of experiments** (Agile "spikes") aimed at answering specific tradeoff questions. Experiments turned unknown architectural parameters into constants or ranges.

**Process:**
1. Quickly create and crudely analyze an **initial architecture concept**; implement incrementally starting with most critical customer-visible functionality.
2. **Adapt** the architecture and **refactor** design/code whenever new requirements emerged or better understanding developed.
3. **Continuous experimentation**, empirical evaluation, and architecture analysis guided decisions as the product evolved.

**Example spike questions:**
- Would moving to a distributed database from local flat files negatively impact latency?
- What scalability improvement would result from `mod_perl` vs. standard Perl?
- How many participants could a single meeting server host?
- What was the correct ratio between database servers and meeting servers?

These questions are **difficult to answer analytically** — answers depend on third-party component behavior and performance characteristics with no standard analytic models.

**Solution:** Build an extensive **testing infrastructure** (simulation + instrumentation) to compare performance of each modification against the base system — determining the effect of each proposed improvement *before* committing it.

### Agile Principles Alignment

The WebArrow approach aligned with these Agile principles:
1. **Principle 1** — Early and continuous delivery of working software
2. **Principle 2** — Welcoming changing requirements
3. **Principle 3** — Delivering working software frequently
4. **Principle 8** — Sustainable development at a constant pace
5. **Principle 9** — Continuous attention to technical excellence and good design

> Making architecture processes agile does **not** require radical re-invention of either Agile practices or architecture methods. The emphasis on **experimentation** proved the key factor.

---

## 15.4 Guidelines for the Agile Architect

### The Incremental Commitment Model (Boehm et al.)

Barry Boehm's **Incremental Commitment Model** is a hybrid process framework balancing agility and commitment, based on six principles:

1. **Commitment and accountability** of success-critical stakeholders
2. Stakeholder **"satisficing"** (meeting acceptability thresholds) based on success-based negotiations and tradeoffs
3. **Incremental and evolutionary growth** of system definition and stakeholder commitment
4. **Iterative** system development and definition
5. **Interleaved** system definition and development — allowing early fielding of core capabilities, continual adaptation, and timely growth without waiting for every requirement
6. **Risk management** — risk-driven anchor point milestones to synchronize and stabilize concurrent activity

### Grady Booch's Guidelines

Booch claims that **all good software-intensive architectures are agile** — meaning:

| Property | Meaning |
|---|---|
| **Resilient and loosely coupled** | Composed of a core set of well-reasoned design decisions with "wiggle room" for modifications and refactorings |
| **Incremental growth** | Architecture grows incrementally as the system is developed and matures |
| **Decomposability** | Separation of concerns and near-independence of parts (all modifiability tactics) |
| **Visible and self-evident in code** | Design patterns, cross-cutting concerns, and important decisions are obvious, well-communicated, and defended |
| **Socialized** | The architect must actively communicate and advocate for the architecture |

### Technical Debt (Ward Cunningham)

**Technical debt** = the software equivalent of consumer debt: "purchasing something now and hoping to pay for it later."

- **Quick-and-dirty implementation** → incurs technical debt → increased future maintenance costs.
- When technical debt becomes **unacceptably high**, projects must **pay it down** through **refactoring** — a key part of every agile architecting process.

### Practical Advice

The authors distill three tiers of guidance:

**1. Large, complex systems with relatively stable, well-understood requirements:**
> Do a **large amount** of architecture work up front. (See Figure 15.1 for sample values — ~40% of schedule for 10M SLOC.)

**2. Big projects with vague or unstable requirements:**
> Start by quickly designing a **complete candidate architecture** — even if it's just a "PowerPoint architecture" leaving out many details, even if designed in just a couple of days. This is similar to Cockburn's *"walking skeleton"* — enough architecture to demonstrate end-to-end functionality linking major system functions. Be prepared to change and elaborate as circumstances dictate, as you perform spikes and experiments, and as requirements emerge and solidify.

This early architecture helps:
- Guide development
- Enable early problem understanding and analysis
- Assist in requirements elicitation
- Help teams coordinate
- Create coding templates and project standards

**3. Smaller projects with uncertain requirements:**
> At least get agreement on the **central patterns** to be employed, but don't spend too much time on construction, documentation, or analysis up front. Use **lightweight, just-in-time** analysis (Chapter 21).

---

## 15.5 Summary

| Key Point | Detail |
|---|---|
| **Agile and architecture are compatible** | The underlying philosophies are not at odds and can be married to great effect |
| **No either/or** | Successful projects need a blend — too much planning stifles; too much agility is chaos |
| **The sweet spot** | Boehm & Turner's data shows an optimal up-front investment that grows with project size (5% for 10 KSLOC → 40% for 10,000 KSLOC) |
| **Documentation** | Views and Beyond aligns with YAGNI — document only what serves a real stakeholder need, in prioritized stages |
| **Evaluation** | ATAM is inherently Agile-compatible — focuses on high-risk, high-value scenarios, not exhaustive analysis |
| **WebArrow case study** | Top-down + bottom-up modes, experimentation (spikes) as the bridge between architecture and agility |
| **Incremental Commitment Model** | Six principles balancing stakeholder commitment with evolutionary growth |
| **Architect's duties** | Make architecture resilient, loosely coupled, visible in code, and socialized; manage technical debt through refactoring |
| **Practical heuristics** | Vary up-front investment by project size and requirement stability; use walking skeletons for uncertain large projects |

---

## Key Terms

| Term | Definition |
|---|---|
| **BDUF** | Big Design Up Front — traditional approach of exhaustive analysis and design before implementation |
| **YAGNI** | "You Ain't Gonna Need It" — only build/document what you actually need now |
| **RESL** | COCOMO II scale factor for architecture and risk resolution effort |
| **Sweet Spot** | The optimal percentage of project schedule devoted to up-front architecture and risk resolution |
| **Spike** | An Agile experiment designed to answer a specific technical or architectural question |
| **Walking Skeleton** | (Cockburn) Minimal architecture sufficient to demonstrate end-to-end functionality |
| **Technical Debt** | (Cunningham) Future maintenance cost incurred by quick-and-dirty implementation choices |
| **Incremental Commitment Model** | Boehm's hybrid process framework balancing agility with stakeholder commitment |
| **Satisficing** | Meeting an acceptability threshold through success-based negotiations and tradeoffs |

---

## Further Reading (from SAiP Ch 15.6)

- Beck 04 — *Extreme Programming Explained*
- Schwaber 04 — *Agile Project Management with Scrum*
- Palmer 02 — *Feature-Driven Development*
- Cockburn 04 — *Crystal Clear*
- Abrahamsson 10 — IEEE Software special issue on agility and architecture
- Fairbanks 10 — *Just Enough Software Architecture*
- Boehm & Turner 04 — *Balancing Agility and Discipline*
- Boehm, Lane, Koolmanojwong & Turner 10 — Incremental Commitment Model
- Boehm 91 — Seminal article on software risk management
- Carriere, Kazman & Ozkaya 10 — Refactoring based on propagation cost of anticipated changes
- Graham, Kazman & Walmsley 07 — WebArrow case study detail
- Cunningham 92 — Origin of "technical debt" term
- Brown et al. 10 — Economics-driven perspective on agility through architecture
- Nord, Tomayko & Wojcik 04 — SEI architecture methods and Extreme Programming
- Bachmann 11 — Lightweight ATAM for Agile projects

---

## Discussion Questions

1. How would you employ pair programming, frequent team interaction, and dedicated customer involvement in a distributed development environment?
2. What would an "Architecture Manifesto" modeled on the Agile Manifesto look like?
3. How do you budget and schedule Agile projects? Does architecture help or hinder?
4. What are the essential skills for an architect in an Agile vs. non-Agile context?
5. How does the Agile Manifesto's preference for "individuals and interactions over processes and tools" reconcile with modern development tools (IDEs, debuggers, CI/CD, config management)?
6. Critique the Agile Manifesto in the context of a 200-developer, 5-million-line project with a 20-year expected lifetime.


## Related

- [[Software Architecture Overview]] — All architecture topics
- [[07_Design_and_Documentation]] — Designing and documenting
- [[09_Evaluation_and_Governance]] — Architecture evaluation
