---
tags:
  - scm
  - commit
  - testing
  - policy
  - smoke-test
  - software-configuration-management
---

# SCM Commit & Testing Patterns

> **Source:** Berczuk & Appleton, *Software Configuration Management Patterns: Effective Teamwork, Practical Integration* (2002), Chapters 11–16.
> **Purpose:** Patterns governing how developers commit changes, what policies govern codelines, and how testing validates those changes before and after integration.

---

## Pattern Map

```
Task Level Commit ───► granularity of check-ins
       │
       ▼
Codeline Policy ─────► rules of the road per codeline
       │
       ▼
Smoke Test ──────────► quick pre/post-checkin validation
       │
       ├──► Unit Test ────────► fine-grained module contracts
       │
       └──► Regression Test ──► exhaustive system-level safety net
       │
       ▼
Private Versions ────► checkpoint without publishing
```

Every pattern in this chapter answers one core question: **how do you maintain a stable codeline while letting developers move fast?** The answer is a layered testing and policy strategy that matches the risk of each change to the appropriate level of validation.

---

## Task Level Commit (Chapter 11)

### Problem

How much work should you do between commits? Too large, and rollback becomes impossible and integration breaks are hard to diagnose. Too small, and the overhead of pre-checkin validation kills productivity.

You need to add a feature or fix a defect — which often means editing many files across the codebase. Every change introduces potential instability, so you want changes to be consistent and atomic. But pre-checkin policies that require long test suites discourage frequent commits.

> **Anti-pattern:** In one organization, rigorous pre-checkin validation caused developers to commit at most once a day — sometimes less. Each commit covered multiple unrelated tasks. When the build broke, nobody could isolate which change caused it, and rollback was coarse and painful.

### Solution

**Do one commit per small-grained, consistent task.**

- The unit of work is a **new feature** (or part of one), a **problem report**, or a **refactoring task**
- Each commit represents a **consistent state** of the system — buildable, if not fully tested
- When in doubt, err on the side of **more commits**: it's easier to roll back, easier to see integration effects, and the commit history becomes a meaningful "pulse" of development

Examples of reasonable commit tasks:
- A single problem report (though a broad problem may span two or more commits)
- Changing all calls to a deprecated method across the entire system
- Changing calls to a deprecated method for one coherent subsystem
- A consistent set of changes accomplished in a day

### Key Heuristics

| Factor | Lean toward more commits | Lean toward fewer commits |
|--------|--------------------------|---------------------------|
| Scope | Changes span multiple subsystems | Changes are isolated to one file/module |
| Risk | High-impact or cross-cutting change | Low-risk, well-understood change |
| Team size | Large team with parallel work | Solo or small team |
| Pre-checkin cost | Fast smoke test | Long, exhaustive test suite |

### When This Fails

- **Extended code freezes** make small-grained commits impossible — developers hoard changes, then dump them when the freeze lifts
- **Overly rigorous pre-checkin policies** inadvertently discourage frequent commits — streamline to test only what's necessary
- **Far-reaching, long-lived changes** can't be committed incrementally — use a [[05_Release_and_Version_Management#Task Branch|Task Branch]] instead

> **Rule of thumb:** Commit at least once a day if it makes sense. The revision control system is the "pulse" of development work.

