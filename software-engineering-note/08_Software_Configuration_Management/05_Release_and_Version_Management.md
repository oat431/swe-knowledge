---
tags: [scm, release, version, branch, software-configuration-management]
source: "Berczuk & Appleton, Software Configuration Management Patterns (2002)"
created: 2026-07-21
---

# 05 — Release and Version Management

> **Source:** Steve Berczuk with Brad Appleton, *Software Configuration Management Patterns: Effective Teamwork, Practical Integration*, Addison-Wesley, 2002. Chapters 17–20.

---

## Overview

Once a product ships, two things must happen in parallel: the released version needs bug fixes and the team needs to build the next release. Doing both on a single codeline creates tension — bug-fix changes may conflict with in-progress refactoring, and feature work destabilises what should be shippable code. The five patterns in this chapter address that tension with branching structures, stabilisation policies, and integration rhythms.

| Pattern | Core Problem | Solution |
|---|---|---|
| **Release Line** (Ch. 17) | Maintaining shipped versions alongside active development | Branch each release off the mainline; propagate fixes both ways |
| **Release-Prep Code Line** (Ch. 18) | Stabilising for release without freezing all work | Branch *before* release — branch instead of freeze |
| **Task Branch** (Ch. 19) | Long-lived, disruptive tasks that can't integrate incrementally | Fork a private branch; merge back as a single transaction |
| **Named Stable Bases** (Ch. 20) | Keeping interfaces from churning unpredictably | Stabilise system interfaces no more than once a week |
| **Daily Build and Smoke Test** (Ch. 20) | Catching integration errors before they accumulate | Build and smoke-test the whole system at least daily |

---

## 1. Release Line (Chapter 17)

### Problem

> How do you do maintenance on released versions without interfering with current development work?

After shipping, the released code may need to evolve independently of the mainline. Customers cannot always upgrade immediately — data migration, deployment complexity, or simple risk-aversion means they stay on the shipped version. Meanwhile the mainline marches toward the next major release. If all work stays on one codeline, you face two pathologies:

- **Label-only shipping:** Tag the release, ship that snapshot, keep working on the same line. Bug fixes to the shipped version are impossible in isolation — every fix ships with whatever unstable work has landed since the tag.
- **Staircase of dependent branches:** Branch each release off the *previous* release branch (release-1.0 → release-1.1 → release-2.0). This makes it nearly impossible to determine what code is common across releases.

### Solution

**Split maintenance and active development into separate codelines.** Branch each release off the *mainline* at ship time. All bug fixes and minor enhancements to the shipped version happen on the release branch; all work for the next major release happens on the mainline.

```
        rel/1.0 ──●──●──●  (bug fixes only)
       ╱
main ─●────────●────────●──●──── (ongoing development)
     1.0        \      2.0
                 rel/2.0 ──●──●
```

Key rules:

- At release time, branch **all** code, including third-party dependencies.
- Bug fixes are made on the release branch, then **propagated (merged) into the mainline** regularly.
- If a fix is also needed on an older release branch, cherry-pick or merge it there too.
- A release branch becomes **dead-end code** when that release is no longer supported — stop merging into it.

### Why Not "Just One Line"?

Linear development (single mainline, tag releases) works when:
- You have few customers who upgrade on your schedule.
- The mainline is always shippable.
- Releases are frequent and there is no divergence.

These conditions rarely hold at scale. With multiple customers, staggered upgrade cycles, and feature work that temporarily destabilises the codebase, a single line becomes a bottleneck.

---

## 2. Release-Prep Code Line (Chapter 18)

### Problem

> How do you stabilise a codeline for an impending release while also allowing new work to continue?

Before shipping, there is a burst of stabilisation work: last-minute bugs, installation tweaks, packaging details, QA sign-off. During this period you want:

- **Restrictive check-in policies** — no new features, only essential fixes.
- **No idle developers** — the rest of the team should not be blocked.

The traditional answer is a **code freeze**: stop all check-ins to the mainline until the release stabilises. This works if the freeze lasts hours, but real freezes often drag on for days or weeks. Developers work offline (bypassing version control), merge hell follows when the freeze lifts, and the release itself is delayed by the very process meant to protect it.

### Solution

**Branch instead of freeze.** Create a *release-engineering branch* (also called a release-prep branch) when the code is approaching release quality. The release is finished on this branch; the mainline stays open for new development.

```
                release-prep ──●──●──●  (stabilisation, packaging)
               ╱
main ─●──●──●──●──●──●──●──●──── (new feature work continues)
            ↑
      branch point (code "approaching done")
```

Guidelines:

