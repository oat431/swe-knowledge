---
title: "Working With Designers"
role: Software Engineer
capability_area: Collaboration and Teamwork
topic: Working With Designers
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - collaboration
  - design
---

# Working With Designers

> **Core skill:** Collaborating on what is desirable and what is feasible

## What Changes from Junior to Mid

A junior engineer waits for the final mockups, then discovers half the design is expensive to build. A mid-level engineer talks to design while the design is still cheap to change: names which interactions are one line of code and which are a week of state management, asks "can we make it do X" as an exploration instead of a threat, and treats empty states and accessibility as design content rather than engineering cleanup. Feasibility becomes an input to design, not a verdict on it.

| Aspect | Junior | Mid |
|--------|--------|-----|
| When design hears from you | At handoff, when the design is finished | During exploration, while nothing is expensive yet |
| Feasibility feedback | "This is impossible" at the end | "This part is cheap, this part is costly" while options are open |
| "Can it do X?" questions | Treats them as scope creep | Explores them and returns with cost and a variant |
| Vocabulary | Talks in components and CSS | Talks in user goals, flows, states, and edge cases |
| Empty and error states | Adds them as engineering afterthoughts | Raises them in design review as shared scope |
| Handoff artifacts | Reads only the happy-path screen | Extracts states, behaviors, and acceptance details from the whole flow |

## Core Frameworks

### Shared Vocabulary

| Term | Designer Means | Engineer Translates To | Why It Matters |
|------|----------------|------------------------|----------------|
| User goal | What the person is trying to accomplish | The job the feature must complete end to end | Keeps both sides optimizing the same outcome when the how gets hard |
| Flow | The ordered path through screens or steps | State transitions and the events that trigger them | Flows are state machines; naming that early exposes missing branches |
| State | What the interface shows at a given moment | Data availability, loading, error, and permission conditions | Every screen has more states than the mockup shows |
| Edge case | Unusual but real user situations | Boundary conditions: empty, partial, stale, conflicting data | Edge cases are where design and code both quietly fail |
| Component | A reusable design pattern | A code module with inputs, variants, and limits | Component vocabulary makes effort discussions concrete |
| Interaction | What the user does and what responds | Event handling, optimistic updates, latency tolerance | Interaction ambition is where cost hides |

### Feasibility Feedback Timing

| Signal Type | What It Says | When to Give It | Cost of Delaying |
|-------------|--------------|-----------------|------------------|
| Cheap signal | "This is a standard pattern; we have the pieces" | At the first sketch | None — it lets design move fast with confidence |
| Expensive signal | "This interaction needs new infrastructure or heavy state work" | During exploration, before the flow is polished | Polished designs anchor the team; late cost news looks like refusal |
| Constrained signal | "Possible, but only for the data we already retain" | When the design depends on data or permissions | The design becomes unbuildable after it is sold to stakeholders |
| Alternative signal | "That is costly; here is a variant with the same user effect" | Same conversation as the expensive signal | Without a variant, cost feedback has no constructive path |
| Unknown signal | "I need a day to price this properly" | Immediately, with a committed follow-up time | Silence is read as consent, then the estimate lands at the worst moment |

### Handoff Artifact Checklist

| Artifact | What It Must Contain | Who Fills Gaps |
|----------|----------------------|----------------|
| Flow map | Ordered steps, entry points, exits, and loop-backs | Designer leads; engineer confirms transitions are representable |
| Screen states | Default, loading, empty, error, permission-denied, partial data | Shared — engineer enumerates states design may not have seen |
| Behavior spec | What happens on each interaction, including latency and failure | Designer defines intent; engineer defines what is achievable |
| Content and edge cases | Empty-state copy, error copy, overflow behavior, long values | Designer; engineer flags truncation and format limits |
| Responsive and platform notes | Breakpoints, touch targets, keyboard paths | Shared |
| Acceptance details | What "done" looks like for the interaction | Shared, written where the ticket lives |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Join while the design is still a sketch"] --> B["Name cheap and expensive parts"]
    B --> C["Return variants for costly interactions"]
    C --> D["Enumerate states and edge cases together"]
    D --> E["Agree on handoff artifacts"]
    E --> F["Build with checkpoint reviews"]
    F --> G["Verify accessibility and states in the real build"]
