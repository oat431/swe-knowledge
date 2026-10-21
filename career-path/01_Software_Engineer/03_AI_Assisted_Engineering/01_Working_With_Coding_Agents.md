---
title: "Working With Coding Agents"
role: Software Engineer
capability_area: AI Assisted Engineering
topic: Working With Coding Agents
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - ai-assisted-development
  - coding-agents
---

# Working With Coding Agents

> **Core skill:** Delegating well-scoped work to agents and reviewing what returns

## What Changes from Junior to Mid

A junior engineer types a wish into a chat box and pastes whatever comes back. A mid-level engineer treats an agent like a fast, confident, context-starved teammate: you scope the work, hand over the context a teammate would need, define what "done" means before the first token is generated, and you review the result against that definition before anything merges. Delegation skill, not prompt magic, decides whether agents save time or move the mess downstream.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Task framing | Vague request: "add pagination" | Scoped unit with goal, constraints, and acceptance criteria |
| Context handed over | Whatever is in their head | The real files, conventions, and a worked example |
| Trust in output | Skims and accepts | Reads line by line against the spec |
| Verification | "It compiles" | Tests, edge cases, and behavior checks defined up front |
| Ownership | "The AI wrote it" | "I wrote it with help; I can explain every line" |
| Failure handling | Regenerates blindly and hopes | Diagnoses what the context or spec was missing |

## Core Frameworks

### Where Agents Are Strong and Where They Are Not

| Task Type | Agent Performance | Why | Your Job |
|-----------|-------------------|-----|----------|
| Boilerplate and scaffolding | Strong | Pattern-complete, low ambiguity | Check naming and layering match your codebase |
| Mechanical transformations | Strong | Rename, migrate, reformat, port syntax | Verify behavior is unchanged, with tests |
| Test drafts | Good draft, weak oracle | Generates plausible cases but misses your domain's real failure modes | Add the cases that actually bite your system |
| Exploration and summarization | Good first pass | Reads faster than you | Spot-check claims against the code; it misreads |
| Glue code and adapters | Good | Well-known shapes | Confirm error handling and idempotency |
| Novel architecture | Weak | No taste, no ownership of tradeoffs; output is generic | Design it yourself; delegate the fill-in |
| Subtle domain rules | Weak | Rules live in your code and business history, not in training data | Encode the rule in context yourself, or write it yourself |
| Concurrency and performance fixes | Weak and dangerous | Plausible-looking code that is subtly wrong under load | Prove it with measurements and tests you write |

### Task Decomposition for Delegation

| Element | What to Provide | Why It Matters |
|---------|-----------------|----------------|
| Goal | One sentence: what should be true when this is done | Everything else is filtered against it |
| Scope boundary | Files and modules in play; what must not change | Prevents sprawling refactors nobody asked for |
| Context | The actual code, conventions doc, a similar existing example | Output quality tracks context quality more than wording |
| Acceptance criteria | Observable checks: tests pass, endpoint shape, error behavior | Turns "looks right" into "is right" |
| Verification steps | How the result will be tested, and by whom | You cannot delegate the judgment that it works |
| Output format | Diff, full file, or plan first | Controls how expensive review will be |

### The Agentic Loop

| Stage | Agent Does | You Do | Exit Condition |
|-------|------------|--------|----------------|
| Plan | Proposes an approach and file list | Challenge assumptions; reject bad plans before code exists | Plan matches your architecture and scope |
| Implement | Produces changes against the plan | Watch for drift from plan and spec | Changes exist and are complete per scope |
| Verify | Runs tests it can run | Run the checks it cannot judge: behavior, security, license | All acceptance criteria demonstrated, not asserted |
| Integrate | Adjusts on feedback | Review, explain in the PR, merge under your name | The change is understood and owned by you |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Scope the task"] --> B["Assemble context"]
    B --> C["Write acceptance criteria"]
    C --> D["Review the agent's plan"]
    D --> E["Delegate implementation"]
    E --> F["Review output line by line"]
    F --> G["Run verification yourself"]
    G --> H["Ship under your ownership"]
