---
title: "Feedback and Self Assessment"
role: Software Engineer
capability_area: Leveling and Growth
topic: Feedback and Self Assessment
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - leveling
  - feedback
---

# Feedback and Self Assessment

> **Core skill:** Rating yourself honestly and turning feedback into a plan

## What Changes from Junior to Mid

A junior engineer receives feedback as a verdict and either deflates or defends; a mid-level engineer treats feedback as data about behavior in context, checks it against their own read, and converts it into a change plan. The paired skill is self-assessment: rating yourself against the same five dimensions your manager uses, with evidence, so calibration conversations are comparisons of two honest reads rather than negotiations.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Feedback requests | "How am I doing?" | "What should I stop doing in code review?" |
| Receiving feedback | Personal, defensive or deflated | Curious: probes for the situation behind it |
| Self-image | Hope and anxiety | Evidence-backed ratings per dimension |
| "Not yet" | Reads as rejection | Reads as a named gap with a target |
| Rhythm | Annual review as the only mirror | Quarterly self-review plus monthly asks |

## Core Frameworks

### Good Feedback Asks vs Bad Feedback Asks

| Bad Ask | Why It Fails | Good Ask | Why It Works |
|---------|--------------|----------|--------------|
| "Any feedback for me?" | Invites nothing or vague praise | "What is one thing I should stop doing in review comments?" | Specific, answerable, about behavior |
| "Am I doing well?" | Puts the reviewer on the spot to reassure | "Where did my work last month create work for others?" | Names a concrete window and effect |
| "Why don't I get promoted?" | Invites defensive explanation | "Which anchor at the next level am I furthest from?" | Points at the ladder, not at you |
| "Did you like my design?" | Asks for approval, not information | "What is the weakest part of this design, and what would you change first?" | Requests judgment on the artifact |
| Asking only friends | Selection bias toward reassurance | Asking the person who reviews your code most | Highest signal per ask |

### SBI: Situation-Behavior-Impact

| Component | What It Names | Example (receiving) | Example (giving) |
|-----------|---------------|---------------------|------------------|
| Situation | When and where it happened | "In yesterday's design review" | "In yesterday's design review" |
| Behavior | The observable action, not the character | "When I presented the schema, you cut in twice before I finished" | "You interrupted the presenter twice" |
| Impact | The effect on work or people | "I stopped sharing half-built ideas there" | "The team held back questions afterward" |

### Self-Rating Against the Five Dimensions

| Dimension | Evidence To Gather | Question That Exposes The Gap |
|-----------|--------------------|-------------------------------|
| Scope | Size of what you carried end to end | "Could I take the next size up without help?" |
| Autonomy | Decisions made without supervision | "Which calls would my manager say I should have made alone?" |
| Ambiguity | Vague inputs you turned into plans | "When did I last reduce uncertainty before building?" |
| Impact | Outcomes verified, not just shipped | "Which of my changes can I show moved something observable?" |
| Communication | Artifacts and risk flags, dated | "Would anyone know my state if I went silent for a week?" |

### Handling "Not Yet"

| Move | What It Sounds Like | What To Do Next |
|------|--------------------|-----------------|
| Clarify the gap | "Which specific behavior is missing?" | Get it named in ladder language, written down |
| Test your read | "My evidence says X; where does that fall short?" | Compare evidence, not impressions |
| Agree the target | "What would demonstration look like?" | Define one observable action and a timeframe |
| Shrink the ask | "What can I take on to practice that?" | Request the specific scope that exercises the gap |
| Set the review | "When do we check this again?" | Book the date before leaving the conversation |

### Quarterly Self-Review Rhythm

| Cadence | Activity | Output |
|---------|----------|--------|
| Weekly | Note wins, decisions, feedback in the brag document | Dated entries |
| Monthly | Ask one person one specific feedback question | One correction or confirmation |
| Quarterly | Self-rate the five dimensions with evidence | Completed self-assessment table |
| Quarterly | Calibration conversation with your manager | Agreed gaps and targets for next quarter |
| Annually | Mine the year's notes into the formal review | Review draft, not a memory exercise |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Ask one specific feedback question"] --> B["Receive it with SBI probing"]
    B --> C["Check it against your evidence"]
    C --> D["Convert it into one behavior change"]
    D --> E["Practice it and log instances"]
    E --> F["Quarterly self-rate the five dimensions"]
    F --> G["Calibrate with your manager"]
    G --> A
