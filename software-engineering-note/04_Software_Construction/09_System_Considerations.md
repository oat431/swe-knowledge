---
tags: ["construction", "integration", "management", "tools", "code-complete"]
source: "McConnell, Code Complete, Chapters 27–30"
date: 2026-07-21
---

# 09 System Considerations

A synthesis of Code Complete Chapters 27–30 covering how program size shapes construction, managing the construction process, integration strategies, and programming tools.

---

## Chapter 27 — How Program Size Affects Construction

### 27.1 Communication and Size

Communication paths grow **proportional to the square of team size** (n(n−1)/2):

| Team Size | Communication Paths |
|-----------|---------------------|
| 2 | 1 |
| 5 | 10 |
| 10 | 45 |
| 50+ | 1,200+ |

More paths → more time communicating → more opportunities for miscommunication. **Larger projects demand organizational techniques that streamline or sensibly limit communication.** Formal documentation replaces ad-hoc conversation.

### 27.2 Range of Project Sizes

No single project size is "typical." Rough distribution:

| Team Size | % of Projects | % of Programmers |
|-----------|---------------|------------------|
| 1–3       | 25%           | 5%               |
| 4–10      | 30%           | 10%              |
| 11–25     | 20%           | 15%              |
| 26–50     | 15%           | 20%              |
| 50+       | 10%           | 50%              |

→ **Half of all programmers work on teams of 50+.**

### 27.3 Effect of Project Size on Errors

- **Small projects:** ~75% of errors from construction. Individual skill dominates quality.
- **Large projects:** Construction errors drop to ~50%; requirements and architecture errors rise proportionally.
- **Defect density increases with size:**

| Project Size (LOC) | Typical Error Density (per KLOC) |
|--------------------|----------------------------------|
| < 2K               | 0–25                             |
| 2K–16K             | 0–40                             |
| 16K–64K            | 0.5–50                           |
| 64K–512K           | 2–70                             |
| 512K+              | 4–100                            |

→ Very large projects can have **4× the errors per KLOC** of small projects.

### 27.4 Effect of Project Size on Productivity

| Project Size (LOC) | Nominal LOC/Staff-Year |
|--------------------|------------------------|
| 1K                 | 4,000                  |
| 10K                | 3,200                  |
| 100K               | 2,600                  |
| 1,000K             | 2,000                  |
| 10,000K            | 1,600                  |

→ Productivity on small projects can be **2–3× higher** than on large ones; up to **5–10×** from smallest to largest.

At small sizes (~2K LOC), the single biggest influence is **individual programmer skill**. As project size increases, **team size and organization** become greater influences.

### 27.5 Effect of Project Size on Development Activities

- **Small projects:** Construction dominates (~65% of total effort).
- **Medium projects:** Construction ~50%; architecture, integration, and system testing grow.
- **Very large projects:** Architecture, integration, and system testing consume proportionally more time.

Activities that grow at a **more-than-linear rate** as project size increases:

- Communication, Planning, Management
- Requirements development, System functional design
- Interface design and specification, Architecture
- Integration, Defect removal, System testing
- Document production

#### Programs, Products, Systems, and System Products

| Type            | Description                                | Cost Multiplier |
|-----------------|--------------------------------------------|-----------------|
| Program         | Self-use or informal sharing               | 1×              |
| Product         | Released to others; tested, documented     | 3×              |
| System          | Multiple programs working together         | 3×              |
| System Product  | Both product polish and system complexity  | 9×              |

→ **Estimation pitfall:** Using program-building experience to estimate a system product can underestimate by **~10×**.

### 27.6 Methodology and Size

- On small projects, methodologies tend to be **casual and instinctive**.
- On large projects, they must be **rigorous and explicitly planned**.
- A 1K-LOC project averages ~7% effort on paperwork; a 100K-LOC project averages ~26%.
- **Key insight:** Scaling up a lightweight methodology works better than scaling down a heavyweight one. The most effective approach is a **"right-weight" methodology**.

---

## Chapter 28 — Managing Construction

### 28.1 Encouraging Good Coding

- Don't mandate rigid technical standards top-down; programmers must **buy into them**.
- Have a **respected architect** define standards rather than a manager (operate on an "expertise hierarchy").
- **Techniques for encouraging good coding:**
  - Assign **two people** to every part (pair programming, buddy reviews).
  - **Review every line of code** (at least 3 people read each line).
  - Require **code sign-offs** by senior technical personnel.
  - **Circulate good code examples** — a "best code listings" manual.
  - Treat code listings as **public assets**, not private property.
  - **Reward good code** — but only if you're technically qualified to judge it.
