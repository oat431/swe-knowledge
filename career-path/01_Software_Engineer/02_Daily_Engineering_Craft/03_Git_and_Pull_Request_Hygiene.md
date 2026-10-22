---
title: "Git and Pull Request Hygiene"
role: Software Engineer
capability_area: Daily Engineering Craft
topic: Git and Pull Request Hygiene
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - craft
  - git-and-pr
---

# Git and Pull Request Hygiene

> **Core skill:** Keeping history readable and reviews fast

## What Changes from Junior to Mid

A junior engineer treats commits as a save button and pull requests as a delivery chute. A mid-level engineer treats history as a shared document: commits at reviewable granularity with messages that say why, pull requests small enough to review in one pass, descriptions that answer the reviewer's questions before they are asked, and a history that stays bisectable when something breaks months later.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Commit granularity | Whatever accumulated since the last save | One logical change per commit, tests with the code |
| Commit messages | "fix", "wip", "updates" | Imperative summary plus context a stranger can follow |
| PR size | Everything the ticket needed, one PR | Sliced by intent; one concern per PR |
| PR description | A link to the ticket | What, why, how verified, risks |
| History quality | Broken by fixup noise and reverts of reverts | Bisectable: every commit builds and passes tests |
| Review rounds | Defensive, long threads | Focused responses, follow-up commits named clearly |

## Core Frameworks

### Commit Granularity and Message Conventions

| Element | Convention | Reason |
|---------|------------|--------|
| Size | One logical change: a fix, a refactor step, a feature slice | Reviewers and bisect can reason about one intent |
| Content | Code and its tests in the same commit | Never leaves a commit where tests fail |
| Summary line | Imperative mood, under about 72 characters, describes the change | Reads well in log one-liners and blame |
| Body | Why the change is needed, alternatives rejected, notable trade-offs | The code shows what; the body carries why |
| References | Ticket or PR link, related commits | Connects history to planning context |
| Cleanup | Squash fixups before merge; no "oops" commits in mainline | Bisectability and readable blame |

### PR Size Limits and Slicing by Intent

| Slice Type | Keeps Together | Splits Apart |
|------------|----------------|--------------|
| Refactor then feature | Behavior-preserving cleanup | The new behavior itself |
| Schema then code | Migration and the code that needs it | Unrelated new endpoints |
| Backend then frontend | API contract and its consumer | Cosmetic UI polish |
| Fix then follow-ups | The root-cause fix | Drive-by cleanups noticed along the way |
| Mechanism then rollout | The code path and its flag | Turning the flag on for everyone |

Practical limits that hold up in review: a pull request a reviewer can read in one sitting, changed files that all serve one stated intent, and any line count that forces the reviewer to scroll without a map. When in doubt, split: stacked PRs and follow-ups are cheaper than a review that rubber-stamps.

### PR Description Structure

| Section | Answers | Failure Mode When Missing |
|---------|---------|---------------------------|
| What | What changed, in one or two sentences | Reviewer reconstructs intent from the diff |
| Why | The problem, constraint, or ticket driving it | Reviewer cannot judge whether this is the right fix |
| How verified | Tests run, manual checks, before-and-after behavior | Reviewer must trust or re-verify everything |
| Risks | Blast radius, migrations, flags, things not covered | Surprises land in production, not in review |
| Review guide | Where to start, what to skim, what is generated | Review effort is wasted on noise |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Slice work by intent"] --> B["Commit one logical change"]
    B --> C["Write why-focused messages"]
    C --> D["Open a PR with what, why, verification, risks"]
    D --> E["Respond to review rounds"]
    E --> F["Squash and merge"]
    F --> G["Keep history bisectable"]
