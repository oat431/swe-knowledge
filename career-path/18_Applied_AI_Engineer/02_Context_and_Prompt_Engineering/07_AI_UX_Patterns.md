---
title: "AI UX Patterns"
note_type: capability-topic
capability_area: context-and-prompt-engineering
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
created: 2026-10-21
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - ai-ux
  - uncertainty-communication
---

# AI UX Patterns

> Designing the user experience around probabilistic systems: streaming, uncertainty display, review flows, and graceful failure.

## Why This Is a Senior Skill

A mid-level engineer wraps the model call in a chat box, ships whatever comes back, and treats the UI as a thin shell around a prompt. A senior engineer knows that with a probabilistic system the interface is part of the correctness story: how partial output is streamed shapes what users believe, how uncertainty is displayed determines whether they verify or blindly trust, and what happens on failure decides whether the product is seen as careful or broken. The model's quality sets the ceiling; the UX patterns decide how much of that quality users actually receive safely.

The senior question is not "is the answer good?" but "does this interface make the system's real reliability legible, so users know when to trust it, how to correct it, and what happens when it fails?"

## Core Frameworks

### Uncertainty Communication

| Signal Available | Honest Display | What It Prevents | What It Costs |
|------------------|----------------|------------------|---------------|
| Model logprobs or confidence | Confidence bands, low-confidence badges | Overtrust on shaky answers | Rarely exposed by hosted APIs; bands are approximate |
| Retrieval evidence | Inline citations with source regions | Trusting claims with no grounding | Clutter; requires grounding pipeline |
| Known scope limits | "This is outside my coverage" states | Users extending the tool to unsupported domains | Requires a defined scope taxonomy |
| Disagreement between passes | Show alternatives when checks disagree | False precision on genuinely ambiguous questions | Extra inference cost per answer |
| Historical accuracy per category | Per-category reliability notes | Uniform trust across uneven quality slices | Requires maintained eval data |

### Streaming and Perceived Latency

| Pattern | When To Use | Design Notes |
|---------|-------------|--------------|
| Token streaming | Open-ended text generation | Show text as it generates; never reflow or reorder after display |
| Skeleton then fill | Structured output (tables, forms, JSON) | Render the structure first, fill cells as they stream; avoids layout jumps |
| Stage progress | Multi-step pipelines (retrieve, reason, verify) | Named stages with completion states beat one spinner for long waits |
| Partial transcript | Voice and long-document ingestion | Show intermediate artifacts (transcript, extracted sections) while work continues |
| Buffered commit | High-stakes output (contracts, code diffs) | Stream privately; show committed output once verification passes |

### Review, Correction, and Failure UX

| UX Surface | What the User Sees | Senior Design Decisions |
|------------|--------------------|-------------------------|
| Review queue | Item, model draft, evidence, one accept/edit/reject action | Show evidence side by side; batch by task type; never ask reviewers to grade without a rubric |
| Edit affordance | Model output with in-place editing | Corrections feed back as eval cases; track what was edited, not just that it was |
| Regenerate | One click, with variation control | Bound retries; a regenerate loop is a cost and trust leak if unbounded |
| Feedback signals | Thumbs up/down plus optional reason | Reasons are data; bare votes are noise; route reasons to eval curation |
| Fallback message | Clear state: what failed, what the system did instead | Never a silent wrong answer; never a raw stack trace; name the next action |
| Citation view | Claim-to-source links the user can open | Citations must resolve to real regions or they are decoration |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Model Output Stream"] --> B["Streaming Display"]
    B --> C["Uncertainty Signals"]
    C --> D["Verification Check"]
    D --> E["Committed Output"]
    E --> F["Correction Affordances"]
    F --> G["Feedback Capture"]
    G --> H["Eval Data Flywheel"]
    H --> A
