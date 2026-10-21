---
title: "Daily Engineering Craft"
role: Software Engineer
capability_area: Daily Engineering Craft
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - craft
  - debugging
  - code-review
  - overview
---

# Daily Engineering Craft

> **Capability area:** The repeatable working practices — reading code, debugging, branching, reviewing, testing, estimating, documenting — that make a software engineer dependable every day.

## What Changes from Junior to Mid

A junior engineer can perform each of these practices when shown. A mid-level engineer performs them habitually, chooses the right intensity for the context (a hotfix and a data migration get different rigor), and teaches them to newer teammates by modeling them.

Judgment at this level shows in:
- Debugging with a hypothesis and a narrowing strategy instead of random toggling
- Writing pull requests that reviewers can approve in one pass
- Estimating small work honestly, including the unknown parts
- Leaving code, tests, and docs clearer than you found them

## Topics in This Area

| # | Topic | Core skill |
|---|-------|------------|
| 01 | [[01_Reading_Unfamiliar_Codebases]] | Building a working mental model of unknown code fast |
| 02 | [[02_Debugging_Method]] | Finding root causes with a hypothesis loop instead of guesswork |
| 03 | [[03_Git_and_Pull_Request_Hygiene]] | Keeping history readable and reviews fast |
| 04 | [[04_Code_Review_Giving_and_Receiving]] | Reviewing for correctness and learning, not approval theater |
| 05 | [[05_Testing_in_Daily_Work]] | Choosing the right test level for the risk in the change |
| 06 | [[06_Small_Task_Estimation]] | Estimating small work with honest ranges and checkpoints |
| 07 | [[07_Documentation_As_You_Go]] | Writing just enough documentation while the context is fresh |

## The Daily Craft Loop

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Read and orient"] --> B["Estimate and slice"]
    B --> C["Implement in small commits"]
    C --> D["Test at the right level"]
    D --> E["Open a reviewable PR"]
    E --> F["Review and integrate"]
    F --> G["Document what surprised you"]
    G --> A
```

## Scope Boundary

This area covers personal working practices inside a single team's flow. It does not cover team-wide process design (owned by [[career-path/05_Tech_Lead/06_Process_and_Quality_Stewardship/00_overview|Process and Quality Stewardship]]), test strategy at organization scale (owned by [[career-path/10_Quality_and_Test_Engineering/00_overview|Quality and Test Engineering]]), or AI-assisted workflows (owned by [[career-path/01_Software_Engineer/03_AI_Assisted_Engineering/00_overview|AI Assisted Engineering]]).

## Key Principles

- Small, reversible changes are the unit of reliable delivery
- Every debugging session starts with a hypothesis and a way to test it
- Review is where team knowledge transfers; treat reviewer time as scarce
- Tests exist to make change safe, not to hit a coverage number
- Documentation written the day you learn something is worth ten times documentation written later

## Common Anti-Patterns

| Anti-Pattern | Why It Fails | Better Approach |
|-------------|-------------|-----------------|
| Debugging by random change | Symptoms move; root cause remains | Hypothesis, experiment, narrow, repeat |
| Giant pull requests | Review quality collapses | Slice by intent; one concern per PR |
| Estimating in hours with false precision | Erosion of trust on the first slip | Ranges with confidence levels and checkpoints |
| Tests only for the happy path | Regressions hide in edge cases | Test the risk, not the line count |
| Docs as an afterthought before release | Context already forgotten | Note decisions and surprises as they happen |

## Maturity Signals

- You can orient in an unfamiliar service and make a safe small change within a day or two
- Your bug reports include a reproduction and a hypothesis, not just a screenshot
- Reviewers comment on design, not on formatting or missing tests you should have written
- Your estimates include unknowns explicitly and you flag slippage early
- New team members learn the system from docs you wrote

## Connections

- [[career-path/01_Software_Engineer/01_Leveling_and_Growth/00_overview|Leveling and Growth]]: how these habits map to level expectations
- [[career-path/02_Senior_Software_Engineer/01_Technical_Ownership/00_overview|Technical Ownership]]: where these habits scale up to system ownership
- [[software-engineering-note/04_Software_Construction/Software Construction Overview]]: construction fundamentals
- [[software-engineering-note/05_Software_Testing/Software Testing Overview]]: testing fundamentals

## Sources

- *The Pragmatic Programmer* (David Thomas, Andrew Hunt) — daily craft heuristics
- *Working Effectively with Legacy Code* (Michael Feathers) — reading and changing unfamiliar code
- *Debugging: The 9 Indispensable Rules for Finding Anything* (David Agans) — systematic debugging
- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright) — review, testing, and estimation practices
