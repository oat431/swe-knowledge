---
title: "Working With Product Managers"
role: Software Engineer
capability_area: Collaboration and Teamwork
topic: Working With Product Managers
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - collaboration
  - product-management
---

# Working With Product Managers

> **Core skill:** Turning fuzzy requests into shared, buildable understanding

## What Changes from Junior to Mid

A junior engineer takes a ticket from the product manager and builds what it says. A mid-level engineer treats the ticket as the start of a conversation: restates the problem in their own words, surfaces the constraints the ticket does not mention, proposes alternatives when the request is expensive, and agrees with the PM on what success looks like before writing code. The shift is from receiving requirements to co-owning the outcome.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Reading a request | Takes the acceptance criteria literally | Restates the underlying problem and checks the framing |
| Scope conversation | Says yes, then slips quietly | Surfaces trade-offs and renegotiates before committing |
| Pushback | "This is hard" or silent resentment | "This is expensive because X; here are two cheaper options" |
| Discovery meetings | Listens and waits for tasks | Asks the engineer's questions: data volume, failure modes, migration |
| Success signals | Ships the feature | Agrees with the PM on how we will know it worked |
| Tickets | Treats them as the spec | Treats them as a placeholder for a conversation held |

## Core Frameworks

### Who Owns What

| Concern | Product Manager Owns | Engineer Owns | Shared |
|---------|----------------------|---------------|--------|
| Problem and priority | Why this problem, why now, what it is worth | Whether the problem is technically real as described | Reframing the problem when discovery reveals new facts |
| Solution shape | Which user outcomes the solution must deliver | How the solution is built, and what is cheap or expensive | Choosing between alternatives once costs are visible |
| Scope and timeline | Business value ranking and date pressure | Honest effort estimates and technical risk disclosure | What gets cut when the timeline is fixed |
| Success signals | Which business or user metric defines success | Which technical signals (latency, errors, adoption) we can measure | Reading the results together after launch |
| Execution detail | None | Sequencing, design decisions, technical trade-offs | Surfacing scope changes the moment they appear |

### The Restate-Constraints-Signals Conversation

| Step | Your Move | Sample Language | Done When |
|------|-----------|-----------------|-----------|
| 1. Restate the problem | Say back what you heard in problem language, not feature language | "So the goal is that new users finish setup without emailing support" | The PM corrects or confirms your restatement |
| 2. Surface constraints | State the technical, data, and dependency limits nobody mentioned | "That flow touches the billing service, so any change there is a two-team effort" | Constraints are on the table before solution talk starts |
| 3. Clarify success signals | Ask how we will know it worked | "Is the signal fewer support tickets, or higher setup completion?" | One or two measurable signals are named |
| 4. Explore alternatives | Offer at least one cheaper or different route | "We could do the lightweight version this sprint and validate before building the full flow" | The PM has real options to weigh, not just a yes or no |
| 5. Confirm understanding | Write down the shared understanding where the work lives | Ticket comment or short doc: problem, constraints, signals, chosen scope | Both sides can point at the same written statement |

### The Engineer's Questions in Discovery

| Question Type | What It Uncovers | Example |
|---------------|------------------|---------|
| Volume and scale | Whether the simple solution survives real usage | "How many records will this touch per day, and how fast is that growing?" |
| Failure modes | What the happy-path request hides | "What should the user see when the upstream data is stale?" |
| Data reality | Whether the data needed even exists | "Do we store the attribute this depends on, or is that a new capture problem?" |
| Migration and legacy | Hidden work the request implies | "Does this need to work for accounts created before the new schema?" |
| Reversibility | How expensive a wrong guess will be | "If the experiment fails, can we roll this back without a data migration?" |
| Dependencies | Other teams on the critical path | "This needs the identity team's API — have they committed to a date?" |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Hear the request"] --> B["Restate the problem in your own words"]
    B --> C["Surface constraints and unknowns"]
    C --> D["Agree on success signals"]
    D --> E["Offer alternatives with costs"]
    E --> F["Write down the shared understanding"]
    F --> G["Build and check back at checkpoints"]