```

## In Practice

**Stream partial output to manage perceived latency, but commit deliberately.** Token streaming changes waiting into reading and is the cheapest latency win available for generative features. For structured or high-stakes output, buffer until a verification step passes, then render committed results; a streamed answer the system later retracts destroys more trust than a slower honest one.

**Communicate uncertainty honestly and specifically, not with noise.** A vague "I might be wrong" hedge on every answer teaches users to ignore disclaimers. Attach uncertainty where the signal exists: a citation on grounded claims, an explicit low-confidence state where checks disagree, a scope note on covered topics. The goal is calibrated trust per claim type, not blanket doubt.

**Design review queues around the reviewer's judgment, not the model's output volume.** Reviewers see the draft, the evidence, and the rubric in one view; batching by task type keeps the rubric stable in their head, and a well-designed queue makes the accept/edit/reject decision a few seconds of genuine judgment. If reviewers rubber-stamp, the queue's data is worthless and the review theater is worse than none.

**Make every failure state name what happened and what happens next.** Graceful failure has three parts: a plain-language state (time out, out of scope, low confidence), the degraded behavior actually delivered (partial answer, cached answer, human handoff), and the user's next action. Users forgive failures they can see and route around; they do not forgive confident wrong answers that a visible failure state would have prevented.

**Build correction affordances and let them pay rent in evaluation data.** Edit, regenerate, thumbs, and citation clicks are the cheapest feedback signals a product can collect, but only if they are captured with context and routed into eval curation (see [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/05_Online_Evaluation_and_Monitoring|Online Evaluation and Monitoring]]). Track what users changed about a draft, not merely that they clicked; the diff is the signal.

**Frame the system as a tool, not a person.** Anthropomorphic framing ("I'm thinking", "I'm sorry", persona avatars) invites trust the system has not earned and muddies responsibility when it fails. Use capability language ("analyzing the document", "this answer is unverified") so users attribute results to a system with known limits, which is also the framing that survives scrutiny from legal and compliance.

**Design AI features for accessibility from the start.** Streaming text must be announced coherently to screen readers (live regions that do not spam), uncertainty states need non-color cues, voice features need caption fallbacks, and correction flows must be keyboard-operable. Accessibility fixes retrofitted onto probabilistic UI are the most expensive kind, because the interaction model itself changes.

## Practical Exercise

Design the UX for an AI drafting assistant that writes contract clauses for legal review:
1. Define what streams, what buffers, and what is committed after verification, per output type
2. Write the uncertainty display for three cases: well-grounded clause, clause with no source support, scope question the tool does not cover
3. Specify the review queue view: what the reviewer sees alongside the draft, how items are batched, and the accept/edit/reject actions
4. Design the correction loop: what an edit captures, where regenerate is allowed, and how thumbs feedback is stored
5. Write the fallback states for timeout, provider outage, and low-confidence output, each with a next action
6. List three accessibility requirements specific to streaming and uncertainty display
7. Name the trust-framing rules: what the assistant never says about itself

## Practical Applications

- [ ] Streaming behavior is defined per output type (stream vs buffer vs commit)
- [ ] Uncertainty appears only where a real signal exists, and is specific per claim
- [ ] Review queue shows draft, evidence, and rubric in one view with fast accept/edit/reject
- [ ] Every failure state names what failed, what was delivered instead, and the next action
- [ ] Correction affordances (edit, regenerate, feedback) are captured with context into eval data
- [ ] System framing avoids anthropomorphic claims of understanding or intent
- [ ] Streaming, uncertainty, and correction flows pass keyboard and screen-reader checks

```
Fallback message template:
State: <what failed, in plain language>
Delivered: <what the user has instead: partial answer, cached answer, human handoff>
Next: <the one action available to the user>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Hedging every answer with the same disclaimer | Users learn to ignore uniform noise; real risk signals disappear | Attach uncertainty to specific claims where a signal exists |
| Streaming then visibly revising output | Retracted text users already read destroys trust faster than delay | Buffer high-stakes output until verification passes |
| Review queues without evidence or a rubric | Reviewers rubber-stamp; the collected labels are noise | Side-by-side evidence and a written rubric per batch |
| Silent fallback to a worse model or cached answer | Users believe the degraded output came from the full system | Name the degraded mode in the response state |
| Buttons for thumbs up/down with no capture pipeline | Feedback theater: data collected but never reaches eval curation | Route every signal, with context, into the eval flywheel |
| Persona avatars and "I think / I feel" framing | Invites overtrust and blurs accountability when output fails | Capability language; the system analyzes, it does not believe |
| Color-only confidence cues | Excludes color-blind users and screen-reader users entirely | Pair every state with text or icon semantics |

## Knowledge Connections

- [[03_Context_Window_Management]]: what the model can hold shapes what the UX can promise
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/05_Online_Evaluation_and_Monitoring|Online Evaluation and Monitoring]]: correction and feedback signals as production telemetry
- [[career-path/18_Applied_AI_Engineer/07_Agentic_Systems/00_overview|Agentic Systems]]: progress and interruption UX for multi-step agents
- [[career-path/01_Software_Engineer/03_AI_Assisted_Engineering/00_overview|AI Assisted Engineering]]: review-flow patterns for AI-assisted work
- [[computing-foundation-note/Artificial_Intelligence/AI Overview|AI Overview]]: probabilistic-system behavior users must be taught to read

## Sources

- The Design of Everyday Things (Don Norman)
- Don't Make Me Think (Steve Krug)
- AI Engineering (Chip Huyen)
- Designing Machine Learning Systems (Chip Huyen)
