---
title: "Evidence For The Mid To Senior Case"
role: Software Engineer
capability_area: Readiness for Senior
topic: Evidence For The Mid To Senior Case
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - senior-readiness
  - evidence
---

# Evidence For The Mid To Senior Case

> **Core skill:** Producing promotion evidence while you grow, not after

## What Changes from Junior to Mid

A junior engineer's progress is visible in their tickets: the work is assigned, the review is recorded, and the manager can see the output. A mid-level engineer's progress becomes harder to read from the tracker, because the valuable parts are the decisions, the unblocking, and the problems that never became tickets.

The promotion case is assembled from what you can show, and most engineers start collecting too late. The habit that prevents this is not a bigger document; it is capturing the artifact at the moment the work happens — the same week, in a form a stranger could understand a year later.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Visibility | Work is visible through assigned tickets | Work is visible through outcomes and artifacts |
| Record keeping | Relies on the tracker | Keeps a personal log of decisions and feedback |
| Feedback | Receives corrections in review | Actively asks for specific feedback and records it |
| Impact framing | "I completed the task" | "The feature changed this behavior" |
| Promotion thinking | Not applicable yet | Understands the case is built continuously |
| Gap awareness | Waits to be told what is missing | Tracks evidence gaps quarterly |

At senior, evidence becomes less about individual artifacts and more about sustained scope and direction — which is why the habit must be in place before the level changes.

## Core Frameworks

### What Counts as Evidence at This Stage

| Evidence type | What it proves | Artifact | Capture cadence |
|---------------|----------------|----------|-----------------|
| Ownership trail | You hold an area, not just tasks | Charter, backlog grooming, runbook, incident notes | At the start and whenever scope changes |
| Decisions documented | You exercise judgment and can defend it | ADRs, design notes, problem docs | Same week as the decision |
| Feedback received | Others experience you as senior | 1:1 notes, review comments, peer messages | Within days of receiving it |
| Fixes initiated | You find problems without being asked | Ticket you opened with context, postmortem action you led | When the problem is found |
| Team lift | Your work changes others' output | Onboarding doc, review habit, mentoring notes, shared tooling | When the change lands |
| Delivered outcomes | You finish what you own | Rollout notes, before-and-after telemetry, demo notes | At completion |

### Strong vs Weak Evidence

| Weak evidence | Why it fails | Strong version |
|---------------|--------------|----------------|
| "I improved performance" | No baseline, no measurement, no attribution | A before-and-after graph with the change that caused it |
| "I mentor people" | Unverifiable and vague | A named person's ramp plan and their first independent delivery |
| "I led the project" | Team outcomes are not personal outcomes | "I framed the problem, wrote the ADR, and coordinated the rollout; X built the UI" |
| A list of tickets | Shows activity, not scope or judgment | Three artifacts that show decisions and their consequences |
| Praise forwarded without context | Compliments are not evidence of level | A specific behavior someone relied on, with the situation |
| A single heroic launch | Spikes are discounted as outliers | The same behavior repeated over two or more quarters |

### The Quarterly Evidence Review

| Step | Question | Output |
|------|----------|--------|
| 1. Collect | What artifacts exist from the last quarter? | A dated list with links, not a narrative |
| 2. Map | Which senior dimension does each artifact support? | One row per artifact, tagged by dimension |
| 3. Mark gaps | Which dimensions have no artifact at all? | A short, honest gap list |
| 4. Choose two | Which two gaps matter most for the next cycle? | Two focus areas, not five |
| 5. Align | Does my manager agree with this list? | Their view recorded verbatim |
| 6. Schedule | When is the next review? | A date in the calendar |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Work happens"] --> B["Capture the artifact same week"]
    B --> C["Tag it to a senior dimension"]
    C --> D["Review the collection quarterly"]
    D --> E["Name the empty dimensions"]
    E --> F["Choose two focus areas"]
    F --> G["Agree the list with your manager"]
    G --> A
