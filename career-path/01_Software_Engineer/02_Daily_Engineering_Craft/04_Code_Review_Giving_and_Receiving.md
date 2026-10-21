---
title: "Code Review Giving and Receiving"
role: Software Engineer
capability_area: Daily Engineering Craft
topic: Code Review Giving and Receiving
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - craft
  - code-review
---

# Code Review Giving and Receiving

> **Core skill:** Reviewing for correctness and learning, not approval theater

## What Changes from Junior to Mid

A junior engineer reviews by reading every line equally and comments on what looks wrong; receiving feedback feels personal. A mid-level engineer reviews in layers — design first, then correctness, then style last — labels each comment so the author knows what must change, and phrases comments so behavior actually shifts. Receiving review is a search for the truth in the comment, not a defense of the work.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Review goal | Find mistakes and approve | Catch defects, spread knowledge, hold standards |
| Reading order | Line by line from the top | Design and intent before the diff, details after |
| Comment style | "This is wrong" or "Nit:" on everything | Labeled: blocking, non-blocking, question, praise |
| Reaction to feedback | Defensive or over-compliant | Evaluates the claim, changes code or argues with reasons |
| Scope | The changed lines | The changed lines plus the blast radius around them |
| Timing | Reviews whenever convenient | Reviews promptly; responsiveness is a team courtesy |

## Core Frameworks

### What Review Is For

| Purpose | What Good Review Produces | What Review Theater Produces |
|---------|--------------------------|------------------------------|
| Defects | Logic errors, edge cases, and regressions caught before merge | Style nits while real bugs pass |
| Knowledge spread | At least one reviewer who can maintain this code | Approval by whoever clicked first |
| Standards | Consistent patterns, APIs, and error handling across the codebase | Endless local debates repeated on every PR |
| Design sanity | A cheaper design discovered before it ships | Architecture discovered only in production |
| Shared ownership | No single point of failure on any module | Code nobody dares touch |

### Comment Taxonomy

| Label | Meaning | Author Must | Example Phrasing |
|-------|---------|-------------|------------------|
| Blocking | Correctness, data loss, security, or a broken contract | Change before merge | "Blocking: this retry can duplicate the charge on timeout." |
| Non-blocking | Improvement worth doing, not required now | Decide: fix now or file a follow-up | "Non-blocking: extracting this parser would help the next caller too." |
| Question | Reviewer does not understand something | Answer; the answer often becomes a comment | "Question: what happens if the list is empty here?" |
| Praise | Something done well, worth repeating | Nothing; internalize | "Nice: naming the states explicitly makes the transitions obvious." |

### Phrasing That Changes Behavior

| Pattern | Effect | Example |
|---------|--------|---------|
| State the risk, not the verdict | Author sees the consequence and decides | "If two jobs overlap here, both write the file" vs "Race condition" |
| Ask what happens in a case | Author discovers the gap themselves | "What does this return when the cache misses?" |
| Propose with a reason | Author can weigh the trade-off | "Consider a guard here, because the caller can pass empty input." |
| Label severity explicitly | Author knows what blocks merge | "Blocking:" or "Non-blocking:" prefix |
| Praise specifically | The good pattern gets repeated | "The error message here will save us a debugging hour." |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Read intent and design"] --> B["Assess the design first"]
    B --> C["Review correctness and blast radius"]
    C --> D["Label every comment"]
    D --> E["Author evaluates the claim"]
    E --> F["Resolve every thread"]
    F --> G["Learn and repeat"]