```

## In Practice

**Restate the problem before you discuss the solution.** Requests arrive as solutions ("add a dropdown here") wrapped around a problem nobody stated ("users pick the wrong option and file tickets"). Saying "let me check I understand the problem" costs two minutes and routinely reveals that the requested feature is one of several possible fixes.

**Put constraints on the table early, in plain language.** PMs cannot weigh options they cannot see: "this touches the billing service" or "we do not retain that data today" are decision-relevant facts that engineers often hold in their heads. State them before the solution conversation, without jargon and without drama. A constraint surfaced early is planning input; the same constraint surfaced at the end is an excuse.

**Push back with evidence and an alternative, never with mood.** "This feels wrong" cannot be acted on; "the full sync would take six weeks because of the legacy export format, but a one-time import gets us the same user outcome in one week" can. Pair every objection with at least one option that moves the goal forward. You are not vetoing the PM's decision; you are making sure the decision is made with the real cost visible.

**Ask the unglamorous discovery questions.** Volume, failure modes, data availability, migration, reversibility, and dependencies are where estimates go to die. These questions are discovery work, and that work belongs to engineering. Ten minutes of them in a planning conversation saves the sprint-long surprise that follows when the request meets production reality.

**Agree on success signals before building.** If nobody can say how we will know the work succeeded, nobody can say when it is done, and every post-launch debate becomes opinion versus opinion. Push for one or two observable signals (a metric, a support-ticket count, a behavior change) and write them down. After launch, reading those signals together with the PM turns the relationship from handoff into partnership.

**Use tickets as memory, not as a substitute for conversation.** A ticket can hold the agreed problem, constraints, and signals; it cannot hold the reasoning behind them or answer the next question that discovery raises. Hold the conversation, then write down what the conversation established. When the ticket is the only channel, you are building from a document that was stale the moment the last edge case was found.

**Renegotiate openly when reality changes.** If the estimate breaks or a hidden dependency appears, tell the PM immediately with the new information and the options it creates: descope, extend, or simplify. Quietly absorbing the slip protects the relationship for a week and destroys it for a quarter. PMs can manage bad news delivered early, not surprises delivered at the deadline.

## Practical Exercise

Run a full request-translation cycle on a realistic fuzzy request:

1. Take a deliberately vague request (for example, "we need users to be able to export their data"), and write the underlying problem in one sentence without using the word "export."
2. List every constraint you can think of that the request does not mention: data volume, formats, dependencies, privacy, legacy records, failure modes.
3. Draft three clarifying questions in the discovery categories: one about volume, one about failure modes, one about data reality.
4. Propose two alternative solutions at different cost levels, and for each write one sentence on what the user gets and what it costs the team.
5. Define two success signals (one user-facing, one technical) that would tell you the work succeeded.
6. Write a short "shared understanding" comment as if posting it to the ticket: problem, constraints, signals, chosen scope, open questions.
7. Role-play the pushback turn: say the expensive version of the request is out of scope for this cycle, and deliver the message with one alternative attached.

## Practical Applications

- [ ] I restate the problem in my own words before discussing any solution
- [ ] My first contribution to planning is constraints and unknowns, not estimates alone
- [ ] Every pushback I give is paired with at least one concrete alternative
- [ ] I ask discovery questions about volume, failure modes, and data before committing
- [ ] Success signals are agreed and written down before implementation starts
- [ ] Ticket comments capture the shared understanding, not just the acceptance criteria
- [ ] When reality changes, I renegotiate scope or timeline the same day, in the open

Request-translation template:

```text
## Problem (in problem language, not feature language)
<what the user or business is actually struggling with>

## Constraints and unknowns
- <technical or data constraint the request assumes away>
- <dependency, migration, or legacy concern>
- <open question that must be answered before estimating>

## Success signals
- <user-facing signal>
- <technical or operational signal>

## Options
| Option | What the user gets | What it costs | Recommendation |
|--------|--------------------|---------------|----------------|
| A |  |  |  |
| B |  |  |  |

## Agreed scope for this cycle
<what we are building now and what we are explicitly deferring>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Building the literal ticket | The request is a hypothesis about the problem; literal builds solve the wrong thing | Restate the problem and confirm it before estimating |
| Saying yes to everything quietly | Scope grows invisibly until the slip becomes public and trust breaks | Surface trade-offs at request time and renegotiate in the open |
| Pushing back with "it's hard" | The PM has no cost information and reads it as resistance | Name the expensive part and offer a cheaper route to the same outcome |
| Skipping failure-mode questions | Happy-path specs meet unhappy-path reality in production | Ask what happens on stale data, timeouts, and empty states early |
| Treating the ticket as the whole communication channel | Reasoning is lost and every change restarts the negotiation | Hold the conversation, then write down what it established |
| Hiding bad news until the deadline | The PM loses every option except disappointment | Report broken estimates and new dependencies the day you learn them |
| Debating solutions before agreeing on the problem | Both sides argue from different problems and neither convinces the other | Lock the problem statement first, then compare solutions |

## Success Indicators

- Your restatements of fuzzy requests survive the PM's correction pass almost every time
- Planning conversations include your constraint list before anyone quotes a date
- Your pushback is remembered as helpful because it always carried an alternative
- PMs pull you into discovery conversations before the ticket is written
- Post-launch reviews read the same success signals you agreed on up front
- Disagreements with product are about trade-offs, not about each other's motives

## Related

- [[00_overview]]
- [[02_Working_With_Designers]]
- [[06_Writing_For_Technical_Stakeholders]]
- [[career-path/02_Senior_Software_Engineer/02_Problem_Framing_and_Requirements/00_overview|Problem Framing and Requirements]]
- [[career-path/14_Product_Manager/00_overview|Product Manager]]

## Sources

- *The Missing README* (Chris Riccomini, Dmitriy Ryaboy)
- *Crucial Conversations* (Kerry Patterson, Joseph Grenny, Ron McMillan, Al Switzler)
- *The Manager's Path* (Camille Fournier)