```

## In Practice

**Capture the artifact while the context is still fresh.** A decision explained a week later loses the constraints that made it hard. Write the ADR, the incident note, or the problem doc on the day, even in rough form. The goal is a durable pointer to what happened, not a polished essay.

**Write for a reader who was not in the room.** The promotion case will be read by people who do not know your project, your team, or your manager. Every artifact should answer: what was the situation, what did you do, what changed, and how would someone verify it. If the artifact needs you present to make sense, it is a note to self, not evidence.

**Record feedback in the words it was given.** Paraphrasing praise into "leadership" strips out the specifics that make it credible. Keep the exact sentence and the context: who said it, about which behavior, and when. Specific feedback also tells you which dimension is actually landing.

**Attribute honestly and precisely.** "I led X with Y's support" is stronger than "I built X", because it survives a reference check and shows you understand team output. Committees probe attribution; a precise claim that gives credit away reads as more credible, not less.

**Ask for the evidence conversation before the cycle.** In a 1:1, ask: "If I applied next cycle, which of these artifacts would you use, and what is missing?" The answer tells you whether your mental model of senior matches your manager's, and it gives you a quarter to close the gap instead of a week.

**Treat the evidence log as a by-product, not a project.** The log should take minutes a week, drawing on artifacts that already exist for other reasons. If maintaining it becomes a separate job, you are either writing too much or doing work that produces no artifacts at all — both are useful signals.

**Do not manufacture evidence for its own sake.** Starting a book club to tick "team lift" or writing an ADR for a decision nobody will revisit is evidence theater. Reviewers can tell the difference between artifacts that changed something and artifacts that were produced to be seen. The work comes first; the record follows.

## Practical Exercise

1. Create a single evidence file — one document, chronological, one entry per artifact.
2. Backfill the last two quarters from memory, tickets, docs, and chat history; mark each entry with a date and a link.
3. Tag every entry with one of the six evidence types and note the senior dimension it supports.
4. Count the entries per dimension and identify the two dimensions with the fewest.
5. For each empty dimension, write one sentence describing the smallest piece of real work that would produce an artifact.
6. Put both sentences into your next 1:1 agenda and ask your manager whether they agree with the gaps.
7. Add a five-minute weekly calendar block to capture the week's artifacts, and a quarterly review date.

## Practical Applications

- [ ] I have one evidence file with dated, linked entries
- [ ] Entries are tagged by evidence type and senior dimension
- [ ] The file covers at least the last two quarters
- [ ] I can show an artifact for every claim I would make in a promotion conversation
- [ ] My manager has reviewed the gap list and agreed with it
- [ ] Capturing the week's artifacts takes five minutes, not an afternoon
- [ ] Nothing in the file exists only to be seen

Evidence log entry template:

```text
## <date> — <short title>

**Type:** ownership / decision / feedback / fix initiated / team lift / outcome
**Dimension:** system area / framing / judgment / team lift / communication
**Situation:** <one or two sentences of context>
**What I did:** <specific actions and decisions>
**What changed:** <observable outcome, with a link or measurement>
**Artifact:** <link to ADR, doc, ticket, dashboard, message>
**Attribution:** <who else contributed and how>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Starting the file when the cycle opens | The evidence is stale, missing, or reconstructed from memory | Capture weekly; review quarterly |
| Writing narrative instead of linking artifacts | Claims without pointers cannot be verified | One link per claim, dated |
| Recording only successes | A case with no setbacks reads as curated, not credible | Include a decision you reversed and what you learned |
| Letting the log become a diary | Volume buries the signal and maintenance collapses | One entry per artifact, six fields, no prose |
| Claiming team outcomes alone | Attribution gets probed and trust is lost | Name who did what, including your part |
| Collecting artifacts nobody asked for | Evidence theater is visible and discounts the real work | Only log work that changed something |
| Avoiding the gap conversation | You spend a quarter polishing the wrong dimension | Ask your manager which artifacts they would actually use |

## Success Indicators

- Every promotion claim you might make has a link behind it
- The evidence file spans multiple quarters without gaps in the core dimensions
- Your manager's view of your gaps matches your own
- Capturing evidence is a small weekly habit, not a seasonal panic
- Artifacts make sense to someone who never worked on your team
- You can name a decision you reversed and what the reversal taught you

## Related

- [[00_overview]]
- [[01_What_Senior_Really_Means]]
- [[04_Design_Judgment_Basics]]
- [[career-path/01_Software_Engineer/01_Leveling_and_Growth/07_The_Brag_Document|The Brag Document]]
- [[career-path/02_Senior_Software_Engineer/09_Promotion_Evidence_and_Capstone/07_Mid_to_Senior_Promotion|Mid to Senior Promotion]]
- [[career-path/02_Senior_Software_Engineer/09_Promotion_Evidence_and_Capstone/02_Evidence_Collection|Evidence Collection]]

## Sources

- *The Staff Engineer's Path* (Tanya Reilly)
- *Thanks for the Feedback* (Douglas Stone, Sheila Heen)
- *The Pragmatic Programmer* (David Thomas, Andrew Hunt)
