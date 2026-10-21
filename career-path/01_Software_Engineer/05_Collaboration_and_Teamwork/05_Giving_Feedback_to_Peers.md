---
title: "Giving Feedback to Peers"
role: Software Engineer
capability_area: Collaboration and Teamwork
topic: Giving Feedback to Peers
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - collaboration
  - feedback
---

# Giving Feedback to Peers

> **Core skill:** Reviewing work and behavior with specifics and respect

## What Changes from Junior to Mid

A junior engineer either stays silent about a peer's work or blurts a reaction that lands as an insult. A mid-level engineer gives feedback as a service: behavior plus impact plus a concrete alternative, delivered while the context is alive, in private unless it is praise. They also receive feedback without armor, and they give it across disciplines — to PMs, designers, and testers — with the same care. The shift is from "honesty as a personality" to "honesty as a craft."

| Aspect | Junior | Mid |
|--------|--------|-----|
| Format | "This is bad" or silence | Behavior + impact + alternative, in that order |
| Timing | Saves it for review season | Gives it within days, while the context is still alive |
| Channel | Public threads when upset | Private for correction, public for praise |
| Code review | Comments as taste assertions | Comments as feedback with reasoning and options |
| Receiving feedback | Defends, explains, deflects | Asks questions, thanks, and follows up with change |
| Across disciplines | Only reviews engineers | Gives PMs, designers, and testers the same quality of feedback |

## Core Frameworks

### The SBI Model

| Part | What It Is | What It Sounds Like | Common Failure |
|------|------------|---------------------|----------------|
| Situation | The specific time and place, anchored in shared memory | "In yesterday's deploy review…" | Vague anchors like "lately" or "always" trigger defensiveness |
| Behavior | The observable action, described without motive-reading | "…you cut the rollback plan with 'we'll handle it if it breaks'" | Adjectives like "careless" or "dismissive" read as character attacks |
| Impact | The concrete effect on work, people, or outcomes | "…so the on-call engineer had no plan for the failure that followed" | Impact on you alone feels like a complaint; name the shared cost |
| Alternative | What to do instead, stated as a real option | "Next time, can we keep the fallback in the plan even when time is tight?" | Feedback without an alternative leaves the person with only the criticism |

### When and Where to Give Feedback

| Situation | Timing | Channel | Note |
|-----------|--------|---------|------|
| Behavior correction | Within days of the event | Private first: direct message, hallway, or 1:1 | Delayed feedback loses context and gains resentment |
| Praise | Promptly | Public unless the person is private by preference | Specific praise teaches the room what good looks like |
| Code review comment | In the review, same cycle | Inline, with reasoning | Review is a feedback channel with a built-in artifact |
| Pattern across many events | When the pattern is named, not after one blip | Private, with two or three examples | One event is a note; a pattern is feedback |
| Urgent safety or trust issue | Immediately | Private, and fast | Some moments cannot wait for the perfect timing |
| Feedback to a manager or a senior | Promptly | Private; frame as your perspective | Hierarchy does not exempt anyone from useful information |

### Review Comments as Feedback

| Comment Type | Example | Effect |
|--------------|---------|--------|
| Taste assertion | "I wouldn't do it this way" | Teaches nothing; invites a style war |
| Blocking with reasoning | "This retry can double-charge; here is the failure case" | Prevents harm and teaches the failure mode |
| Question comment | "What happens if the cache is cold here?" | Lets the author find the issue and keeps ownership with them |
| Suggestion with an alternative | "Extracting this into X would make the retry testable; want me to pair on it?" | Improves the code and the relationship |
| Praise with specifics | "The error cases in this test are exactly the ones we kept missing" | Reinforces the behavior and shows what the team values |
| Nit labeled as nit | "Nit: naming only — take or leave" | Keeps the signal-to-noise ratio honest |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Notice the specific behavior"] --> B["Wait for calm and choose the channel"]
    B --> C["Describe situation and behavior"]
    C --> D["State the concrete impact"]
    D --> E["Offer a concrete alternative"]
    E --> F["Listen to their side"]
    F --> G["Follow up after they act on it"]
