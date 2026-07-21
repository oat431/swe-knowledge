---
tags:
  - scm
  - branching
  - mainline
  - workspace
  - repository
  - software-configuration-management
---

# Codeline & Branching

> **Source:** Berczuk & Appleton, *Software Configuration Management Patterns: Effective Teamwork, Practical Integration* (Addison-Wesley, 2002), Chapters 4–7
> **Patterns Covered:** Mainline, Active Development Line, Private Workspace, Repository

## Overview

When developing software as a team, you must reconcile parallel development efforts. Version control tools provide branching and merging facilities, but branching — while powerful — has a cost. Every branch that diverges must eventually be merged, and deferred integration grows exponentially harder with time. These four patterns form a coherent strategy for managing codelines: keep a single primary line of development (**Mainline**), keep it usable through lightweight quality gates (**Active Development Line**), give each developer a controlled sandbox (**Private Workspace**), and make it trivially easy to get the right versions of everything (**Repository**).

---

## 1. Mainline — Simplify Your Branching Model

### Problem

How do you keep the number of currently active codelines manageable, avoid a version tree that grows too wide and dense, and minimize merging overhead?

Branching isolates you from change, which is useful — but every branch that diverges must eventually merge. Merging is hard even with good tools, because two changes can conflict in intent (not just text), and resolving those conflicts requires knowledge of both authors' intentions. The effort saved by branching can be more than consumed by a messy merge.

A common anti-pattern is the **staircase (cascade) branching model**, where each release branches off the previous release. This makes it hard to trace where code originated and complicates urgent fixes that must span multiple releases.

### Solution

**Do the majority of development on a single *mainline* — a "home codeline" that all work eventually lands on.**

- The mainline is the central codeline that serves as the basis for sub-branches and their resultant merges.
- All ongoing development activities end up on this single codeline at some point.
- Branch only with a clear, justified reason where the independence gained clearly outweighs the cost of a later merge.

**When to branch:**

| Situation | Rationale |
|---|---|
| **Customer releases** | Bug-fix development on released code without exposing customers to new-feature work in progress. Migrate fixes between release branches and mainline as needed. |
| **Long-lived parallel efforts** | When work will make the codeline unusually unstable. Use a **Task Branch**; integrate back as soon as feasible. |
| **Integration stabilization** | Create an integration/release-prep branch near the end of a cycle so mainline development can continue while the release stabilizes. |

**Shallow branching structure (the recommended model):**

```
                Release 1.0 ──→ 1.0 Fixes
               /
Mainline ──→──┼──→ Release 2.0 ──→ 2.0 Fixes
               \
                → ... ongoing development ...
```

Instead of branching a new release-line off the previous release-line, merge the previous release-line back to the mainline and branch the new release-line from there.

### Key Practices

- Create a codeline (e.g., `/main`) using the current active code base as a starting point.
- Check in all changes to this codeline.
- Follow per-check-in test procedures to keep the mainline usable.
- Think hard before branching: ask whether the work *really* requires a branch. Avoid long-lived branches that must be merged later.

> **Quote:** "90% of SCM 'process' is enforcing codeline promotion to compensate for the lack of a mainline." — Wingerd & Seiwald (1998)

### Related Patterns

- → **Active Development Line** — keeps the mainline usable when many people work on it.
- → **Release Line** — a branch created from mainline for a stabilized release.
- → **Task Branch** — for long-lived, high-risk parallel work that can't live on the mainline.

---

## 2. Active Development Line — Define Your Goals

### Problem

How do you keep a rapidly evolving codeline stable *enough* to be useful?

When many people change the code concurrently, any change can break the system. You need the codeline to be stable so it doesn't interfere with people's work. But exhaustive pre-check-in testing takes time and, counter-intuitively, can provide a false sense of security — two changes tested individually can still break the system when submitted close together. The longer the tests, the more likely an incompatible change sneaks in.

The extremes are both dysfunctional:

- **Too strict:** Semaphores and exhaustive tests → a stable but dead codeline (Figure: `Good Test → Good Test → Good Test` — no progress).
- **Too loose:** No gates → a very active but useless codeline (Figure: `Good → Poor → Poor → Good` — constant breakage).

### Solution

**Institute policies that make the mainline stable *enough* for the work it needs to do. Don't aim for perfection — aim for *usable and active*.**

An active development line will have frequent changes, well-tested checkpoints that are guaranteed "good," and most other points good enough for development. (Figure: `Good → Poor → Good → Good` — alive and usable.)

### Analysis: How "Good" Does It Need to Be?

Answer these questions to calibrate:

