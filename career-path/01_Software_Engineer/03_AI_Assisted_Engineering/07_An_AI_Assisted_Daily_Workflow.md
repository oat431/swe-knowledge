---
title: "An AI Assisted Daily Workflow"
role: Software Engineer
capability_area: AI Assisted Engineering
topic: An AI Assisted Daily Workflow
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - ai-assisted-development
  - workflow
---

# An AI Assisted Daily Workflow

> **Core skill:** Integrating assistance into the daily craft loop end to end

## What Changes from Junior to Mid

A junior engineer bolts AI onto one step of their day — usually typing code faster — and calls it a workflow. A mid-level engineer integrates assistance across the whole craft loop: reading code, estimating, implementing, testing, reviewing, documenting, with explicit checkpoints where human judgment gates progress. The workflow is honest about what the tool does well and what stays human, and it stays stable enough that teammates can see and trust how work actually gets done.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Where AI appears | One step: code typing | Across the loop: reading, estimating, implementing, testing, reviewing, documenting |
| Checkpoints | None; output flows straight to commit | Explicit gates where human judgment decides progress |
| Estimating | Uses generation as a timing oracle | Estimates by decomposition; uses AI to surface hidden scope |
| Testing | Accepts generated tests as coverage | Writes the failing test first; generation fills the matrix |
| Team visibility | Ad hoc, undocumented | Workflows are visible, described, and reviewable |
| Honesty | Speed stories | Measured outcomes and admitted limits |

## Core Frameworks

### Workflow by Task Type

| Task Type | Where AI Helps | Where Human Judgment Gates |
|-----------|----------------|----------------------------|
| Reading unfamiliar code | Summaries of modules, call-graph drafts, question answering | You trace the critical path yourself before trusting any summary |
| Estimating work | Surfacing hidden scope, listing affected files, edge cases | You set the estimate; the tool has no accountability for the number |
| Implementing features | Boilerplate, scaffolding, mechanical transforms, test drafts | Plan approval first; scope and design decisions stay yours |
| Debugging | Hypothesis generation, log pattern reading, candidate explanations | Evidence gathering and root-cause judgment stay yours |
| Writing tests | Case matrices, fixture scaffolding, repetition-heavy suites | The failing-test-first step and the "what can actually break" list are yours |
| Reviewing code | First-pass scan for style, obvious defects, missing checks | Final correctness, security, and license verdicts are yours |
| Documenting | Draft docs, PR descriptions, changelogs from diffs | Accuracy of claims and what is worth saying are yours |
| Refactoring | Mechanical renames, signature changes, pattern migration | Behavior-preservation judgment and test strategy are yours |

### Human Judgment Checkpoints

| Stage | Gate Question | Evidence Required |
|-------|---------------|-------------------|
| Before delegation | Is this task on my delegate list, with a written brief? | Goal, scope, acceptance criteria |
| Before implementation | Do I approve the plan, including scope and approach? | Plan reviewed; scope boundary respected |
| Before acceptance | Do I understand every line well enough to explain it? | Line-by-line review notes |
| Before merge | Do tests I trust pass, including one I wrote myself? | Test results; independent verification |
| Before review submission | Do security and license checks pass? | Scanner results; provenance check |
| After merge | Did the change behave in production as specified? | Deployment and defect signals |
| Periodically | Did assistance improve my outcomes, or just my activity? | Measurement log on comparable tasks |

### Keeping the Workflow Honest in Team Settings

| Practice | What It Prevents |
|----------|------------------|
| Declare AI-assisted changes in the PR description | Reviewers under-reviewing code they assume is human-drafted |
| Keep prompts and context lists with the change | Unreproducible work defended by "the tool said so" |
| Agree as a team where the gates are | Individual shortcuts becoming the team's quality floor |
| Share what failed as well as what worked | Repeated mistakes and inflated expectations |
| Measure team outcomes, not tool adoption | Tooling theater where usage rises and delivery does not |
| Never outsource a review verdict | Liability drift toward "the model approved it" |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Read and orient with tool support"] --> B["Estimate with your own decomposition"]
    B --> C["Write acceptance criteria and the failing test"]
    C --> D["Review the agent's plan"]
    D --> E["Implement with delegation where it fits"]
    E --> F["Verify with tests you trust"]
    F --> G["Review code, security, and license"]
    G --> H["Document and declare AI assistance in the PR"]
    H --> I["Measure outcomes and adjust the workflow"]