```

## In Practice

**Describe behavior, not character.** Everything you can observe is behavior: what was said, what was written, what was skipped. The moment you name a trait — "you are disorganized," "you do not care about quality" — the conversation becomes about the person's identity and no longer about the work. Specific behavior is something a person can change tomorrow morning; a trait is a verdict.

**Always attach the impact.** Feedback without impact is a preference; feedback with impact is information. Name what the behavior did to the work, the team, or a specific person: the deploy that stalled, the tester who chased a false bug, the decision that got made without data. Impact is also what makes the feedback hard to dismiss — you are describing consequences, not tastes.

**Offer a real alternative, not just a complaint.** "Can you write the plan earlier?" is a path forward; "stop surprising us" is only a wall. The alternative does not have to be your way — "what would work for you?" is a legitimate alternative offer — but the person must leave with something to do differently. Feedback that ends at criticism is a vent; feedback that ends at an option is a gift.

**Give correction in private and praise in public, with judgment for exceptions.** Private correction protects the relationship and the person's standing with the room; public praise shows everyone what good looks like. If the behavior happened in public and the correction matters to the whole group, correct the pattern in general terms in the group and the specifics in private. When in doubt, choose the channel with less audience.

**Keep it timely: feedback has a half-life.** Within a few days, the situation is fresh enough to describe precisely and the person remembers their own reasoning. Months later, the same feedback arrives as a stored grievance, and every example sounds invented. If you missed the window, name the pattern honestly ("this has happened a few times since March") instead of pretending one stale event is the whole case.

**Receive feedback with curiosity before defense.** The first job is to understand: "say more about the moment you mean" buys you accuracy and time. You can disagree — but disagree after you can state their point back to them to their satisfaction. Defensiveness trains people to stop telling you things, and the feedback you never hear is the feedback that shapes your reviews for you.

**Calibrate the craft to the person and the discipline.** A designer needs feedback phrased in user goals and flows; a PM in outcomes and trade-offs; a junior engineer in behavior they can practice this week. Across disciplines, ask permission and check the frame: "can I give you a read on how that meeting landed for me?" The model stays the same — behavior, impact, alternative — but the vocabulary meets them where they work.

## Practical Exercise

Run a full feedback cycle on realistic situations:

1. Take a raw reaction — "this pull request is a mess" — and rewrite it with SBI: name the situation, the observable behaviors, and the concrete impact on the work.
2. Add a concrete alternative to the rewrite, phrased as an option the person could take or adapt.
3. Choose the channel and timing for each of three cases: a one-time slip, a repeated pattern, and outstanding work worth praising; justify each choice in one sentence.
4. Write two code review comments for the same flaw: one as a taste assertion, one as a question comment with reasoning; compare what each teaches.
5. Role-play receiving harsh feedback on your own work: respond with a clarifying question and a restatement of their point, without explaining yourself.
6. Write the same feedback for a non-engineer: phrase a collaboration concern for a PM in outcomes and trade-offs instead of engineering terms.
7. Draft the follow-up message after the person acts on your feedback: what you noticed and what it changed.

## Practical Applications

- [ ] My feedback names observable behavior and never a personality trait
- [ ] Every piece of correction states the concrete impact on work or people
- [ ] Correction arrives with at least one doable alternative
- [ ] I give feedback within days of the event, in private unless it is praise
- [ ] My code review comments carry reasoning, and taste is labeled as taste
- [ ] When feedback arrives, I understand it fully before I respond to it
- [ ] I can give the same quality of feedback to PMs, designers, and testers

Peer feedback template:

```text
## Situation
<when and where, anchored in shared memory>

## Behavior I observed
<what was said or done, without interpretation>

## Impact
<effect on the work, the team, or a specific person>

## Alternative I want to offer
<what to do differently — as an option, or a question>

## Your side
<what I want to understand: your reasoning, constraints, context>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Feedback as personality judgment | The person defends their identity instead of changing behavior | Describe observable behavior and concrete impact |
| The feedback sandwich | Praise gets discounted as packaging and the message gets lost | Say the thing plainly, with respect and an alternative |
| Saving feedback for review season | Context is gone; the examples feel invented | Give it within days while the situation is alive |
| Correcting in public threads | Shame triggers defensiveness and teaches the room to hide | Private for correction; public only for praise |
| Criticism with no alternative | The person leaves with a verdict and no path | Offer an option, or ask what would work for them |
| Defending before understanding | You never learn if the feedback is right, and people stop giving it | Ask questions and restate their point first |
| Only reviewing engineers | Cross-functional friction never gets addressed in the open | Use SBI with PMs, designers, and testers too |

## Success Indicators

- People act on your feedback and come back to show you the result
- Your code review comments get replies like "good catch" and "that helped"
- You can name the last piece of feedback you received and what you changed
- Correction from you never surprises anyone — it is expected and specific
- Non-engineers accept your feedback because it speaks their outcomes
- Your praise is quoted; people know exactly what you considered good

## Related

- [[00_overview]]
- [[04_One_On_Ones_With_Your_Manager]]
- [[07_Handoffs_and_Knowledge_Sharing]]
- [[career-path/01_Software_Engineer/02_Daily_Engineering_Craft/00_overview|Daily Engineering Craft]]
- [[software-engineering-note/14_Software_Engineering_Professional_Practice/Professionalism of Software Engineering Overview]]

## Sources

- *Radical Candor* (Kim Scott)
- *Thanks for the Feedback* (Douglas Stone, Sheila Heen)
- *Nonviolent Communication* (Marshall B. Rosenberg)
- *Crucial Conversations* (Kerry Patterson, Joseph Grenny, Ron McMillan, Al Switzler)