- An effective standard for a manager with a programming background: *"I must be able to read and understand any code written for the project."*

### 28.2 Configuration Management (SCM)

Configuration management = the practice of **handling changes systematically** so the system maintains integrity over time.

#### Requirements and Design Changes

- Follow a **systematic change-control procedure**.
- **Handle change requests in groups** — write down all ideas, prioritize as a batch.
- **Estimate the cost** of each change (including ripple effects through design → code → test → docs).
- **High change volume is a warning sign** that requirements or architecture aren't solid.
- Establish a **change-control board** (or equivalent) — separate wheat from chaff.
- Substance matters more than form; **don't let fear of bureaucracy stop you from doing change control**.

#### Software Code Changes

**Version-control benefits:**
- No stepping on toes (lock-based or merge-based)
- One-command update to current versions
- Backtrack to any historical version
- List of changes per version
- Safety net (no personal backup worries)

Version control is **indispensable on team projects**. Consider also versioning tools, standardized machine configurations (disk images), and testing backup/restore procedures.

### 28.3 Estimating a Construction Schedule

The average large project is **1 year late and 100% over budget**. Developer estimates have a built-in **20–30% optimism factor**.

#### Estimation Approaches

- Use estimating software or algorithmic models (Cocomo II)
- Have outside experts estimate
- Walk-through meetings
- Estimate pieces → sum them up
- Use previous project data; track estimate accuracy over time

#### Good Estimation Practice

1. **Establish objectives** — what are you estimating and why?
2. **Allow time for the estimate** — rushed estimates are inaccurate.
3. **Spell out requirements** — you can't estimate an undefined product.
4. **Estimate at a low level of detail** — errors on small pieces tend to cancel out (Law of Large Numbers).
5. **Use multiple techniques** and compare results.
6. **Reestimate periodically** — early estimates can vary by 4×; accuracy improves as the project progresses.

#### Schedule Influences

| Factor | Potential Help | Potential Harm |
|--------|---------------|-----------------|
| Product complexity | -27% | +74% |
| Time constraint | 0% | +63% |
| Storage constraint | 0% | +46% |
| Requirements analyst capability | -29% | +42% |
| Programmer capability | -24% | +34% |
| Platform volatility | -13% | +30% |
| Personnel turnover | -19% | +29% |
| Use of software tools | -22% | +17% |

#### What to Do If You're Behind

- **Don't hope you'll catch up** — delays generally increase toward the end.
- **Brooks's Law:** Adding people to a late project makes it later (unless tasks are partitionable).
- **Reduce scope** — prioritize "must haves" vs. "nice to haves" vs. "optionals"; drop the least important features.

### 28.4 Measurement

> *"For any project attribute, it's possible to measure that attribute in a way that's superior to not measuring it at all."* — Tom Gilb

Key reasons to measure:
- Gives you a **handle on your development process**.
- **Enables scientific evaluation** of methods.
- **Motivational effect** — people focus on what's measured (choose carefully).

#### Useful Measurement Categories

| Size | Quality | Defect Tracking | Maintainability | Productivity |
|------|---------|-----------------|-----------------|--------------|
| Total LOC | Total defects | Severity | Public routines/class | Work-hours |
| Comment lines | Defects per class/routine | Location (class/routine) | Parameters/routine | Hours per class |
| Classes/routines | Defects per KLOC | Origin (req/design/code/test) | Decision points/routine | Dollars per LOC |
| Data declarations | MTBF | Defect correction method | Control-flow complexity | Dollars per defect |
| Blank lines | Compiler errors | Person responsible | LOC per class/routine | Changes per class |

→ Start with a **simple set** (defects, work-months, dollars, LOC), standardize across projects, and refine over time.

### 28.5 Treating Programmers as People

#### How Programmers Spend Their Time (Bell Labs, 1964)

| Activity | % |
|----------|---|
| Code-related | 35% |
| Business/Misc | 29% |
| Personal | 13% |
| Meetings | 7% |
| Training | 6% |
| Technical documents | 5% |
| Mail/Misc documents | 2% |
| Operating procedures | 2% |
| Program test | 1% |

#### Performance Variation

- **Individual variation:** 20:1 initial coding time, 25:1 debugging time, 10:1 execution speed (between best and worst).
- **Team variation:** 3.4:1 effort, 3:1 program size (identical projects).
- Top-20% performers produce ~50% of output across professions.
- **If you can pay more for a top-10% programmer, jump at the chance.**

#### Religious Issues (avoid mandating these)