### Related Patterns
- [[#Smoke Test]] and [[#Unit Test]] — keep pre-checkin validation fast enough to encourage this pattern
- [[#Codeline Policy]] — defines what validation is required before commit on each codeline
- [[05_Release_and_Version_Management#Task Branch|Task Branch]] — for changes too disruptive for incremental mainline commits

---

## Codeline Policy (Chapter 12)

### Problem

When you have multiple codelines (mainline, release lines, development lines), how do developers know which one to commit to, when to commit, and what tests to run before checking in?

Each codeline serves a different purpose — bug-fixing a shipped release, porting to a new platform, day-to-day development — and each demands a different stability level. Naming conventions alone can't capture the finer points: is this release line strictly restricted, or only slightly slower? Documentation that gets out of sync is worse than no documentation at all.

> **Anti-pattern (Impedance Mismatch):** A company developed products on shared components. One component was on an evolving codeline with relaxed policies, but other teams treated it as stable. They were constantly disrupted by interface changes, even with advance warning. The codeline policy didn't mesh with all users' needs.

### Solution

**For each branch or codeline, formulate a concise, auditable policy — the "rules of the road."**

A codeline policy should be **1–3 paragraphs** (one page absolute maximum) and should specify:

| Policy Element | Description |
|----------------|-------------|
| **Purpose** | What kind of work: development, maintenance, specific release, subsystem |
| **Checkin rules** | How and when to check in/out, branch, and merge |
| **Access control** | Restrictions by individual, role, or group |
| **Import/export** | Which codelines feed into this one, and which consume from it |
| **Lifetime** | Duration of work or conditions for retiring the codeline |
| **Activity load** | Expected frequency of integration |

### Example Policies (from Wingerd & Seiwald 1998)

**Development codeline:**
> Interim code changes may be checked in; affected components must be buildable.

**Release codeline:**
> Software must build and pass regression tests before checkin. Checkins limited to bug fixes; no new features. After checkin, branch is frozen until the entire QA cycle completes.

**Mainline:**
> All components must compile and link, and pass regression tests. Completed, tested new features may be checked in.

### Storage and Enforcement

- Store the policy in the **branch description** of your version control tool if supported — it's one command away
- Otherwise, store in a **well-known, readily accessible place** with a simple command/macro to display it
- Enforce via **triggers** (pre-commit hooks), but if automation becomes too constraining, use it to **report on adherence** instead of blocking
- **Create a branch whenever you have an incompatible policy** — policy drives branching structure

### Key Principle

> Developers follow policies they understand and believe in, not ones that seem arbitrary (Karten 1994). Keep policies short, practical, and visibly enforced through automation where possible — not through blame or punishment.

### Related Patterns
- [[#Task Level Commit]] — the policy defines what validation gates a commit must pass
- [[#Smoke Test]], [[#Unit Test]], [[#Regression Test]] — the testing layers referenced in policy rules
- [[05_Release_and_Version_Management#Release Line|Release Line]] and [[02_Codeline_and_Branching#Active Development Line|Active Development Line]] — the codelines that need distinct policies

---

## Smoke Test (Chapter 13)

### Problem

The code builds — but does the system still *work* after your change? You can't run exhaustive tests before every commit without killing velocity. How do you catch "show stopper" defects without slowing development to a crawl?

Integration problems are the most common source of failures, but integration-level tests are slow to set up and run. If the pre-checkin test is too long, developers commit less often and each commit carries more risk. If testing is too shallow, broken builds waste everyone's time.

> **Anti-pattern (exhaustive pre-checkin):** At one company, the pre-checkin test suite took 60 minutes. Developers feared it and committed as infrequently as possible, batching many unrelated changes. Someone else would inevitably check in a conflicting change during that hour. The long test reduced both productivity and quality.
>
> **Anti-pattern (no smoke test):** At another company, release candidates were built periodically. The first developer to try a new build got the "pleasure" of finding (and sometimes fixing) all the bugs — leading to a culture of blame and reluctance to adopt new builds.

### Solution

**Subject each build to a smoke test — a quick, automated verification that the application hasn't broken in an obvious way.**

A smoke test should be:

| Property | Meaning |
|----------|---------|
| **Quick** | Fast enough to run before every commit and after every build — "quick" depends on your context, but think seconds to minutes, not hours |
| **Self-scoring** | Returns pass/fail automatically — no manual inspection required to know if something broke |
| **Broad coverage** | Exercises the critical paths across the system, not just one module |
| **Runnable by developers** | Part of the developer's own pre-commit workflow, not just QA's |

The smoke test is **not** a replacement for deeper testing. It catches "show stopper" defects while disregarding trivial ones (McConnell 1996). A suite of unit tests can form the basis for a smoke test if nothing else is available.

### When to Run Smoke Tests

- **Before every commit** — developer runs manually as part of pre-checkin validation
- **After every integration build** — automated as part of the build pipeline
- **Before release candidate builds** — combined with more thorough regression tests
- **Daily** — as part of the [[02_Codeline_and_Branching#Daily Build and Smoke Test|Daily Build and Smoke Test]] to establish [[02_Codeline_and_Branching#Named Stable Bases|Named Stable Bases]]

### Maintaining Smoke Tests

When you add new basic functionality, **extend the smoke test to cover it.** But don't put exhaustive edge-case tests here — those belong in [[#Unit Test]] or [[#Regression Test]].

### Pitfalls

- **Buggy testing infrastructure:** The test itself must be reliable. If developers can't trust the test results, they'll ignore them
- **Unrealistic test data:** Canned inputs are fine only if they're realistic enough to surface real problems
- **Test scope creep:** Resist the temptation to make the smoke test exhaustive — it defeats the purpose

### Related Patterns
- [[#Unit Test]] — verify individual module contracts; smoke tests can be composed of unit test suites
- [[#Regression Test]] — the exhaustive complement to smoke testing, run less frequently
- [[03_Workspace_and_Build#Private System Build|Private System Build]] — gives meaningful smoke test results by building consistently

---

## Unit Test (Chapter 14)

### Problem

A smoke test tells you *that* something broke — but not *what* broke, or whether a specific module still honors its contract after your change. How do you test at the fine-grained level of classes and functions?

Integration is where most problems become visible, but when an integration test fails you're left asking "what broke?" Testing at the component interface level is easier to reason about than testing at the system level, but writing tests for every method all the time can become tedious.

> **Experience report:** The author worked at places where testing was ad-hoc — system tests only, no unit tests. When system tests failed, they'd run the debugger and sometimes found a problem, sometimes found a contract violation by a client. It took more effort than necessary. After adopting unit testing (inspired by Kent Beck and XP), problems were isolated quickly — often to code that *didn't* have unit tests. Unit tests made code changes "less scary."

### Solution

**Develop and run unit tests — fine-grained, automated tests that verify individual components obey their contract.**

A good unit test has these properties (Beck 2000):

| Property | Description |
|----------|-------------|
| **Automatic & self-evaluating** | Returns a boolean pass/fail — no human inspection needed unless there's a failure |
| **Fine-grained** | Tests every significant interface method with known inputs. Don't test trivial accessors/setters — test things that *might break* |
| **Isolated** | Does not interact with other tests — one test failing shouldn't cascade |
| **Tests the contract** | Self-contained so external changes don't affect results. Update the test when the interface contract changes |
| **Simple to run** | A single command-line or IDE invocation, no setup required |

### When to Run Unit Tests

| When | Why |
|------|-----|
| **While coding** | Immediate feedback on whether your change broke anything |
| **Just before committing** | After catching up to the current codeline version — verify your changes still work with others' |
| **When debugging a smoke/regression failure** | Isolate which low-level component or interface broke |
| **During refactoring** | Indispensable — verify behavior hasn't changed when structure has (Fowler 1999) |

### Tooling

Use a testing framework: **JUnit** for Java, **cppUnit** for C++, **PyUnit/unittest/pytest** for Python, and equivalents for other languages. Frameworks handle the infrastructure (discovery, running, reporting) so you can focus on the tests themselves.

### Design Feedback

> "If I can't come up with a good unit test for a class, I should make sure my design is not overly complicated and not abstract enough." — Berczuk

Unit testing serves double duty: it validates behavior **and** exposes over-complicated designs. If a class is hard to test, it's probably hard to use.

### Unresolved Issues

- **Narrow public interfaces:** When you want to test internal functions but the public API is narrow, you face a trade-off: widen the interface for testability, or accept untested internals. Approaches include package-private visibility, test-specific subclasses, or dependency injection.
- **Test maintenance:** As the codebase evolves, unit tests must evolve too. Tests that aren't maintained become false confidence.

### Related Patterns
- [[#Smoke Test]] — unit test suites can form the basis for smoke tests
- [[#Regression Test]] — unit tests help isolate *what* broke when a regression test fails
- [[#Task Level Commit]] — fast unit tests enable small-grained commits

---

## Regression Test (Chapter 15)

### Problem

Fixing a defect has a substantial chance of introducing another (Brooks 1995). How do you ensure existing code doesn't get worse as you make improvements?

Software systems are complex. Every change risks breaking something seemingly unrelated. Exhaustive testing takes time, but skipping it wastes developer — and potentially customer — time. Even if you commit to periodic exhaustive testing, you still face the questions: *which* tests to write, and *when* to run them?

> **Anti-pattern:** At a small software company, the codebase mixed newer clean code with evolved legacy code. On any given day, you couldn't be sure whether pulling the latest version would give you a working system or waste your day getting it to compile. There was no automated testing of core APIs. People avoided moving to the current codebase in fear — but this caused even more problems as they fell further behind. Easily preventable quality issues recurred because there was no way to check for known failure modes.

### Solution

**Run regression tests whenever you need to ensure codeline stability — before a release, before a risky change, and as part of the nightly build.**

Regression tests are:
- **End-to-end black box tests** that cover actual past or anticipated failure modes
- **Large-grained** — they test for unexpected consequences of integrating components
- **Accumulated over time** — every time you find a bug, write a test that reproduces it and add it to the suite

### Building a Regression Suite

Derive test cases from:

| Source | Rationale |
|--------|-----------|
| **Pre-release QA** | Problems found during formal QA cycles |
| **Customer/user reports** | Real-world failures that actually affected users |
| **System requirements** | Tests based on what the system is supposed to do |

> **Cardinal rule:** Always add test cases to the regression suite. If a problem happened once, it is likely to happen again. Remove test cases only for very well-thought-through reasons.

### When to Run Regression Tests

| Frequency | When |
|-----------|------|
| **Nightly** | As part of the automated nightly build — identifies *when* something regressed |
| **Before a release** | Establish that the codebase is no worse than the last release |
| **Before a risky/sweeping change** | Baseline the system before a major refactoring |
| **On demand** | When investigating a suspected regression |

Regression tests take too long to run before every commit. That's by design: they're the exhaustive complement to the quick [[#Smoke Test]]. Together they form a layered safety net.

### Debugging Workflow

When a regression test fails:
1. The regression test identifies a **system-level failure** but doesn't necessarily say what broke
2. Run [[#Unit Test|unit tests]] to isolate the low-level component or interface
3. Investigate whether unit test inputs still match the system's actual behavior
4. Fix the root cause, then add any new test cases discovered during debugging

### Related Patterns
- [[#Smoke Test]] — the quick complement; together they form a fast + exhaustive testing pair
- [[#Unit Test]] — used to isolate failures found by regression tests
- [[#Codeline Policy]] — defines when regression tests are required (e.g., "must pass regression tests before release-line checkin")

---

## Private Versions (Chapter 16)

### Problem

Some changes are complex explorations — you want to checkpoint intermediate steps, try a design path, back out if it fails, and try another. But committing these experiments to the mainline subjects everyone to unstable, potentially dead-end code. How do you use version control to checkpoint your work without publishing changes to the team?

> **The dilemma:** You want to use the tools of your trade — version control — to create a stable, useful codeline. But you want to do it *privately*. Without version control, you can't checkpoint, revert, or explore branches of a design decision. But publishing every experiment clutters the version history with noise and can break the build for others.

Copying files manually creates a minimalist (and unreliable) version control system. Skipping version control entirely means you can't back out of a dead-end change. What you need is the power of version control without the visibility.

### Solution

**Provide developers with a mechanism for checkpointing changes at whatever granularity they need, without publishing to the shared repository. Only stable, complete code sets go into the project repository.**

### Implementation Strategies

| Approach | When to Use |
|----------|-------------|
| **Dedicated Private Workspace** | Global changes (e.g., interface overhauls) where you need to evaluate consequences before sharing |
| **Local repository** | Changes scoped to a small part of the source tree (one package, one directory) — map that subtree to a local CVS/Git repo or a developer-specific branch |
| **Promotion levels / stages** | Tools that support "private" stages — check in locally, promote to shared only when ready |
| **Developer-specific branch** | A branch in the shared repo that others agree not to integrate from until you declare it ready |

### The Critical Rule

> **Before merging private work into the active codeline, follow *all* the procedures in your standard [[#Codeline Policy|codeline policy]].** Private versioning defers validation, it doesn't eliminate it. The final checkin to the shared repository must pass the same gates as any other commit.

### What Private Versions Enable

- **Decision trees:** Explore path A, checkpoint, explore path B, checkpoint, compare, pick the winner — all without noise in the shared history
- **Major refactorings:** Break a week-long refactoring into small steps, checkpointing at each stable intermediate state
- **Proof of concepts:** Spike a solution, verify it works, then either discard the branch or polish it for mainline
- **Integration testing:** Check changes into a private version, catch up with the mainline, test integration, and roll back if there's a problem — all before anyone else sees it

### Pitfalls

- **Private work diverges too long:** Developers must migrate changes to the shared repository at reasonable intervals. The longer private work lives in isolation, the harder the eventual merge
- **Tool fragmentation:** Using a different tool for private versioning than for shared versioning creates a learning burden — prefer mechanisms that use the same version control concepts

### Related Patterns
- [[03_Workspace_and_Build#Private Workspace|Private Workspace]] — dedicating an entire workspace to experimental work
- [[05_Release_and_Version_Management#Task Branch|Task Branch]] — for long-lived team-level isolation (vs. individual private versioning)
- [[#Task Level Commit]] — private versions enable the small-grained checkpointing that Task Level Commit encourages, without the visibility cost

---

## Pattern Relationships Summary

```
                    ┌──────────────────────┐
                    │   Codeline Policy    │  "What are the rules?"
                    └──────────┬───────────┘
                               │ defines gates for
          ┌────────────────────┼────────────────────┐
          ▼                    ▼                    ▼
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────────┐
│  Task Level     │  │  Smoke Test     │  │  Regression Test    │
│  Commit         │  │  (fast gate)    │  │  (exhaustive gate)  │
│  (granularity)  │  └────────┬────────┘  └──────────┬──────────┘
└────────┬────────┘           │                       │
         │                    ▼                       ▼
         │           ┌─────────────────┐    ┌─────────────────┐
         └──────────►│  Unit Test      │◄───│  (isolates      │
                     │  (component)    │    │   failures)     │
                     └─────────────────┘    └─────────────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │  Private        │
                     │  Versions       │  "Experiment safely"
                     └─────────────────┘
```

**Core tension:** Speed vs. stability. Every pattern resolves this tension at a different layer:
- **Codeline Policy** sets the stability bar per codeline
- **Task Level Commit** keeps changes atomic and rollback-friendly
- **Smoke Test** provides the fast "does it basically work?" check
- **Unit Test** verifies fine-grained contracts
- **Regression Test** catches unexpected system-level regressions
- **Private Versions** lets developers checkpoint without destabilizing shared codelines

Together, they form a **testing pyramid for SCM**: fast, frequent tests at the bottom (unit, smoke); slower, exhaustive tests at the top (regression); policy and commit discipline tying it all together.
