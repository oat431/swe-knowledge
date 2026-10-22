---
title: "Measuring Real Productivity"
role: Software Engineer
capability_area: AI Assisted Engineering
topic: Measuring Real Productivity
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - ai-assisted-development
  - productivity
---

# Measuring Real Productivity

> **Core skill:** Distinguishing real delivery gains from perceived speed

## What Changes from Junior to Mid

A junior engineer feels faster because more code appears on screen per hour, and calls that productivity. A mid-level engineer knows that speed of typing was never the bottleneck: what matters is lead time from idea to reliable production change, including review rounds, defect escape, and rework. AI shifts effort from typing to specifying and verifying, so honest measurement tracks outcomes around the change, not activity during it, and watches for quality debt that arrives later than the speedup.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Signal of success | More code generated, faster | Shorter lead time to reliable production changes |
| What is measured | Keystrokes saved, generation count | Lead time, review rounds, defect escape, rework |
| Time horizon | The hour of generation | The weeks after merge, when defects surface |
| Effort shift | Not noticed | Typing down, specifying and verifying up; accounted for honestly |
| Quality debt | Invisible until it compounds | Tracked as an explicit cost of the speedup |
| Comparison basis | Gut feel | Before/after on comparable tasks |

## Core Frameworks

### Perceived vs Real Signals

| Perceived Signal | Why It Feels True | Real Counterpart |
|------------------|-------------------|------------------|
| Code appears faster | High visible activity | Lead time from ticket start to production deploy |
| Fewer minutes at the keyboard | Typing effort drops | Total effort including specifying, reviewing, fixing |
| First draft is done sooner | Generation compresses drafting | Time to merged, verified change |
| Big diffs look like progress | Volume reads as value | Value delivered per change, complexity avoided |
| "It works on my machine" arrives sooner | Happy path is generated well | Defect escape rate into later review and production |
| Standup sounds productive | Activity is describable | Outcomes are demonstrable in the environment |

### What to Measure Around AI-Assisted Work

| Metric | What It Tells | How AI Can Distort It |
|--------|---------------|----------------------|
| Lead time | Speed of real delivery | Draft finishes early while review and rework stretch the tail |
| Review rounds | How well the change matched expectations first time | Plausible-but-wrong code adds review cycles you did not budget |
| Defect escape | Quality of what shipped | Fluent code passes skims; defects surface in production instead of review |
| Rework volume | How much output had to be redone | Regeneration hides rework inside the generation step |
| Change failure rate | Reliability of the delivery stream | Volume increases while failure proportion stays flat or worsens |
| Total effort per change | Whether time was actually saved | Specifying and verifying time gets excluded from the tally |
| Complexity added | Whether speed came from borrowing against the future | Generated code tends toward more code, not less |

### Designing a Self-Experiment

| Step | Action | Discipline |
|------|--------|------------|
| Baseline | Record lead time, effort, review rounds, and defects for comparable tasks done without assistance | Comparable means similar size, area, and risk — not "any two tickets" |
| Change | Introduce assistance for the same task class, keeping everything else equal | One variable at a time |
| Measure | Track the same metrics, plus verify-and-fix time explicitly | Include the time you spend reviewing generated output |
| Compare | Look at medians over several tasks, not one success | Single samples are stories, not evidence |
| Attribute | Ask what improved: framing, tooling, or task mix | Task mix drift invalidates before/after comparisons |
| Adjust | Keep what moved outcomes; drop what only moved activity | Write the conclusion down; rerun the experiment later |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Record a baseline on comparable tasks"] --> B["Change one variable: add assistance"]
    B --> C["Track lead time, review rounds, defects, rework"]
    C --> D["Include specifying and verifying effort"]
    D --> E["Compare like-for-like outcomes"]
    E --> F["Check for quality debt"]
    F --> G["Keep what helps outcomes; drop what does not"]
    G --> H["Write the conclusion down"]