```

## In Practice

**Ask for feedback in a form that is safe and answerable to give.** Specific, behavior-level questions about a recent window get real answers; global character questions get comforting ones. Ask the person who observes the behavior most, not the person who likes you most. One good ask per month beats a yearly "tell me everything."

**Receive with SBI before you react.** When feedback arrives, probe for its components: in what situation, what exactly did I do, and what was the impact. This separates the data from the delivery, lowers the temperature, and often reveals that the feedback is narrower (and more fixable) than it first sounded. Thank first, analyze later, respond never in the moment.

**Check feedback against your own evidence instead of accepting or rejecting it wholesale.** Your job is to triangulate: the feedback, your artifact trail, and one more view. Sometimes the feedback is right and your self-image is stale; sometimes it is one person's context and your record says otherwise. Either way the output is the same: a written note on what you believe and why.

**Self-rate with evidence per dimension or you are negotiating with feelings.** A rating without a dated example is a mood. Before each calibration, pull one artifact per dimension from your notes: the scope you carried, the decision you made alone, the ambiguity you reduced, the outcome you verified, the update you wrote. The conversation then compares two evidence sets, which managers can work with.

**Treat "not yet" as a named gap with a target date.** The phrase is information: a specific behavior is missing, and someone believes it is reachable. Get it written in ladder language, agree what demonstration looks like, and book the next check. Engineers who do this convert "not yet" into promotions; engineers who hear it as a verdict look for the exit instead.

**Run the quarterly rhythm even when nobody asks for it.** Reviews are assembled from whatever is remembered; memory is recent and loud. The weekly note habit and quarterly self-review mean your review writes itself from dated evidence. When the official cycle arrives, you are summarizing, not excavating.

## Practical Exercise

Run one full feedback cycle this month:
1. Pick the behavior you most want to calibrate: one of the five dimensions
2. Write one specific feedback question using the good-ask table
3. Ask the person who sees that behavior most often; write down the answer verbatim
4. Reflect it back in SBI form and ask whether you captured it correctly
5. Compare it to your own evidence for that dimension and note where they diverge
6. Agree one behavior change and what demonstration would look like
7. Schedule the quarterly self-review in your calendar before you finish

## Practical Applications

- [ ] Ask one specific feedback question every month and record the answers
- [ ] Keep the self-rating table filled with a dated example per dimension
- [ ] Probe every piece of feedback for situation, behavior, and impact before reacting
- [ ] Convert every "not yet" into a written gap, target demonstration, and check-in date
- [ ] Hold a quarterly calibration conversation even in quiet quarters
- [ ] Save verbatim quotes in your brag document; paraphrase loses the evidence

Quarterly self-review template:

```text
Quarter:
Dimension | My rating | Evidence (dated) | Feedback I heard | Gap for next quarter
Scope     |           |                  |                  |
Autonomy  |           |                  |                  |
Ambiguity |           |                  |                  |
Impact    |           |                  |                  |
Comms     |           |                  |                  |

Strongest signal this quarter:
Behavior I will change next quarter:
What I want more feedback on:
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| "Any feedback?" as your only ask | Invites empty reassurance and trains silence | Ask one specific behavior question at a time |
| Defending yourself in the moment | Turns data into a debate and closes the tap | Thank, probe with SBI, respond later with evidence |
| Accepting feedback as verdict | Swallows either confidence or correction | Triangulate with your artifacts and a second view |
| Rating yourself on intentions | Reviews calibrate on observed behavior | Rate with dated examples only |
| Treating "not yet" as rejection | Loses the specific gap inside the phrase | Extract the gap, define demonstration, book the check |
| Only reviewing at annual time | The story is written from the last few weeks | Weekly notes plus quarterly self-review |

## Success Indicators

- You can name your two current gaps in ladder language without prompting
- People give you specific, behavior-level feedback because they know it lands
- Your self-rating and your manager's rating differ by at most one notch per dimension
- "Not yet" conversations end with a written target and a booked date
- Your annual review draft takes an afternoon, built from dated notes

## Related

- [[02_Junior_vs_Mid_Level_Expectations]]
- [[05_Impact_and_Visibility]]
- [[07_The_Brag_Document]]
- [[career-path/02_Senior_Software_Engineer/09_Promotion_Evidence_and_Capstone/04_Self_Assessment||Self Assessment]]

## Sources

- *Thanks for the Feedback* (Douglas Stone, Sheila Heen)
- *Radical Candor* (Kim Scott)
- *The Missing README* (Chris Riccomini, Dmitriy Ryaboy)
