---
title: "Context and Prompting for Code Tasks"
role: Software Engineer
capability_area: AI Assisted Engineering
topic: Context and Prompting for Code Tasks
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - ai-assisted-development
  - prompt-engineering
---

# Context and Prompting for Code Tasks

> **Core skill:** Assembling the context that makes generated code usable

## What Changes from Junior to Mid

A junior engineer treats prompting as wordcraft: a cleverer phrasing will produce better code. A mid-level engineer treats it as information design: output quality is bounded by what the tool knows about your system, your conventions, and your intent, so the work is assembling the right context, stating the goal and constraints precisely, and iterating on failures by diagnosing what was missing. Prompt skill is mostly context skill wearing a different hat.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Main lever | Better wording | Better context: real files, conventions, examples |
| Problem framing | "Make it work" | Goal, constraints, output format, acceptance criteria |
| When output is wrong | Rewords the same prompt and regenerates | Diagnoses which missing context or constraint caused it |
| Reproducibility | One-off chat sessions nobody can repeat | Saved prompts and inputs that teammates can rerun |
| Context limits | Pastes everything and hopes | Cuts by relevance; keeps the load-bearing files |
| Examples | None | One worked example of the desired output shape |

## Core Frameworks

### Context Assembly

| Element | Example | What Goes Wrong Without It |
|---------|---------|----------------------------|
| The actual files | The module being changed, its callers, its tests | Output targets a plausible version of your code, not your code |
| Conventions | Style guide, naming patterns, layering rules, a pointer to a similar feature | Correct logic in the wrong shape; review becomes rework |
| A worked example | An existing function or test that already looks like what you want | Output format drifts; you rewrite structure instead of content |
| Constraints | Performance budgets, compatibility, "do not touch schema", error-handling policy | The tool optimizes for the happy path and breaks your non-negotiables |
| Domain rules | The business rule behind the requirement, not just its summary | Plausible code that implements the rule slightly wrong |
| Negative examples | "Do not use callbacks here", "no new dependencies" | Recurring violations you must strip out every iteration |

### Prompt Structure for Code Tasks

| Block | Content | Purpose |
|-------|---------|---------|
| Goal | One sentence: what must be true when done | Anchors every generated decision |
| Context | Files, conventions, and background the tool cannot infer | Replaces guesswork with fact |
| Constraints | What must not change, what must hold (perf, compat, security) | Bounds the solution space |
| Examples | One desired-output example; optionally one counterexample | Fixes shape and style without long description |
| Output format | Plan first, diff only, full file, or test-first | Controls review cost and scope of change |
| Acceptance criteria | Observable checks the output must satisfy | Turns review from taste into verification |

### Iteration and Regeneration Strategy

| Situation | Move | Why |
|-----------|------|-----|
| Output is wrong in one local spot | Fix forward by hand or ask for a targeted edit | Regenerating everything discards good work and re-review costs more |
| Output repeats the same defect after two attempts | Stop generating; fix the brief | The missing context or constraint is the bug, not the model's luck |
| Approach is fundamentally wrong | Reject and regenerate with an explicit plan step first | Patching a wrong design bakes in the wrong design |
| Style drifts from codebase | Add a worked example and restate the convention | Examples beat adjectives every time |
| Output invents APIs or behaviors | Add the real signatures to context and forbid invention | Unverifiable output cannot be owned |
| Result is close but inconsistent across runs | Freeze the prompt and inputs into a saved artifact | Reproducibility turns prompting into engineering |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Define the goal and constraints"] --> B["Gather real files and conventions"]
    B --> C["Add one worked example"]
    C --> D["State output format and acceptance criteria"]
    D --> E["Generate and evaluate against the criteria"]
    E --> F["Diagnose what context was missing"]
    F --> G["Fix forward or regenerate with the fix in the brief"]
    G --> H["Save the prompt and inputs for reuse"]