```

## In Practice

**Commit at the granularity of intent.** Each commit should answer one question: "what single change does this make, and why?" If your commit message needs the word "and" between two unrelated things, it is two commits. Commits sized by intent make review, revert, and bisect all cheap at once.

**Keep every commit buildable and test-passing.** Bisect is only useful if the states it visits actually run. Pair each code change with its tests in the same commit, and avoid committing broken intermediate states even on a feature branch; you will want to bisect your own work before it merges.

**Write the message for a stranger reading blame.** The reader is someone debugging at 2 a.m., possibly you, with no memory of the ticket. State the why: the constraint, the failed alternative, the observed bug. "Fix null deref in ingest" plus one sentence of cause beats a paragraph of diff narration.

**Slice pull requests by intent, not by time.** Size is a review-quality problem before it is a process problem: large diffs get shallow review. Split refactors from features, mechanism from rollout, and bug fixes from drive-by cleanups. If the story needs three concerns, ship three PRs and say how they stack.

**Write the description as the first review round.** The description should answer what, why, how it was verified, and what the risks are before anyone opens the diff. Include a short review guide: where to look first and what is mechanical. A good description turns review time into judgment time instead of archaeology.

**Protect bisectability like a test suite.** Avoid commits that move code and change behavior at once, avoid reverts of reverts when a rebase would do, and keep generated or formatting churn in separate commits. When a regression is found in six months, a clean history finds the guilty change in minutes.

**Respond to review rounds with named follow-ups.** Push fixes as separate commits during review so reviewers see exactly what changed, then squash at merge if your team prefers. Reply to every comment with a resolution (done, argued, or deferred with a reason), so the reviewer never has to re-read the whole diff to see what moved.

## Practical Exercise

Take a piece of work you have in progress and turn it into a reviewable stack:

1. List the distinct intents in your current changes (for example: refactor, feature, tests, config).
2. Reshape the work into one commit per intent, each with its tests; verify the branch builds and tests pass at every commit.
3. Write commit messages with a summary line plus a body that explains why; have a teammate read one message without the diff and say what changed and why.
4. Write a PR description with the four sections: what, why, how verified, risks; add a two-line review guide.
5. Check the diff for noise: generated files, formatting churn, unrelated edits; move anything mechanical into its own commit.
6. Estimate review time honestly; if the PR needs more than one focused sitting, split it and note how the pieces stack.
7. Practice one review round: ask a colleague for comments, push named follow-up commits, and reply to each comment with a resolution.

## Practical Applications

- [ ] Every commit does one logical thing and includes its tests
- [ ] Commit messages explain why, readable from git blame with no context
- [ ] The branch builds and passes tests at every commit, so bisect works
- [ ] The PR changes serve one stated intent; anything else is split or deferred
- [ ] The PR description covers what, why, how verified, and risks
- [ ] The description includes a review guide for non-obvious parts
- [ ] Review follow-ups are named commits that get squashed at merge if that is team practice
- [ ] Fixups, formatting, and generated churn are never mixed with behavior changes

PR description template:

```text
# <Imperative summary of the change>

## What
<one or two sentences: the change in behavior or structure>

## Why
<the problem or constraint; link the ticket; note rejected alternatives>

## How verified
<tests run, manual checks, before-and-after on the affected path>

## Risks
<blast radius, migrations, flags, known gaps, rollout plan>

## Review guide
<start here: file or commit; skip: generated or mechanical changes>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Commits as save points ("wip", "fix stuff") | Blame and bisect become useless | One intent per commit, message explains why |
| Giant pull requests | Review quality collapses into rubber-stamping | Slice by intent; stack PRs when pieces depend |
| Mixing refactors with behavior changes | Reviewer cannot tell which lines matter | Refactor commit first, behavior commit second |
| PR description that restates the diff | Reviewer spends time reconstructing intent | What, why, how verified, risks, review guide |
| Breaking the build in intermediate commits | Bisect lands on states that do not run | Keep every commit buildable and test-passing |
| Silent fixes during review | Reviewer cannot see what changed since their pass | Named follow-up commits, replies on every thread |
| Formatting or generated churn mixed in | Real changes hide in noise | Separate mechanical commits, or exclude the churn |

## Success Indicators

- Reviewers say your PRs are easy to read and quick to approve
- git log tells a stranger the story of why the code is the way it is
- You can bisect a regression in your own feature without fighting broken commits
- Your PR descriptions answer questions before reviewers ask them
- Review rounds are short because follow-ups are visible and answered
- You split work yourself before anyone asks you to

## Related

- [[00_overview]]
- [[04_Code_Review_Giving_and_Receiving]]
- [[06_Small_Task_Estimation]]
- [[career-path/05_Tech_Lead/06_Process_and_Quality_Stewardship/00_overview|Process and Quality Stewardship]]
- [[software-engineering-note/04_Software_Construction/Software Construction Overview]]
- [[career-path/01_Software_Engineer/01_Leveling_and_Growth/00_overview|Leveling and Growth]]

## Sources

- *The Pragmatic Programmer* (David Thomas, Andrew Hunt)
- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright)
- *A Philosophy of Software Design* (John Ousterhout)