Programming language, indentation style, brace placement, IDE choice, commenting style, efficiency vs. readability, methodology choice, naming conventions, gotos, global variables, productivity measures.

#### Physical Environment

| Factor | Top 25% | Bottom 25% |
|--------|---------|-------------|
| Dedicated floor space | 78 sq. ft. | 46 sq. ft. |
| Acceptably quiet | 57% yes | 29% yes |
| Acceptably private | 62% yes | 19% yes |
| Ability to silence phone | 52% yes | 10% yes |
| Frequent needless interruptions | 38% yes | 76% yes |

→ Top-25% environments yield ~**2.6× productivity** over bottom-25%. Moving from average to top-25% can improve productivity **40%+**.

### 28.6 Managing Your Manager

Tactics: Plant ideas and let manager "have the brainstorm," educate your manager continuously, focus on their interests (encapsulate your job), or — in extreme cases — refuse, or find another job. Best long-term solution: **educate your manager**.

---

## Chapter 29 — Integration

### 29.1 Importance of Integration Approach

Integration = combining separate software components into a single system. Done poorly, the system can **"collapse under its own weight during construction"** — even if the finished product would have worked.

**Benefits of careful integration:**
Easier defect diagnosis, fewer defects, less scaffolding, shorter time to first working product, shorter schedules, better customer relations, improved morale, improved chance of completion, more reliable estimates, more accurate status reporting, improved code quality.

### 29.2 Phased vs. Incremental Integration

#### Phased ("Big Bang") Integration

1. Design, code, test, debug each class individually.
2. Combine all at once.
3. Test and debug the whole system ("system dis-integration").

**Problems:** Errors can be anywhere; all surface at once; problems interact; debugging becomes panicky rather than methodical. Only viable for **tiny** programs (2–3 classes).

#### Incremental Integration

Write and test in small pieces; combine **one piece at a time**:

1. Develop a small, functional **skeleton** — thoroughly test and debug it.
2. Design, code, test a new class.
3. Integrate the new class with the skeleton. Test and debug the combination. Repeat.

**Benefits:** Errors are easy to locate (the new class is the obvious suspect); the system succeeds early; improved progress monitoring; better customer relations; classes are tested more fully; shorter development schedules through parallelism.

### 29.3 Incremental Integration Strategies

| Strategy | Description | Pros | Cons |
|----------|-------------|------|------|
| **Top-Down** | Integrate highest-level classes first; use stubs for lower levels | Control logic tested early; partially working system visible early; can begin coding before low-level design is complete | System interfaces exercised last; needs many stubs; nearly impossible to implement purely |
| **Bottom-Up** | Integrate lowest-level classes first; use test drivers for higher levels | System interfaces exercised early; errors easy to locate; integration can start early | High-level design problems found last; requires complete design before starting |
| **Sandwich** | Integrate top-level business objects and bottom-level device/utility classes first; save middle-level for last | Avoids rigidity of pure top-down or bottom-up; minimizes scaffolding; realistic and practical | — |
| **Risk-Oriented** | Integrate highest-risk (most challenging) classes first | Addresses hardest problems early; reduces project risk | — |
| **Feature-Oriented** | Integrate one identifiable feature at a time; hang features on a skeleton | Eliminates most scaffolding beyond low-level libraries; each feature adds incremental functionality; works well with OO design | Pure feature-oriented is as hard as pure top-down or bottom-up |
| **T-Shaped** | Build one deep vertical slice end-to-end first, then develop breadth | Verifies architectural assumptions early; flushes out major design problems quickly | — |
| **Vertical-Slice** | Implement top-down in sections, fleshing out one area of functionality at a time | Combines benefits of top-down with practical flexibility | — |

→ **The best approach is usually a custom combination** tailored to the specific project. These are heuristics, not algorithms.

### 29.4 Daily Build and Smoke Test

Every day: compile, link, and combine into an executable; run a **smoke test** (simple end-to-end check that the product "smokes" when run).

**Key Benefits:**
- Reduces risk of low quality
- Easier defect diagnosis (broken on Day 18 → something between Days 17 and 18 broke it)
- Improves morale (something works every day)
- Surfaces hidden work accumulation early

**Critical Practices:**
- **Build daily** — the heartbeat/sync pulse of the project.
- **Check for broken builds** — a broken build is top priority to fix.
- **Smoke test daily** — exercise the system end-to-end; evolve the smoke test as the system grows.
- **Automate** the build and smoke test.
- **Establish a build group** — on large projects, this can be a full-time role.
- **Add revisions only when code is in a consistent state** — but don't wait more than a couple of days.
- **Require developers to smoke test their own code** before adding it to the build.
- **Create a holding area** for new code.
- **Create a penalty for breaking the build** (light-hearted: lollipops, goat horns, $5 fund).
- **Release builds in the morning** rather than the afternoon.
- **Build and smoke test even under pressure** — discipline is needed most when stress is highest.