```

## In Practice

**Measure outcomes, not activity.** Lines generated, tokens used, and time-to-first-draft measure the tool's throughput, not your delivery. The outcomes that matter are lead time to a reliable production change, review rounds to acceptance, and defects that escaped. If a metric cannot be traced to stakeholder-visible delivery, it is an activity metric wearing a costume.

**Count the verification time honestly.** AI shifts effort from typing to specifying, reviewing, and verifying; that work is real work and it often grows with output volume. A change that took 20 minutes to generate and 90 minutes to verify is not a 20-minute change. Tally the full lifecycle or your measurements will argue for more of exactly the wrong thing.

**Compare like with like.** Before/after comparisons only hold when task size, area, and risk are similar; comparing a generated CRUD endpoint to a hand-built concurrency fix tells you nothing about tooling. Track several tasks of the same class and compare medians, since single samples are stories. When the task mix drifts, restart the baseline instead of stretching the comparison.

**Watch the tail, not just the front.** Generated output compresses drafting but can stretch the review-and-fix tail, and quality debt lands weeks later as defects, rework, and maintenance drag. Extend your measurement window past merge: defect escape and change failure rate are where borrowed time gets billed. A speedup that moves work from week one to week four is not a speedup.

**Track rework explicitly, including regeneration.** Regenerating output until it looks right is rework, even when it happens inside a tool and feels like iteration. Count how often output had to be redone, and how much of the original survived; low survival rates mean the framing, not the model, needs investment. Rework that stays invisible is rework you will repeat.

**Beware of quality debt as a silent borrower.** Plausible code with missing edge cases, inflated abstractions, and unexplained complexity ships fast and costs slow. Periodically audit AI-assisted changes a month later: what had to be fixed, explained, or deleted? That audit is your real interest rate on generated speed.

**Write conclusions down and act on them.** An experiment nobody wrote down is an anecdote; a written result ("assistance cut lead time on small migrations, did nothing for design work") becomes a delegation policy. Update what you delegate and how you measure as the tools change. This is measurement in service of judgment, not dashboards in service of comfort.

## Practical Exercise

Run a small controlled self-experiment over one to two weeks:

1. Pick one recurring task class you do regularly: small features, bug fixes, test writing, or migrations.
2. Do two or three tasks of that class without assistance, recording lead time, total effort, review rounds, and any defects found after merge.
3. Define exactly what assistance means for the next batch: which tool, what workflow, what stays human (review, verification, tests).
4. Do three comparable tasks with that assistance, recording the same metrics plus explicit time spent specifying, reviewing, and fixing output.
5. Compare medians for the two batches; list where the time actually moved, front of the cycle or tail.
6. Inspect the quality-debt signal: any defect, rework, or explanation debt from assisted changes discovered after merge.
7. Write a one-paragraph conclusion stating what you will delegate going forward, what you will not, and what you would need to measure next to be surer.

## Practical Applications

- [ ] I have recorded a baseline for at least one task class before claiming AI made me faster
- [ ] My metrics include lead time, review rounds, defect escape, and rework, not lines or generation counts
- [ ] Specifying and verifying time is included in my effort tallies
- [ ] Comparisons are like-for-like on task size, area, and risk, over several samples
- [ ] I inspect AI-assisted changes after merge for defects and complexity debt
- [ ] Regeneration counts as rework in my tracking
- [ ] I have a written conclusion on what to delegate and what to keep
- [ ] I can show my numbers when I claim assistance improved my delivery

Measurement log template:

```text
# Task: <task class and ticket>

## Setup
<with / without assistance; tool and workflow used>

## Metrics
<lead time; total effort incl. verify; review rounds; defects found after merge; rework count>

## Where the effort moved
<typing vs specifying vs verifying vs fixing>

## Post-merge check (after ~2 weeks)
<any defects, rework, or explanation debt traced to this change>

## Note
<one line: what this sample suggests, and what it cannot show>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Measuring lines or keystrokes | Activity rises while outcomes stall | Track lead time, review rounds, defects, rework |
| Excluding verification effort | The real cost hides in the tail | Tally the full lifecycle, verify time included |
| Declaring victory on one fast task | Single samples are stories, not evidence | Compare medians over comparable batches |
| Comparing unlike tasks | Task mix differences swamp tooling effects | Baseline the same task class |
| Ignoring quality debt | Borrowed speed is billed later as defects | Post-merge audits of AI-assisted changes |
| Counting regeneration as free | It is rework wearing an iteration costume | Track how much output survives |
| Optimizing the metric, not the delivery | Teams game what you count | Choose metrics tied to stakeholder-visible outcomes |

## Success Indicators

- You can show before/after numbers for at least one task class, with verification effort included
- Your claims about AI-assisted speed cite lead time and defect data, not feelings
- You know where effort moved in your workflow: typing down, verifying up
- Post-merge audits show no growing defect or rework tail from assisted changes
- You have a written delegation policy derived from your own measurements
- You drop tools or habits that raise activity without improving outcomes

## Related

- [[00_overview]]
- [[02_Reviewing_AI_Generated_Code]]
- [[07_An_AI_Assisted_Daily_Workflow]]
- [[career-path/05_Tech_Lead/06_Process_and_Quality_Stewardship/00_overview|Process and Quality Stewardship]]
- [[career-path/02_Senior_Software_Engineer/05_Quality_Reliability_Security/00_overview|Quality, Reliability and Security]]
- [[SWEBOK v4 - Overview]]

## Sources

- *The Mythical Man-Month* (Fred Brooks)
- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright)
- *The Pragmatic Programmer* (David Thomas, Andrew Hunt)