```

## In Practice

**Give feasibility feedback while the design is still cheap to change.** A sketch costs an hour; a polished, approved design costs a week of design time and the political weight of stakeholder sign-off. Telling a designer "the drag interaction needs real-time sync we do not have" during exploration is collaboration; the same sentence after approval is a demolition notice. Ask to see work in progress, and respond to rough shapes with rough cost signals.

**Speak in user goals, flows, and states, not just components.** When your shared language is only buttons and breakpoints, every conversation stalls at the surface. Restating "the goal is that the user trusts the save happened" moves the discussion to something both of you can solve: maybe a persistent indicator instead of the animation. Flows are state machines and screens have more states than any mockup; making that explicit is one of engineering's gifts to the design conversation.

**Answer "can we make it do X" with exploration, not reflex.** The question is a designer thinking out loud, not a change order. Your answer has three parts: what is possible, what it costs, and what a cheaper variant achieves. Even when the answer is no in its literal form, returning with a variant keeps you on the same side of the table: both of you are now solving the cost problem together.

**Treat empty, error, and loading states as shared design content.** These states are where users suffer and where mockups are thinnest, and no amount of clean code fixes an unthought-out failure screen. Raise them in design review by name: "what does the user see when the list is empty, when the request fails, and when the data is stale?" If design has not drawn them, offer to pair on the wording while you build the logic; the states belong to both of you.

**Price interaction ambition honestly and early.** Optimistic updates, offline behavior, real-time sync, and intricate animations are the usual hiding places for cost. You do not need exact numbers to be useful: "that is a day" versus "that is a week of state work" is enough signal for a design decision. Numbers that turn out wrong are fine when they arrive early and are revised openly; vague silence is not.

**Extract the full behavior spec from handoff artifacts.** Happy-path screens are the smallest part of the design. What you actually need is the flow map, every screen state, interaction behavior under latency and failure, and the acceptance details. Walk the artifacts once with the designer and write the gaps down; a twenty-minute gap review is cheaper than a week of building the wrong behavior and screenshotting it back and forth.

**Make accessibility and responsive behavior joint decisions.** Keyboard paths, contrast, touch targets, and motion sensitivity shape what design can draw and what code must do. Raise them as design constraints during exploration instead of compliance findings after build. When both sides treat them as scope, they get designed; when only engineering treats them as scope, they get skipped.

## Practical Exercise

Run a design collaboration cycle on a small feature:

1. Take a single-screen feature idea (for example, "let users reorder items in a list"), and write the user goal in one sentence and the flow as an ordered list of transitions.
2. Enumerate every screen state the flow touches: default, loading, empty, error, stale data, permission-limited; note which ones a typical mockup would omit.
3. Classify each proposed interaction as a cheap, expensive, constrained, or unknown feasibility signal, and write the one-line cost message you would say out loud for each.
4. Pick the most expensive interaction and design two cheaper variants that preserve the user goal; write one sentence each on what the user loses and gains.
5. Run a mock "can we make it do X?" conversation where the request is ambitious (say, live multi-user reordering), and answer with possibility, cost, and variant.
6. Fill in the handoff artifact checklist for this feature as if preparing it for a teammate to build.
7. Write a short design review comment raising accessibility and edge states as scope, phrased as collaboration, not criticism.

## Practical Applications

- [ ] I ask to see designs in the sketch stage and give cost signals there
- [ ] My feasibility feedback always names cheap and expensive parts, never just "no"
- [ ] Every expensive-signal message carries at least one variant
- [ ] I enumerate loading, empty, and error states during design review, not after build
- [ ] I use shared vocabulary (goal, flow, state, edge case) in design conversations
- [ ] Handoff artifacts include behavior under latency and failure, not just happy-path screens
- [ ] Accessibility and keyboard paths are discussed as design constraints before implementation

Feasibility response template:

```text
## What is possible
<what the design already supports with existing pieces>

## What it costs
- <cheap part>: <rough effort>
- <expensive part>: <rough effort and why — state, sync, data, permissions>

## Variants for the expensive part
<variant 1: what the user gets and what it saves>
<variant 2: what the user gets and what it saves>

## Open question I need to price
<what I will investigate and by when>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Reviewing designs only at handoff | Cost lands after the design is anchored and approved | Ask for work-in-progress and respond to sketches |
| Answering "can it do X?" with a flat no | Reads as obstruction and pushes design around you | Return with possibility, cost, and a cheaper variant |
| Building only the happy path | Empty and error states become user-facing surprises | Enumerate states in review and pair on their content |
| Speaking only in implementation terms | Design cannot map "use a state machine" to a user outcome | Translate to goals, flows, and states first |
| Quietly accepting unpriced interaction ambition | Scope and schedule erode invisibly | Price ambition early, even roughly, and revise openly |
| Treating accessibility as post-build compliance | It gets cut under pressure and users pay for it | Raise keyboard, contrast, and motion as design constraints |
| Screenshot-driven back-and-forth loops | Each round rebuilds shared context from zero | Hold a twenty-minute gap review over the handoff artifacts |

## Success Indicators

- Designers tag you into exploration threads before the flow is polished
- Your cost signals arrive while options are still open, and they carry variants
- Empty and error states appear in designs because you raised them early
- "Can we make it do X?" conversations end with a shared option, not a verdict
- Handoff artifacts are complete enough that you build without screenshot archaeology
- Accessibility questions stop arriving as surprises from outside the team

## Related

- [[00_overview]]
- [[01_Working_With_Product_Managers]]
- [[07_Handoffs_and_Knowledge_Sharing]]
- [[career-path/02_Senior_Software_Engineer/02_Problem_Framing_and_Requirements/00_overview|Problem Framing and Requirements]]
- [[software-engineering-note/14_Software_Engineering_Professional_Practice/Professionalism of Software Engineering Overview]]

## Sources

- *The Missing README* (Chris Riccomini, Dmitriy Ryaboy)
- *Nonviolent Communication* (Marshall B. Rosenberg)
- *Debugging Teams* (Brian Fitzpatrick, Ben Collins-Sussman)