- Branch as **late as possible** — the closer to "done" the code is, the less merging you will have to do between the two lines. But don't wait so long that you're already in a de facto freeze.
- The release-prep branch becomes the **release-maintenance branch** after shipping (it *is* the Release Line from pattern 1).
- The mainline codeline owner sets policy for how and when stabilisation fixes propagate from the release-prep branch back to mainline.

### Trade-offs

| Approach | Advantage | Disadvantage |
|---|---|---|
| **Code freeze** | Zero merge overhead during freeze | Developers idle or work outside VCS; chaos when freeze lifts |
| **Branch after release** (Release Line) | Clean separation of shipped vs. in-progress | Still need a freeze window before ship |
| **Branch before release** (this pattern) | No freeze; parallel work throughout | Merge overhead between the two lines |

### When the Team Is Small

If only a few people are working on the *next* release, invert the pattern: keep the mainline as the release-prep line and create a **Task Branch** (next pattern) for the forward-looking work. This avoids forcing the whole team to merge.

---

## 3. Task Branch (Chapter 19)

### Problem

> How can your team make multiple, long-term overlapping changes to a codeline without compromising its consistency?

Some development tasks should not be integrated incrementally:

- A major interface refactoring that breaks everything until it is complete.
- A new persistence mechanism being built by a small sub-team while the rest of the team fixes bugs on the current codebase.
- A feature targeting a release *after* the imminent one, being developed in parallel with release stabilisation.

Checking such work into the mainline piecemeal destabilises everyone. Working entirely outside version control — sharing patches via email or shared drives — loses traceability, recoverability, and visibility. The alternative, creating a full Release Line too early, forces heavy merge overhead for work that is not yet ready to share.

### Solution

**Fork off a separate branch for each activity that involves significant, disruptive changes.** The branch provides physical isolation while preserving all version-control benefits (history, backup, collaboration within the sub-team).

```
        task/refactor-persistence ──●──●──●──● (isolated work)
       ╱                            ↓ merge
main ─●──●──●──●──●──●──●──●──────●────────●──
```

Rules for task branches:

- **Integrate from mainline frequently.** Pull changes from the mainline into the task branch regularly so the final merge is not a collision of weeks of divergent code.
- **Merge back as a single transaction.** Once the task is complete, tested, and stable, merge the entire branch into the mainline at once.
- **Short-lived by design.** A task branch that lives for months becomes a parallel universe. If the task is truly long-running, consider whether it should be a Release Line or a separate project.
- **Lazy branching is ideal.** If your VCS supports "lazy" (copy-on-write) branching — where unchanged files are inherited from the parent — task branches are cheap to create and maintain.

### Task Branch vs. Release Line

| Aspect | Task Branch | Release Line |
|---|---|---|
| **Scope** | One feature / refactoring / sub-team effort | An entire shipped release |
| **Lifetime** | Days to weeks | Months to years (as long as the release is supported) |
| **Who works on it** | A small sub-team | Potentially the whole team (for maintenance) |
| **Merge direction** | Back into mainline, then branch is dead | Bidirectional — fixes flow both ways |
| **Policy** | Relaxed; sub-team self-governs | Formal; codeline owner sets propagation rules |

### When to Use a Task Branch

- A small group is working on a future-release feature while the rest of the team stabilises the current release (avoids creating a Release Line too early).
- A refactoring changes interfaces broadly and must land atomically.
- Multiple people need to share intermediate work that is not yet ready for the mainline.

---

## 4. Named Stable Bases (Chapter 20, Referenced)

> *Originally published in James O. Coplien, "A Generative Development-Process Pattern Language" (1995).*

### Intent

**Stabilise system interfaces no more than once a week.** Other software can be changed and integrated more frequently.

### How It Works

The idea is to establish a rhythm: every week (or at a similar cadence), the team agrees on a *named stable base* — a known-good snapshot where all module interfaces are frozen. Between stable bases, individual modules can evolve internally as fast as needed, but the contracts they expose to other modules do not change. This decouples *interface volatility* from *implementation volatility*.

In modern practice this maps to:

- **API versioning** — internal and external APIs change on a declared schedule, not continuously.
- **Release trains** — e.g., "we cut a release branch every two weeks; whatever is merged by Friday ships."
- **Contract testing** — pact/CDCT tests that verify consumer expectations against provider interfaces at stable-base boundaries.

### Relationship to Other Patterns

- A **Release Line** is a named stable base at the product level — the shipped interface is frozen.
- The **Daily Build and Smoke Test** (below) validates that yesterday's changes haven't broken today's stable base.

---

## 5. Daily Build and Smoke Test (Chapter 20, Referenced)

> *Originally published in James O. Coplien, "A Generative Development-Process Pattern Language" (1995).*  
> *Later popularised by Steve McConnell and Microsoft's "synch-and-stabilise" process.*

### Intent