```

## In Practice

**Delegate outcomes, not keystrokes.** "Add input validation to the signup endpoint" is an outcome; "write some validation code" is a keystroke request. Outcomes give the agent a target and give you a standard to review against. If you cannot state the outcome in one sentence, the task is not ready to delegate.

**Give the agent the same context a new teammate would need.** A new teammate gets the relevant files, the conventions doc, and a pointer to a similar feature that already exists in the codebase. The agent needs exactly that and nothing less; it cannot ask you follow-up questions the way a person can, so it fills gaps with plausible guesses instead. Context assembly is covered in [[03_Context_and_Prompting_for_Code_Tasks]].

**Write acceptance criteria before you write the prompt.** Decide what tests must pass, what the API shape should be, and what error behavior is required before generation starts. Criteria written afterward tend to bend toward whatever the agent produced. Pre-committing to checks is what makes the review phase objective.

**Treat the agent's plan as a proposal, not a decision.** In plan-then-implement workflows, the plan is the cheapest place to catch a wrong approach; the same mistake in code costs a full review cycle. Read the plan for scope creep, wrong abstractions, and invented requirements, then correct it before implementation runs. Rejecting a plan is cheap; rejecting an implementation is not.

**Review what returns like a pull request from a fast junior.** Speed of generation says nothing about correctness. Read every line against the acceptance criteria, check edge cases and error handling, and confirm nothing outside the declared scope changed. The review checklist in [[02_Reviewing_AI_Generated_Code]] is the same one you apply to human code, applied more strictly.

**Keep ownership explicit in every artifact.** Your name goes on the commit, the pull request, and the explanation in review. If you cannot explain a line, you do not ship that line — you either understand it, rewrite it, or delete it. "The agent wrote it" is not a defense at incident review and never will be.

**Prefer several small delegations over one large ask.** Small units are reviewable, verifiable, and easy to regenerate when wrong; one big ask produces a large diff you will skim, which is exactly where wrong-but-plausible code survives. When a task is large, decompose it yourself and delegate the leaves.

## Practical Exercise

Take one real task from your current work and run it through the full delegation loop:

1. Pick a task of medium size (a few hours of work): a feature slice, a migration, or a test suite expansion.
2. Write the goal and scope boundary in two sentences: what must be true, and what must not change.
3. Assemble the context pack: the actual files involved, your team's conventions or style guide, and one existing example of the desired output shape.
4. Write acceptance criteria as observable checks: named tests, API shapes, error behaviors.
5. Delegate the work in plan mode first; read the plan and reject or amend it before any code is generated.
6. Review the returned code line by line against the acceptance criteria; list every discrepancy you find, including style and convention mismatches.
7. Fix or regenerate the gaps, run the verification steps yourself, and write the pull request description as if you had typed every line — because you are accountable for every line.

## Practical Applications

- [ ] Every delegation starts with a one-sentence goal and a stated scope boundary
- [ ] My context pack includes real files, conventions, and a worked example of desired output
- [ ] Acceptance criteria are written before generation, not after
- [ ] Agent plans are reviewed and amended before implementation runs
- [ ] Returned code is read line by line; nothing merges on a skim
- [ ] I can explain any AI-assisted line in review without hedging
- [ ] Delegations are small units, not one sprawling request
- [ ] The commit, PR, and review explanation all carry my ownership

Delegation brief template:

```text
# Task: <one-line goal>

## Goal
<what must be true when this is done>

## Scope
<files and modules in play; what must NOT change>

## Context
<files provided, conventions that apply, one example of desired output>

## Acceptance criteria
- <observable check: test names, API shape, error behavior>
- <observable check>

## Verification
<how the result will be tested, and by whom>

## Output format
<plan first / diff / full files>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Delegating a vague wish | The agent fills gaps with plausible guesses you must later un-guess | Goal, scope, and acceptance criteria first |
| Handing over too little context | Output is generic and fights your conventions | Include real files and a worked example |
| Accepting the first generation as final | First drafts optimize for plausibility, not fit | Review, amend, regenerate deliberately |
| One huge delegation | The diff is too large to review properly | Decompose into small, verifiable units |
| Letting the agent define "done" | It asserts success instead of demonstrating it | You run the verification steps |
| Blaming the tool for wrong output | Wrong output usually means wrong framing or missing context | Fix the brief before blaming the model |
| Disowning generated code in review | Your name is still on the merge | Explain every line or do not ship it |

## Success Indicators

- Your delegation briefs contain goal, scope, context, and acceptance criteria before any generation starts
- You reject or amend agent plans regularly and cheaply, before code exists
- Review of returned code catches real defects, and you can name the ones you caught
- You can explain every line of your AI-assisted changes in pull request review
- Small delegations are your default; large ones are decomposed first
- You have a written list of task types you delegate and task types you keep

## Related

- [[00_overview]]
- [[02_Reviewing_AI_Generated_Code]]
- [[03_Context_and_Prompting_for_Code_Tasks]]
- [[06_When_Not_to_Use_AI]]
- [[career-path/01_Software_Engineer/02_Daily_Engineering_Craft/00_overview|Daily Engineering Craft]]
- [[SWEBOK v4 - Overview]]

## Sources

- *The Pragmatic Programmer* (David Thomas, Andrew Hunt)
- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright)
- *A Philosophy of Software Design* (John Ousterhout)
- *Working Effectively with Legacy Code* (Michael Feathers)
