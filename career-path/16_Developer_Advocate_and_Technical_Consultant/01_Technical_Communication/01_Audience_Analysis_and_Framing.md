---
title: Audience Analysis and Framing
role: Developer Advocate and Technical Consultant
capability_area: Technical Communication
topic: Audience Analysis and Framing
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - developer-advocate
  - technical-communication
  - audience-analysis
  - framing
---

# Audience Analysis and Framing

> **Core skill:** The advocate reads an audience before speaking — naming who they are, what they must do afterwards, and what they already know — then chooses the depth, vocabulary, and frame that meets them at their altitude.

## Why This Matters

Most failed technical communication fails at the audience step, not the technical step. Content written for "developers" as one undifferentiated mass misses the beginner who does not know the vocabulary and irritates the expert who is forced through a primer. The same API can be framed as a way to ship faster for an application team, as a reliability boundary for operators, and as a cost line for a finance owner. The technical truth does not change; the entry point does.

Audience analysis is not flattery — it is deciding what the person must be able to do after consuming the artifact. A talk that ends with an executive unable to make a funding decision was entertainment, not communication. A tutorial that ends with a reader unable to run the code was a demo, not education. Deciding the ending first — the decision, the task, the capability — is what makes every step before it purposeful instead of ornamental.

The most common professional failure is the curse of knowledge: experts cannot remember not knowing. The second is assuming one audience for one artifact, when most artifacts drift across audiences — an executive reads the first paragraph of a blog post, an engineer reads the rest; a conference talk reaches practitioners in the room and decision makers watching the recording months later. Framing layers exist so every reader finds an entry point without the others being served noise.

## Audience Dimensions

| Dimension | Questions to Ask | What It Changes |
|-----------|------------------|-----------------|
| Prior knowledge | Which concepts, tools, and vocabulary are already familiar | Definitions needed; where to start |
| Goal after consuming | What decision or task follows this artifact | The ending; the call to action |
| Altitude | Concept, capability, interface, or internals | How deep to begin and how far to go |
| Constraints | Time, budget, compliance, team size, legacy stack | Which trade-offs matter to them |
| Channel and habit | Docs, talk, video, chat, email, social, search | Length, structure, and format |
| Disposition | Skeptical, enthusiastic, mandated, evaluating | Tone and burden of proof |

## The Four Altitudes

| Altitude | Audience Question | Artifact That Serves It |
|----------|-------------------|-------------------------|
| Concept | Why does this exist, and why now | Narrative, keynote framing, announcement |
| Capability | What can it do for me | Use cases, outcomes, short demos |
| Interface | How do I use it | Guides, tutorials, example code, signatures |
| Internals | How does it actually work | Design docs, source reading, deep dives |

Different audiences enter at different altitudes. The craft is meeting each audience at its entry altitude and then walking down — never opening at internals for a reader who has not yet accepted the concept, and never stranding a practitioner at concept level when they came to build.

## Framing Techniques

| Technique | When It Fits | Shape of the Opening |
|-----------|--------------|----------------------|
| Analogy | The concept is unfamiliar but the experience is not | Map the new thing onto a familiar mechanism |
| Contrast | The audience already knows a competing approach | State the trade being made against what they know |
| Concrete first | Engineers and skeptics | Show working code, then explain why it works |
| Stakes framing | Decision makers | State what breaks, costs, or is missed by inaction |
| Progressive disclosure | Mixed audiences | TL;DR, then layers that each stand alone |
| Problem framing | Everyone | Open with the pain before the solution is named |

## Reading a Room in Live Settings

| Signal | Likely Meaning | Adjustment |
|--------|----------------|------------|
| Silence after a claim | The claim was not understood or not believed | Offer the concrete example now |
| Questions about vocabulary | Wrong altitude — too deep too early | Step back one altitude and rebuild |
| Questions outside the scope | The audience cares about a different problem | Acknowledge; park; follow up directly |
| Rising engagement at internals | Expectation met at concept; appetite for depth | Slow down and go one layer deeper |
| No question about the ask | The call to action was unclear or unmotivated | Restate the ask with its stakes |

## The Audience Loop

```mermaid
flowchart LR
    WHO["Who is in the audience"] --> GOAL["What must they do afterwards"]
    GOAL["What must they do afterwards"] --> ALTITUDE["Choose the altitude and the frame"]
    ALTITUDE["Choose the altitude and the frame"] --> ARTIFACT["Produce the artifact or talk"]
    ARTIFACT["Produce the artifact or talk"] --> REACTION["Read the reaction and adjust"]
```

The loop closes in every live setting: a confused face, a question at the wrong altitude, or silence after an ask is data. Framing is not decided once — it is confirmed in the room and corrected in the next artifact.

## Practical Applications

### Audience Brief Checklist

- [ ] The primary audiences are named with their current knowledge and their goal
- [ ] One altitude is primary; deeper layers are optional rather than required
- [ ] Vocabulary matches the audience — jargon is defined on first use, neither dodged nor assumed
- [ ] Trade-offs are expressed in the audience's constraints, not the advocate's
- [ ] The artifact ends in a specific next action for each audience segment
- [ ] The framing was tested on at least one representative reader or listener

### Audience Brief Template

```markdown
## Audience Brief — <artifact or talk>

| Audience | Knows Already | Must Be Able To | Altitude | Next Action |
|----------|---------------|-----------------|----------|-------------|
| <group>  | <concepts>    | <task or decision> | <level> | <verb>   |
```

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Audience of one abstraction** | "Developers" hides beginners and experts who need opposite things | Name two or three concrete sub-audiences and their altitudes |
| **Curse of knowledge** | Experts skip the steps beginners need; beginners leave silently | Test with a real beginner; define terms on first use |
| **Framing as spin** | Emphasizing what pleases slides into hiding what matters | Frame emphasis honestly; never omit a material trade-off |
| **Single-altitude artifact** | Too deep for decision makers, too shallow for practitioners | Layer the artifact — summary, then depth for the committed |
| **Assumed channel fit** | A forty-minute video does not serve someone on a phone between meetings | Match format to how the audience actually consumes |
| **One-shot audience model** | Assumptions set at writing time are never corrected | Treat live reactions as data; revise the framing |

## Success Indicators

- Different roles each find their entry point in the same artifact
- Follow-up questions arrive at the audience's altitude, not as requests to re-explain basics
- Decision makers act in the session; practitioners build the same week
- Content tested with a representative beginner needs no rescue narration
- The advocate can state, in one sentence per audience, what that audience must do next

## Related Topics

- [[03_Explaining_Complex_Concepts]]
- [[04_Presenting_and_Public_Speaking]]
- [[04_Customer_and_Developer_Understanding/00_overview|Customer and Developer Understanding]]
- [[career-path/02_Senior_Software_Engineer/06_Communication_and_Influence/00_overview|Communication and Influence (Senior)]]

## Summary

Audience analysis and framing decide everything that follows: who the reader is, what they already know, what they must be able to do afterwards, and which altitude they enter at. The advocate who names those things before producing the artifact — and tests the framing on real readers — lands at the right depth without diluting the truth, because the depth was chosen deliberately rather than inherited from the author's own expertise.