**At least daily, build the entire software system and perform a smoke test to determine that the software is still usable.**

### The Problem It Solves

Integration errors are cheap to fix when caught immediately and exponentially expensive when discovered days or weeks later. Without a daily integration pulse:

- Developers work in isolation, then face a "big bang" merge.
- Broken builds go unnoticed for hours or days while more changes pile on top.
- The build itself atrophies — when no one runs it regularly, it stops working.

### How It Works

1. **Daily build.** Every night (or on every commit to the mainline), an automated process checks out the latest code, compiles it, links it, and produces a deployable artifact.
2. **Smoke test.** A fast, shallow test suite runs against the build to verify that the system starts, basic features work, and nothing is catastrophically broken.
3. **Immediate feedback.** If the build or smoke test fails, the team is notified immediately. Fixing the build becomes the top priority — no new check-ins until it is green.

### Modern Evolution

Today this pattern is table-stakes in professional software development:

- **CI/CD pipelines** (GitHub Actions, Jenkins, GitLab CI) automate the daily build.
- **Smoke tests** have evolved into **CI test suites** (unit + integration), **linting**, **security scanning**, and **deployment to staging**.
- The cadence has accelerated from "daily" to **"on every commit"** (continuous integration).
- **Build cop / build sheriff** rotations formalise the "fix the build first" rule.

### Relationship to Other Patterns

| Pattern | How Daily Build Supports It |
|---|---|
| **Release Line** | Ensures the mainline (and optionally release branches) are always in a known state |
| **Release-Prep Code Line** | Validates that stabilisation work on the release-prep branch hasn't broken anything |
| **Task Branch** | Developers should run the build on their task branch before merging back |
| **Named Stable Bases** | The daily build confirms the stable base hasn't regressed |

---

## Putting It All Together

A mature SCM process weaves these five patterns into a coherent workflow:

```
                    ┌─ Daily Build & Smoke Test ─┐
                    │  (runs on every codeline)   │
                    └─────────────────────────────┘
                                  │
        ┌─────────────────────────┼─────────────────────────┐
        ▼                         ▼                         ▼
   main (Active Dev)      release-prep/2.0          rel/1.0 (Maintenance)
        │                      │                         │
        │  Task Branch         │  Stabilise              │  Bug fixes
        │  (large features)    │  Package                │  only
        │      │               │  QA sign-off            │
        │      ▼               │       │                 │
        │  merge back ─────────┤       ▼                 │
        │                      │  Ship → becomes rel/2.0 │
        │                      │                         │
        ▼                      ▼                         ▼
   Named Stable Base     Named Stable Base         Named Stable Base
   (weekly interface     (release interface        (shipped interface
    freeze)                freeze)                   is frozen)
```

### Decision Heuristics

- **One codeline + labels** → Early-stage, single customer, always-shippable mainline.
- **Release Lines** → Multiple customers, staggered upgrades, need to patch old versions.
- **Release-Prep Branch** → Approaching a release, can't afford a freeze, large team.
- **Task Branch** → Small sub-team doing disruptive work; keep the mainline clean.
- **Named Stable Bases** → Multi-team / multi-module system; decouple interface churn from implementation churn.
- **Daily Build and Smoke Test** → Always. There is no scale or stage where skipping this is a good idea.

---

## Key Takeaways

1. **Branching is not overhead — it is risk management.** Each branch isolates a different category of change (maintenance, stabilisation, disruptive feature work) so they don't collide.

2. **Branch instead of freeze.** A short-lived release-prep branch costs some merges but eliminates idle time and the chaos of post-freeze integration.

3. **Merge frequently, in both directions.** The longer two codelines diverge, the more painful the eventual merge. Pull mainline changes into task/release branches regularly; push bug fixes back to mainline promptly.

4. **Task branches are for isolation, not abandonment.** They must be short-lived, frequently synced with mainline, and merged back as a single tested transaction.

5. **The Daily Build is non-negotiable.** Without a rhythm of integration and verification, all the branching structure in the world won't save you — you'll just have multiple broken codelines instead of one.

6. **Interfaces stabilise on a schedule.** Named Stable Bases decouple the pace of internal change from the pace of contract change, letting teams move fast without breaking each other.

---

## See Also

- [[Software Configuration Management Overview]] — map of all SCM patterns
- [[Version Control/03 Git Advanced]] — Git branching models (GitFlow, GitHub Flow, Trunk-Based Development) as modern implementations of these patterns
- [[Version Control/02 Git Workflows]] — team workflows that operationalise Release Lines and Task Branches
- *Streamed Lines* (Appleton et al., 1998) — deeper taxonomy of branching patterns
- *Software Release Methodology* (Michael Bays, 1999) — codeline types and release engineering