```

## In Practice

**Context quality caps output quality.** No phrasing can compensate for the tool not knowing your layering, your data shapes, or the rule behind the requirement. Before rewording anything, ask what a competent new teammate would need to read first, and put exactly that in the context. Rewording is the last lever, not the first.

**Show the desired shape instead of describing it.** One existing example of the output you want — a function, a test, a doc block in your style — constrains naming, structure, and tone faster than three paragraphs of description. Pair it with a counterexample when you keep getting the same wrong pattern. Examples are the highest-bandwidth context you can send.

**Structure every code prompt the same way.** Goal, context, constraints, examples, output format, acceptance criteria — every time, in that order. A fixed structure makes omissions visible: if you cannot fill in the constraints block, the task is not understood well enough to delegate. It also makes prompts comparable and debuggable across attempts.

**Diagnose failures before regenerating.** When output is wrong, name what was missing: the file the tool never saw, the constraint you assumed was obvious, the domain rule that lives only in your head. Fix that specific gap in the brief, then regenerate. Blind regeneration on the same input is a random number generator with formatting.

**Keep prompts and outputs reproducible.** Save the prompt, the context list, and the settings alongside the resulting change — in the repo, the PR description, or a team prompt library. Reproducibility lets a teammate regenerate the same work, lets you compare approaches honestly, and turns personal prompting tricks into team assets. An undocumented one-off is a claim you cannot back.

**Respect context limits by cutting, not shrinking.** When the context does not fit, drop the least load-bearing material: boilerplate, generated files, unrelated modules, verbose logs. Keep the spec, the touched code, its callers and tests, and the conventions. A short summary of cut material is better than truncated real code, which reads as complete when it is not.

**Say what not to do as clearly as what to do.** Constraints like "do not change the schema" or "no new dependencies" prevent the most expensive class of output: correct work you cannot accept. Negative constraints are cheap to write and costly to omit, because generation will happily take the liberty you left unspoken.

## Practical Exercise

Take one real code task and run it twice — once with a bare prompt, once with a designed brief — then compare:

1. Pick a small real task: a bug fix, a test addition, or a refactor of one module.
2. Run it with a bare one-line prompt and save the output; note every defect and mismatch you would have to fix.
3. Now assemble the full context pack: the real files involved, your conventions, one worked example of desired output, and the domain rule behind the requirement.
4. Write the structured prompt: goal, context, constraints, examples, output format, acceptance criteria.
5. Generate again and evaluate strictly against the acceptance criteria; count the defects that remain.
6. For each remaining defect, name the missing context or constraint that caused it and make one targeted revision; fix forward or regenerate by explicit decision.
7. Save the final prompt, context list, and outputs with the change so a teammate can reproduce it, and note which context element gave the biggest quality jump.

## Practical Applications

- [ ] Every code prompt follows the same structure: goal, context, constraints, examples, output format, acceptance criteria
- [ ] The context pack includes the real files, not summaries of them
- [ ] At least one worked example of desired output is provided for shape and style
- [ ] Negative constraints ("do not change X") are stated explicitly
- [ ] Failures are diagnosed as missing context or constraints before any regeneration
- [ ] Prompts, context lists, and outputs are saved so the work is reproducible
- [ ] Context is cut by relevance when it exceeds limits, with a summary of what was cut
- [ ] The domain rule behind a requirement is included, not just its summary

Prompt template:

```text
# Goal
<one sentence: what must be true when this is done>

# Context
<files and modules involved; conventions that apply; one worked example of desired output>

# Constraints
<what must not change; performance, compatibility, and security requirements>
<explicit negatives: no new dependencies, do not touch schema, ...>

# Output format
<plan first / diff only / full files / tests first>

# Acceptance criteria
- <observable check>
- <observable check>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Chasing cleverer wording | Phrasing cannot replace missing facts | Assemble real context first |
| Pasting whole repositories | Load-bearing details drown in noise | Cut by relevance; keep spec, touched code, tests, conventions |
| Describing output shape in prose | Words under-specify structure and naming | Include one worked example |
| Regenerating on the same input | Same causes, same defects | Diagnose the missing context or constraint |
| Omitting negative constraints | Generation takes every liberty left open | State what must not change |
| Editing generated output with no record | Nobody can reproduce or defend the result | Save prompts, context, and outputs with the change |
| Trusting summaries of code you did not send | Summaries lose exactly the details that decide correctness | Send the real files or verify against them yourself |

## Success Indicators

- Your structured briefs are complete before generation starts; you never prompt from a blank chat
- One worked example per task type is in your context pack by default
- You can name the missing context behind each recent wrong output
- Teammates can reproduce your AI-assisted changes from saved prompts and inputs
- You cut context deliberately by relevance and never by truncating code silently
- Your second attempts succeed because the brief improved, not because luck did

## Related

- [[00_overview]]
- [[01_Working_With_Coding_Agents]]
- [[02_Reviewing_AI_Generated_Code]]
- [[07_An_AI_Assisted_Daily_Workflow]]
- [[career-path/01_Software_Engineer/02_Daily_Engineering_Craft/00_overview|Daily Engineering Craft]]
- [[SWEBOK v4 - Overview]]

## Sources

- *The Pragmatic Programmer* (David Thomas, Andrew Hunt)
- *A Philosophy of Software Design* (John Ousterhout)
- *The Mythical Man-Month* (Fred Brooks)
- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright)