```

## In Practice

**Integrate assistance into the loop, not onto it.** The daily loop is read, estimate, implement, test, review, document — assistance belongs at each stage in a defined role, not as a bolt-on at the typing step. Define for each stage what the tool does and what stays human, then keep that split stable for a while. A workflow you change every week is a habit you cannot evaluate.

**Keep a human gate at every stage where judgment is the product.** Plan approval, line-by-line understanding, independent tests, security and license verdicts, and final review calls are checkpoints where a person decides. Each gate needs a question and an evidence standard — not "does this feel fine" but "does every line explain, do my tests pass." Gates without evidence are theater.

**Write the failing test before generation touches the code.** A hand-written failing test defines correct behavior independently of whatever the tool will produce, and it keeps testing instinct sharp. Generation can then expand the case matrix and fixtures around your anchor test. The order matters: definition of done first, volume second.

**Estimate by decomposition; let AI surface scope, not set numbers.** Use the tool to list affected files, hidden edge cases, and migration costs you might have missed — genuinely useful for making estimates complete. But the estimate is a commitment under your name, so you set it from your own decomposition. Tools that produce code fast distort duration intuition; that is exactly when estimates need more structure, not less.

**Document while the context is hot, and declare assistance.** Generate the PR description and doc drafts from the real diff, then correct every claim yourself — generated summaries overstate completeness confidently. State in the PR where and how AI was used, so reviewers calibrate their scrutiny. Deception by omission here quietly lowers the team's review quality.

**Review with the checklist, verdict included.** Use the tool for a fast first scan of style and obvious gaps, but run your own pass for correctness, security, and license provenance, per [[02_Reviewing_AI_Generated_Code]]. The verdict — accept, fix, reject — is yours and must be explainable in one paragraph. A review with no human verdict is not a review.

**Keep the workflow honest by measuring and sharing.** Track outcomes around AI-assisted work — lead time, review rounds, defects, rework — and compare them to your baseline as in [[05_Measuring_Real_Productivity]]. Share failed experiments alongside wins so the team's expectations track reality. Honest workflow hygiene is what turns tool adoption into delivery improvement.

## Practical Exercise

Build your own AI-assisted daily workflow for one week and pressure-test it:

1. Map your current craft loop stages: reading, estimating, implementing, testing, reviewing, documenting; for each, write today's tool usage and today's human work.
2. For each stage, decide the split: what the tool does, what you do, and the gate question that decides progress.
3. Define the evidence for each gate: which tests, which checks, which explanations must exist before the stage passes.
4. Run one real task through the workflow end to end, recording where the gates actually caught something and where they were skipped.
5. Write the PR description from the real diff, correct every claim yourself, and include a declaration of where AI was used.
6. Collect the week's numbers — lead time, review rounds, defects found in review, rework — and compare against your own recent baseline.
7. Adjust one thing in the workflow based on what the week showed, and write one sentence on what you would tell a teammate about what is safe to delegate and what is not.

## Practical Applications

- [ ] Each loop stage has a defined tool/human split and a named gate question
- [ ] The failing test I wrote myself exists before generated code does
- [ ] Plans are approved by me before implementation runs
- [ ] Estimates come from my decomposition; AI only surfaces hidden scope
- [ ] Every merge passes tests I trust, a security and license check, and my own review verdict
- [ ] PR descriptions declare where and how AI was used
- [ ] Prompts and context lists are saved with AI-assisted changes
- [ ] I hold a weekly look at outcomes: lead time, review rounds, defects, rework

Daily workflow template:

```text
# Task: <ticket or goal>

## Stage plan
reading: <tool role / my role / gate>
estimating: <tool role / my role / gate>
implementing: <tool role / my role / gate>
testing: <tool role / my role / gate>
reviewing: <tool role / my role / gate>
documenting: <tool role / my role / gate>

## Evidence collected
<my anchor test; scanner results; explanation notes>

## Declaration for the PR
<where AI was used and what I verified myself>

## Post-merge note
<outcome vs estimate; anything the gates missed>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Adding AI only at the typing step | Speed up front, verify-and-fix cost in the tail | Define the tool/human split at every loop stage |
| Gates that ask "does this feel fine" | Feelings are what plausible output manipulates | Evidence-based gate questions per stage |
| Accepting generated tests as the only coverage | Shared blind spots pass themselves | The failing test first, written by hand |
| Letting tools set estimates | Fast generation distorts duration intuition | Decompose yourself; AI surfaces scope |
| Undeclared AI assistance in PRs | Reviewers calibrate wrongly and under-scrutinize | Declare usage in the description |
| Treating adoption as improvement | Activity rises while delivery may not | Measure outcomes weekly and adjust |
| Chopping and changing the workflow constantly | No stable baseline to judge against | Change one element at a time, on evidence |

## Success Indicators

- Your workflow covers every loop stage with a defined tool/human split
- Every gate has an evidence standard, and gates actually block skips
- Your PRs declare AI usage and carry explanations you can defend in review
- Your weekly numbers show outcome movement, not just activity movement
- Teammates know your workflow and borrow parts of it
- You can state plainly which tasks you delegate and which you refuse

## Related

- [[00_overview]]
- [[01_Working_With_Coding_Agents]]
- [[03_Context_and_Prompting_for_Code_Tasks]]
- [[05_Measuring_Real_Productivity]]
- [[career-path/01_Software_Engineer/02_Daily_Engineering_Craft/00_overview|Daily Engineering Craft]]
- [[career-path/05_Tech_Lead/06_Process_and_Quality_Stewardship/00_overview|Process and Quality Stewardship]]

## Sources

- *The Pragmatic Programmer* (David Thomas, Andrew Hunt)
- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright)
- *The Mythical Man-Month* (Fred Brooks)
