---
title: "Design Judgment Basics"
role: Software Engineer
capability_area: Readiness for Senior
topic: Design Judgment Basics
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - senior-readiness
  - design-judgment
---

# Design Judgment Basics

> **Core skill:** Making and documenting medium-risk design decisions

## What Changes from Junior to Mid

A junior engineer implements a design that someone else chose, and learns the codebase's patterns by following them. A mid-level engineer chooses the implementation shape inside an agreed approach: which module owns a behavior, how data flows, where the test seams go. Most of those choices are local and cheap to reverse.

Design judgment begins when the choice stops being local. A medium-risk decision changes an interface other teams use, a data shape that is expensive to migrate, or a failure mode that will page someone at night. The mid-level skill is to make those calls deliberately, write down the reasoning, and invite review before the cost of being wrong compounds.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Design input | A described approach | An agreed outcome and constraints |
| Decision scope | Local implementation detail | Module boundaries, interfaces, data shape |
| Options considered | The first one that works | Two or three, with trade-offs named |
| Documentation | Code comments | A short design note or ADR |
| Review | Seeks approval on the approach | Seeks challenge on trade-offs |
| Reversibility | Not considered | Weighs cost of change and chooses a seam |
| Failure handling | Assumes the happy path | Names what happens when a dependency fails |

At senior, the same reasoning applies to system-level decisions: cross-team contracts, platform choices, and investments whose payoff is a year away.

## Core Frameworks

### Decision Risk Tiers

| Tier | Examples | Reversibility | Required process |
|------|----------|--------------|------------------|
| Low | Naming, internal helpers, test structure, log format | Cheap: revert in a day | Decide alone; mention in the PR |
| Medium | Public interface shape, schema addition, queue semantics, retry policy, caching layer | Moderate: a migration or a coordinated rollout | Written ADR with options; one reviewer outside the feature |
| High | Storage engine, service boundary, authentication model, data ownership | Expensive: quarters or a rewrite | Options memo, design review, explicit owner sign-off |

### A Lightweight ADR Format

| Section | Content | Quality bar |
|---------|---------|-------------|
| Title and status | Short decision name; proposed, accepted, superseded | A reader can find it by searching the problem, not the solution |
| Context | The forces at play: requirements, constraints, current pain | No solution language; a stranger can understand the pressure |
| Options | Two to four realistic alternatives, including "do nothing" where viable | Each option has a fair summary and its strongest argument |
| Decision | The chosen option, in one paragraph | States what changes and what deliberately does not |
| Consequences | What gets better, what gets worse, what becomes harder later | Includes the cost you are accepting, not only the benefit |
| Triggers | Conditions under which this decision should be revisited | A date, a scale threshold, or an event |

### Choosing Reversibility

- **Feature flag** — buys instant rollback and a staged rollout, costs a second code path and flag debt to clean up.
- **Interface seam** — buys the ability to swap implementations later, costs an abstraction that may be premature.
- **Expand-contract migration** — buys a schema change without a flag day, costs two write paths for a transition period.
- **Small batches** — buy cheap correction when the design is wrong, cost more releases and more coordination.
- **Default off** — buys reversibility without a redeploy, costs the risk that nobody owns the flip.
- **Compatibility window** — buys time for consumers to migrate, costs old paths that linger until retired on purpose.

### Common Beginner Design Errors

| Error | Symptom | Correction |
|-------|---------|------------|
| Over-engineering | Abstraction layers for a second use case that never arrives | Build for the current requirement plus one known near-term case |
| Cargo-culting patterns | A familiar architecture applied without its preconditions | Name the constraint the pattern solves, then check you have it |
| Premature generalization | Configurability nobody asked for | Hard-code until the second real variation appears |
| Ignoring operational cost | Design looks clean but needs three new dashboards and a runbook | Include operability in the options, not as an afterthought |
| Single-option ADR | The document records a decision already made | Require at least one genuine alternative |
| Deciding in chat | The reasoning disappears and gets relitigated | Move medium-risk decisions into a written note |
| Optimizing for elegance | The design is praised, the failure modes are unknown | Ask what happens when each dependency is slow, down, or wrong |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Notice a decision worth recording"] --> B["Classify its risk tier"]
    B --> C["List realistic options and trade-offs"]
    C --> D["Choose the most reversible adequate option"]
    D --> E["Write the ADR with consequences"]
    E --> F["Seek review from outside the feature"]
    F --> G["Decide, record status, set revisit trigger"]