**Windows 2000** (50M LOC, tens of thousands of source files, 19-hour full build) used daily builds successfully. **The larger the project, the more important incremental integration becomes.**

#### Continuous Integration

Most published references use "continuous" to mean "at least daily." Literal continuous integration (every couple of hours) is **too much of a good thing** for most projects. Daily sync points are frequent enough for medium/large projects.

---

## Chapter 30 — Programming Tools

> *Use of a leading-edge tool set — and familiarity with the tools used — can increase productivity by 50% or more.* (Jones 2000, Boehm et al. 2000)

**20% of the tools account for 80% of the tool usage** (Pareto principle).

### 30.1 Design Tools

Graphical tools for UML, architecture block diagrams, hierarchy charts, ERDs, class diagrams. Offer automated layout, consistency checking, level-of-abstraction navigation, and sometimes direct code generation.

### 30.2 Source-Code Tools

#### Editing / IDEs

Programmers spend up to **40% of their time editing source code**. A good IDE should provide:

- Compilation and error detection from within the editor
- Integration with source control, build, test, and debugging tools
- Outline/folding views of programs
- Jump to definition and all usages
- Language-specific formatting and smart indenting
- Brace matching and template completion
- Automated refactorings
- Programmable macros
- Regular-expression search-and-replace across multiple files
- Side-by-side diff comparisons
- Multilevel undo

#### Other Source-Code Tools

| Tool Category | Purpose |
|---------------|---------|
| **Multiple-file search/replace** | grep, Perl, AWK, sed — find/change strings across the codebase |
| **Diff tools** | Compare file versions; find what changed |
| **Merge tools** | Handle simultaneous edits by multiple developers |
| **Source-code beautifiers** | Standardize formatting, highlight syntax; can transform legacy code |
| **Interface documentation** | Extract @tag-annotated documentation (e.g., Javadoc) |
| **Templates** | Streamline repetitive keyboarding; encourage consistency across the team |
| **Cross-reference tools** | List all uses of variables/routines |
| **Class-hierarchy generators** | Analyze inheritance trees; support modularization |
| **Picky syntax/semantics checkers** | Find subtle errors compilers miss (e.g., Lint: uninitialized variables, `=` vs. `==`) |
| **Metrics reporters** | Complexity analysis, LOC counts, defect tracking — ~20% positive impact on maintenance |
| **Refactorers** | Automated renaming, routine extraction, parameter reordering |
| **Restructurers** | Convert spaghetti/goto code to structured form — 25–30% impact on maintenance |
| **Code translators** | Convert from one language to another (hazard: bad code → bad code in new language) |
| **Data dictionaries** | Database of all significant data/class names; prevents naming clashes |

### 30.3 Executable-Code Tools

*(Covered in source text: debugging, disassembling, profiling, code-coverage, memory analysis, etc.)*

### 30.4 Tool-Oriented Environments

Integrated environments where tools work together seamlessly — Unix with its small-tools philosophy, or integrated CASE environments.

### 30.5 Building Your Own Programming Tools

When off-the-shelf tools fall short, custom scripts and utilities (Perl, Python, shell scripts, macros) can fill gaps. The **"build vs. buy"** decision applies to programming tools too.

### 30.6 Tool Fantasyland

McConnell describes an idealized future tool suite (circa 2004) including:
- Integrated version control with semantic understanding
- Intelligent code completion with context awareness
- Automated refactoring across entire codebases
- Real-time collaboration features
- Advanced visualization of code structure and runtime behavior

*(Many of these have since become reality in modern IDEs.)*

---

## Key Points Summary

| Chapter | Core Message |
|---------|-------------|
| **27: Program Size** | As projects grow, communication, error density, and non-construction activities explode nonlinearly. Use "right-weight" methodologies matched to project size. |
| **28: Managing Construction** | Encourage good coding through review and examples, not rigid mandates. SCM, estimation, and measurement are essential scaffolding. Treat programmers as knowledge workers deserving quality environments. |
| **29: Integration** | Incremental integration (any flavor) beats phased/"big bang" integration. Daily builds + smoke tests are the heartbeat of a healthy project. |
| **30: Programming Tools** | Invest in tools — they can boost productivity 50%+. The 80/20 rule applies: master the 20% of tools you'll use 80% of the time. |