1. **Who uses the codeline?** Other active dev teams → emphasize speed. External clients → more validation.
2. **What is the release cycle?** Early in development → expect instability. Nearing release → test more.
3. **What test mechanisms do we have?** Good regression tests → errors don't persist long, so emphasize speed on check-in. Poor test infrastructure → be more careful.
4. **How much is the system evolving?** Heavy feature work → expect more instability.
5. **What are the real costs of breakage?** Match investment to actual pain.

### Key Practices

- Set a pre-check-in quality bar: "strict enough to keep showstopper defects out, but lenient enough to disregard trivial defects" (McConnell).
- If the pre-check-in process takes too long, developers will do larger-grained, less frequent check-ins — which increases merge conflict probability and makes it harder to back out problematic changes.
- Each developer uses a **Private Workspace** with a **Private System Build**, **Unit Test**, and **Smoke Test** before submitting.
- Run more exhaustive tests as a batch process to create **Named Stable Bases**.
- Use an integration workspace where snapshots are built periodically and subjected to more exhaustive tests.
- Understand your project's **rhythm** — the recurring, predictable exchange of work products that keeps concurrent work synchronized.

> **Quote:** "Change is expensive, no question about it. But consider the alternative — stagnation." — Jim Highsmith

### Related Patterns

- → **Codeline Policy** — establishes which lines follow this model and the check-in process for each.
- → **Private Workspace** — gives each developer isolation to keep the active line alive.
- → **Release-Prep Code Line** — when stability needs increase near release time.
- → **Task Branch** — for work needing more stability than an active development line provides.
- → **Named Stable Bases** — labeled snapshots for clients needing guaranteed stability.

---

## 3. Private Workspace — Isolate Your Work to Control Change

### Problem

How do you stay current with a continuously changing codeline while also making progress without your environment changing out from under you?

Developers need a place to work on code isolated from outside changes until a task is complete. Continuous, uncontrolled updates break *flow* — the deep, nearly meditative state of involvement that produces the best work. But working in total isolation until the last moment causes a painful big-bang integration.

**Three approaches to managing parallel change:**

| Approach | Description | Risk |
|---|---|---|
| **Continuous integration into workspace** | Integrate every change as it happens | Spend all your time integrating; lose flow |
| **Delayed (big-bang) integration** | Integrate at the last possible moment | Massive merge; hard to isolate where flaws came from |
| **Controlled workspace updates** | Update on your own schedule, at task boundaries | Requires discipline; the pattern's solution |

### Solution

**Do your work in a Private Workspace where you control when and how your environment changes.**

A workspace is "a copy of all the 'right' versions of all the 'right' files in the 'right' directories" (White, 2000) — a place "where an item evolves through many temporary and inconsistent states until it is checked into the library" (Whitgift, 1991).

### What a Private Workspace Contains

**Should contain:**
- Source code you are editing
- Any locally built components
- Third-party derived objects (libraries, DLLs, JARs) you cannot or choose not to build
- Built objects for all system code
- Configuration and data needed to run and test the system
- Build scripts to build the system in your workspace
- Information identifying the versions of all components

**Should NOT contain:**
- Private versions of system-wide policy-enforcing scripts (use shared binaries)
- Components copied from somewhere else rather than version control
- Tools (compilers, etc.) that must be consistent across all versions — build scripts should select the correct tool version

### Development Workflow

1. **Get up to date** — Update the source tree from the codeline, or populate from the latest system build. If working on a different branch/baseline, create a new workspace from it.
2. **Make your changes** — Edit the components you need to change.
3. **Private System Build** — Build to update derived objects.
4. **Unit Test** — Test your change.
5. **Update workspace** — Get the latest versions of all components you haven't changed.
6. **Rebuild + Smoke Test** — Verify you haven't broken anything.
7. **Submit** — Check in your changes.

### Key Practices

- **Multiple workspaces for multiple tasks:** Have separate workspaces for different branches/releases (e.g., a "release 1.1" workspace for bug fixes and a "release 2" workspace for new development). Don't try to save space by factoring out common components — separate is simpler and safer.
- **Update before starting a new task**, not only before check-in. This catches conflicts incrementally.
- **Fine-grained tasks, frequent check-ins** — the easiest way to avoid getting out of date.
- **Periodically create a fresh workspace** to avoid the "works for me" syndrome caused by stray files.
- **Ensure scripts/tools use correct execution paths** so they run with the workspace version, not an installed system version.
- **Keep an integration/build workspace** on a build machine for centralized validation.

### Related Patterns