```

## In Practice

**Review the design before the lines.** Read the PR description and, if needed, the issue before opening the diff. Ask whether this should exist, whether the shape is right, and where it will be in a year — a design flaw caught in review costs minutes; the same flaw in production costs a rewrite. Line-level polish on a wrong design is wasted effort.

**Label every comment with its severity.** Blocking, non-blocking, question, praise. Unlabeled comments force the author to guess what matters and turn every nit into a negotiation. A comment taxonomy is the cheapest way to keep review focused on what must change.

**Phrase comments around consequences, not judgments.** "This is wrong" invites defense; "if this retries twice, the user is charged twice" invites reasoning. State the risk or ask about the edge case, and let the author draw the conclusion. The goal is a better change and a better engineer, not a demonstration that you were right.

**Review the blast radius, not just the diff.** Changed lines are where the author looked; bugs hide where the change reaches: callers, serialization, error paths, concurrency, performance under load. Spend part of every review asking what else this touches and verifying those assumptions.

**Receiving review: evaluate the claim, not the tone.** Assume good intent and translate the comment into a factual claim you can check: is there a real edge case here, or a style preference? Fix real issues without negotiation, argue preferences with reasons, and follow up on deferred items. Your reputation grows when people see you change your own code willingly.

**Respond to every comment with a resolution.** Done, argued, or deferred with a tracked follow-up — silence is the one wrong answer. Reviewers invest scarce attention; closing the loop tells them the attention was worth it and makes the next review round faster.

**Treat review responsiveness as team courtesy.** A PR waiting days for review blocks a teammate's whole thread of work. Review promptly, even if briefly — a first-pass design comment within hours is worth more than a perfect review next week. Protect your own focus too: batch review windows instead of constant interruption.

## Practical Exercise

Review a real pull request in layers and then receive review on your own:

1. Read the PR description and the linked issue before the diff; write one sentence on the change's intent and one on its risks.
2. Assess the design first: does this belong here, is the shape right, what will this look like after the next three similar changes? Note any design comments.
3. Read the diff for correctness: edge cases, error handling, concurrency, callers beyond the diff; write at least one question about a boundary case.
4. Label each of your comments blocking, non-blocking, question, or praise; rewrite any comment that states a verdict instead of a consequence.
5. Submit the review; then pick one of your own open PRs and read incoming feedback, translating each comment into a factual claim.
6. Fix the real issues, reply to every thread with a resolution, and file follow-ups for anything deferred.
7. In your next team sync or PR thread, note which phrasing patterns produced the fastest, least defensive discussion.

## Practical Applications

- [ ] I read the design and the description before reviewing any line
- [ ] Every comment I write is labeled blocking, non-blocking, question, or praise
- [ ] My comments state consequences or ask about cases, not verdicts
- [ ] I checked callers and blast radius, not only the changed lines
- [ ] I included at least one specific piece of praise when the work deserved it
- [ ] On my own PRs, I answer every thread with done, argued, or deferred with a follow-up
- [ ] I review within a working day, or I say explicitly when I cannot
- [ ] I separate style preferences from correctness claims in my own head before commenting

Review response template:

```text
# Review response: <PR name>

| Comment | Type | Resolution |
|---------|------|------------|
| <summary> | blocking | fixed in commit <sha or name> |
| <summary> | question | answered: <one line> |
| <summary> | non-blocking | deferred to <ticket>; reason: <why> |
| <summary> | preference | argued: <trade-off>; open to counter-argument |

Follow-ups filed: <ticket list>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Reviewing line by line without reading intent | Polish on a wrong design wastes everyone's time | Design first, then the diff |
| Unlabeled nits and blockers look identical | Author guesses or negotiates every comment | Label: blocking, non-blocking, question, praise |
| Rewriting the code in comments | Signals distrust; the author disengages | State the risk or question; let the author fix it |
| Rubber-stamping large PRs | Defects pass because nobody really looked | Ask for smaller slices; review what you can hold |
| Taking review feedback personally | Defense mode blocks learning and slows the PR | Evaluate the factual claim, not the tone |
| Leaving comments unanswered | Reviewer cannot tell if their time mattered | Every thread gets a resolution |
| Letting PRs sit unreviewed | Blocks teammates and trains everyone to batch huge PRs | Review promptly or state when you will |

## Success Indicators

- Your reviews catch design problems before line-level problems
- Authors say your comments are specific, kind, and actionable
- Review threads on your PRs close quickly with clear resolutions
- You change your code after review without it feeling like defeat
- Teammates trust you to review their hardest changes
- Comment labels are used consistently across the team because you modeled them

## Related

- [[00_overview]]
- [[03_Git_and_Pull_Request_Hygiene]]
- [[05_Testing_in_Daily_Work]]
- [[career-path/05_Tech_Lead/06_Process_and_Quality_Stewardship/00_overview|Process and Quality Stewardship]]
- [[software-engineering-note/04_Software_Construction/Software Construction Overview]]
- [[career-path/02_Senior_Software_Engineer/00_overview|Senior Software Engineer]]

## Sources

- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright)
- *The Pragmatic Programmer* (David Thomas, Andrew Hunt)
- *A Philosophy of Software Design* (John Ousterhout)