```

## In Practice

**Classify the decision before you spend time on it.** Most design energy should go to medium-risk calls, because low-risk decisions do not need ceremony and high-risk ones need more than one person. Misclassifying upward produces slow teams; misclassifying downward produces migrations nobody planned.

**Write the context before you write the options.** The context section is where you discover whether you actually understand the problem. If the context reads like a justification for a solution you already chose, stop and rewrite it until a colleague could propose a different option from it.

**Always include a real alternative.** An ADR with one option is a memo announcing a decision. Include the option you rejected, with its strongest argument, so future readers can tell whether the rejection still holds. "Do nothing" is often the most useful alternative because it forces the cost of the status quo into the open.

**Choose reversibility when the uncertainty is high, and simplicity when it is low.** Flags and seams are insurance, and insurance has a premium. If you expect to learn the answer within a sprint, buy the option that lets you undo it. If the requirement is stable and well understood, the simpler design is usually the better one.

**Record the consequences you are accepting.** Every design trades something away: operational simplicity for flexibility, latency for consistency, speed for safety. Write the cost down explicitly. A decision note that lists only benefits is marketing, and the next engineer will not know which constraints were deliberate.

**Ask for review from someone who does not share your assumptions.** A reviewer inside the feature will tend to fill the same gaps you did. Bring the ADR to a neighbouring team or a more senior engineer and ask a specific question: "Where would this break?" Specific questions get specific challenges.

**Set a trigger for revisiting.** Decisions expire when their context changes. Name the condition — traffic level, team size, a new compliance rule — that would reopen the question. Without a trigger, old decisions silently become constraints that nobody remembers choosing.

## Practical Exercise

1. List the last five design decisions you made, including ones you made implicitly by following a pattern.
2. Classify each as low, medium, or high risk using the tiers table, and note which ones you never wrote down.
3. Pick one medium-risk decision and write an ADR using the format above, with at least two options and a do-nothing option.
4. For each option, write one sentence on what gets harder if it is chosen.
5. Choose the most reversible adequate option and state the revisit trigger.
6. Send the ADR to one engineer outside the feature and ask where it would break; record their answer.
7. Update the ADR status and link it from the code or the ticket where the decision lives.

## Practical Applications

- [ ] I classify design decisions by risk before choosing a process
- [ ] Every medium-risk decision I made recently has a written ADR
- [ ] My ADRs contain at least two real options and an explicit do-nothing where relevant
- [ ] Each ADR lists consequences, including the costs I accepted
- [ ] I have asked a reviewer outside my feature to challenge a design
- [ ] I can name the seam or flag that makes my riskiest recent decision reversible
- [ ] My ADRs have revisit triggers, not just dates

ADR template:

```markdown
# ADR: <decision in a few words>

- **Status:** proposed / accepted / superseded by <link>
- **Date:** <date>
- **Decision owner:** <name>
- **Risk tier:** medium

## Context
<requirements, constraints, current pain — no solution language>

## Options
### Option A: <name>
<what it is; strongest argument for it; what it makes harder>

### Option B: <name>
<what it is; strongest argument for it; what it makes harder>

### Option C: do nothing
<what happens if we leave it as is, and for how long that is acceptable>

## Decision
<the chosen option and the reasoning in one paragraph>

## Consequences
- Better: <...>
- Worse: <...>
- Becomes harder later: <...>

## Review trigger
<date or condition that reopens this decision>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Writing the ADR after implementation | The document becomes a justification, not a decision aid | Write it while the options are still open |
| Treating every decision as high risk | Reviews become a bottleneck and people route around them | Reserve ceremony for medium and high tiers |
| Only one option in the document | No evidence the alternatives were considered | Include a rejected option and its best argument |
| Confusing reversibility with indecision | Flags pile up and nobody removes them | Attach an owner and a cleanup date to every reversible choice |
| Ignoring operational consequences | The design ships clean and pages someone later | Add failure behavior and on-call cost to every option |
| Cargo-culting an architecture | The pattern's preconditions are missing, so its benefits are too | Name the constraint the pattern assumes and check it |
| Never revisiting accepted ADRs | Old decisions harden into unexplained constraints | Set review triggers and supersede explicitly |

## Success Indicators

- Medium-risk decisions in your area have a written record with options
- Reviewers engage with your trade-offs instead of asking basic questions
- You can explain why the rejected option was rejected, months later
- Your designs include failure behavior and operational cost by default
- You have reversed a decision cleanly because you built a seam for it
- Other engineers cite your ADRs when making adjacent decisions

## Related

- [[00_overview]]
- [[03_Problem_Framing_Before_Solutioning]]
- [[05_Evidence_For_The_Mid_To_Senior_Case]]
- [[career-path/02_Senior_Software_Engineer/03_Architecture_and_Design_Judgment/00_overview|Architecture and Design Judgment]]
- [[career-path/02_Senior_Software_Engineer/01_Technical_Ownership/00_overview|Technical Ownership]]
- [[career-path/02_Senior_Software_Engineer/09_Promotion_Evidence_and_Capstone/07_Mid_to_Senior_Promotion|Mid to Senior Promotion]]

## Sources

- *A Philosophy of Software Design* (John Ousterhout)
- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright)
- *The Pragmatic Programmer* (David Thomas, Andrew Hunt)