- → **Private System Build** — verify changes don't break the build before check-in.
- → **Repository** — populates the workspace with the right versions of everything.
- → **Integration Build** — centralized build that catches what private builds miss.
- → **Task Branch** — for work touching large parts of the codebase that takes a long time.
- → **Smoke Test** — quick validation that major functionality isn't broken.
- → **Task Level Commit** — when you merge changes back to the codeline.

---

## 4. Repository — One-Stop Shopping

### Problem

How do you get the right versions of the right components into a new workspace — easily and reliably?

A workspace consists of more than just source code: third-party libraries, configuration files, data files, build scripts, install scripts, and built objects. These artifacts often live in multiple locations — version control, shared servers, install kits, e-mail attachments. Multiple source points create overhead, errors, and reluctance to stay current or create new workspaces. When it's too hard to set up a workspace, developers work with stale code and bugs go undetected until QA finds them.

### Solution

**Have a single point of access — a Repository — for all code and related artifacts. Make creating a developer workspace simple, repeatable, and transparent.**

### Three Requirements

1. **Easy to use and repeatable** — In the spirit of "ubiquitous automation" (Hunt & Thomas). One command should populate a workspace.
2. **Complete** — Gives you *all* components needed for any identifiable revision: source, build scripts, configuration, third-party libraries, and built objects.
3. **Works for all versions** — You can recreate the workspace for any point in the project's history, not just the tip.

### Implementation Approaches

**Approach A: Everything in version control.** Place all source files, configuration files, build scripts, and third-party components in the VCS. Use labels or branches to identify the set relevant to a particular version. A simple `get` command specifying the version gives you a complete workspace. The VCS mirrors the build environment; history of build-environment changes is tracked automatically.

```
Repository
├── Project1/
│   ├── src/     (versioned)
│   └── lib/     (third-party, versioned)
├── Project2/
│   ├── src/
│   └── lib/
└── ThirdParty/
    ├── Comp1/
    │   ├── Version 1
    │   └── Version 2
    └── ...
```

**Approach B: Script-driven assembly.** When the VCS tree doesn't map directly to the build tree, or binary files are better stored elsewhere, use a script (make, ANT) that copies the appropriate versions of the appropriate files to the appropriate places. This may be the easiest way to populate a workspace with the result of a nightly build.

### Key Practices

- All configuration files, database libraries, and their settings get the same label — they change together.
- Keep the mapping from version-tree to build-tree in version control itself, so it evolves with the product.
- When a new third-party component version is needed, developers get it automatically via their normal update process — no missed e-mails, no forgotten manual steps.

### Related Patterns

- → **Third Party Codeline** — organize vendor code within the repository using its own codeline with branches for customization.
- → **Private Workspace** — the workspace that the repository populates.
- → **Named Stable Bases** — labels on the repository that identify known-good configurations.

---

## Summary: How These Patterns Fit Together

```
┌──────────────────────────────────────────────────────────┐
│                     REPOSITORY                           │
│  (single source for all artifacts: code, libs, configs)  │
├──────────────────────────────────────────────────────────┤
│                                                          │
│   populates          populates          populates        │
│       ↓                  ↓                  ↓            │
│  ┌─────────┐       ┌─────────┐       ┌─────────┐        │
│  │Private  │       │Private  │       │ Build   │        │
│  │Workspace│       │Workspace│       │Workspace│        │
│  │(Dev A)  │       │(Dev B)  │       │ (CI)    │        │
│  └────┬────┘       └────┬────┘       └────┬────┘        │
│       │                 │                 │              │
│       └────────┬────────┘                 │              │
│                ↓                          ↓              │
│         ┌──────────────────────────────────┐            │
│         │         MAINLINE                 │            │
│         │   (Active Development Line)      │            │
│         │   — single home codeline         │            │
│         │   — usable, not perfect          │            │
│         │   — shallow branching            │            │
│         └───────────┬──────────────────────┘            │
│                     │                                    │
│            ┌────────┴────────┐                          │
│            ↓                  ↓                          │
│     Release 1.0        Release 2.0                      │
│     (stabilized)       (in development)                  │
└──────────────────────────────────────────────────────────┘
```

1. The **Repository** provides one-stop access to everything needed to build and test.
2. Each developer works in a **Private Workspace**, controlling when outside changes enter.
3. Changes flow into the **Mainline**, which is kept usable as an **Active Development Line** through lightweight quality gates.
4. Release branches are created from the mainline (not from prior releases), keeping the branching structure shallow and merges tractable.

The core philosophy: **branch sparingly, integrate continuously, isolate locally, and make everything accessible from one place.**

---

## Related Notes

- [[Software Configuration Management Overview]]
- [[Version Control/01 Git Fundamentals]]
- [[Version Control/02 Git Workflows]]
- [[Version Control/03 Git Advanced]]
