---
title: "Debugging Method"
role: Software Engineer
capability_area: Daily Engineering Craft
topic: Debugging Method
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - craft
  - debugging
---

# Debugging Method

> **Core skill:** Finding root causes with a hypothesis loop instead of guesswork

## What Changes from Junior to Mid

A junior engineer debugs by changing things until the symptom disappears. A mid-level engineer runs a hypothesis loop: reproduce reliably, form a hypothesis, design the cheapest test that can kill it, narrow along one axis at a time, and verify the fix against the original reproduction. The fix is only half the output; the other half is a write-up that prevents the same bug class from returning.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Starting point | Tweak code and re-run | Reproduce reliably, then form a hypothesis |
| Strategy | Random changes and long print sessions | Narrow by code, time, or data, one axis per step |
| Reading errors | Skims the first line of the stack trace | Reads the full message, the failing assertion, and the frames that matter |
| Tool choice | The tool they know best | Debugger, logs, tracing, or bisection chosen by the question |
| Definition of done | Symptom is gone | Root cause explained, fix verified, regression noted |
| Output | A commit | A commit plus a short bug write-up |

## Core Frameworks

### The Hypothesis Loop

| Phase | Action | Exit Condition |
|-------|--------|----------------|
| Reproduce | Trigger the bug reliably with minimal steps | You can make it appear and disappear at will |
| Hypothesize | State a testable cause: "X is null because Y skips initialization" | The statement can be proven false by one experiment |
| Test | Run the cheapest experiment that could kill the hypothesis | Result confirms or kills it decisively |
| Narrow | Cut the search space by code, time, or data | The suspect region is small enough to inspect fully |
| Fix | Change the root cause, not the symptom | The change is minimal and explainable |
| Verify | Re-run the reproduction and the surrounding tests | Bug gone, no new failures, root cause named |

### Bisecting by Three Axes

| Axis | Method | Best For | Cost |
|------|--------|----------|------|
| Code | Debugger stepping, targeted logging, commenting out regions | You suspect a specific path or module | Low to medium |
| Time | Binary search over commits (git bisect), or over deploys | The bug appeared between two known states | Medium; needs a reliable reproduction script |
| Data | Halve the input, mutate fields, replay a captured payload | The bug depends on specific input shape or values | Low; often the fastest axis |

### Tool Selection

| Question You Are Asking | Right Tool | Why |
|-------------------------|-----------|-----|
| What is the state at line X? | Debugger / breakpoint | Shows values without editing code |
| What path did execution take? | Tracing, structured logs with a correlation ID | Shows order and branches across services |
| When did behavior change? | git bisect over a reproduction script | Finds the offending commit without reading diffs |
| Does this input trigger it? | Minimal repro script or failing test | Turns the bug into a repeatable experiment |
| Is something slow or blocked? | Profiler, thread dump, spans in a tracer | Time and contention are invisible to a debugger |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Reproduce reliably"] --> B["State a killable hypothesis"]
    B --> C["Design the cheapest test"]
    C --> D["Narrow by code, time, or data"]
    D --> E["Fix the root cause"]
    E --> F["Verify with the reproduction"]
    F --> G["Write the bug write-up"]
```

## In Practice

**Reproduce before you theorize.** A bug you cannot reproduce is a bug you cannot verify you fixed. Reduce the report to the smallest reliable trigger — a script, a curl call, a failing test — and confirm it fails consistently. If you cannot reproduce it yet, reproducing is the task, not a precondition you can skip.

**Write the hypothesis as a killable sentence.** "The cache returns stale data after a deploy" is a hypothesis; "something is wrong with the cache" is not. Force the sentence to name a mechanism and a cause, then design the one experiment that would disprove it fastest. A hypothesis that survives a real test earns the next narrowing step.

**Narrow along one axis at a time.** Change one dimension per experiment — input halves, commit range, or code region — so the result is interpretable. Debugging by simultaneously editing code, swapping data, and downgrading a dependency produces results you cannot attribute to anything.

**Read the error message before reading the code.** The message, the failing assertion, and the top frames usually name the exact contract that broke. Read the whole message including the cause chain; the first line is often a wrapper. Most "impossible" bugs are a misread error message.

**Choose tools by the question, not by habit.** A debugger answers state questions, tracing answers flow questions, and bisection answers time questions. Reaching for printf when you need a timeline, or a debugger when you need to find a bad commit, wastes hours. Name the question first, then pick the instrument.

**Fix the root cause and name it.** A change that makes the symptom disappear without explaining it is a time bomb; the same class of bug returns elsewhere. Write one sentence naming the root cause before you commit — if you cannot write it, you have not found it yet.

**Turn every hard bug into a write-up.** After the fix, capture the symptom, the reproduction, the root cause, and the detection gap in a short note or ticket comment. This is the artifact that turns personal debugging into team knowledge and feeds your regression tests and documentation.

## Practical Exercise

Debug a real defect using the full loop:

1. Take a bug report and reduce it to a reliable reproduction: a script, a curl call, or a failing test you can run repeatedly.
2. Write one killable hypothesis sentence naming a mechanism and cause; list what evidence would disprove it.
3. Choose one narrowing axis (code, time, or data) and halve the search space with a single experiment; record the result.
4. Repeat narrowing until the suspect region is small enough to inspect fully; note each hypothesis you killed along the way.
5. Read the full error message and stack frames in the suspect area before editing anything; write down the contract that broke.
6. Implement the minimal fix at the root cause and explain it in one sentence.
7. Verify with the reproduction and the surrounding test suite, then write a four-part bug write-up: symptom, reproduction, root cause, detection gap.

## Practical Applications

- [ ] I have a reliable reproduction before I change any code
- [ ] My current hypothesis is written as a sentence that one experiment can disprove
- [ ] I narrowed along one axis per step and recorded what each result ruled out
- [ ] I read the entire error message and the relevant stack frames first
- [ ] I picked my tool by the question I was answering (state, flow, time, or input)
- [ ] The fix addresses the root cause and I can name it in one sentence
- [ ] The bug produced a write-up and, where it fits, a regression test
- [ ] I noted the detection gap: why did our tests or monitoring not catch this?

Bug write-up template:

```text
# Bug: <one-line symptom>

## Symptom
<what the user or system observed, with error text>

## Reproduction
<exact steps, input, or script; note frequency and environment>

## Root cause
<mechanism and cause, one or two sentences; cite the failing code path>

## Fix
<what changed and why this addresses the cause>

## Verification
<reproduction now passes; tests run; no new failures>

## Detection gap and follow-up
<why nothing caught this earlier; regression test or alert to add>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Debugging by random change | Symptoms move or vanish; the root cause stays | Hypothesis, experiment, narrow, repeat |
| Changing several things at once | Results cannot be attributed to any change | One variable per experiment |
| Fixing the symptom with a null check | The bug class returns elsewhere | Name and fix the root cause |
| Skipping reproduction because the bug "is obvious" | You cannot verify the fix or prevent regression | Build the minimal repro first |
| Reading only the first line of an error | The cause chain hides the real contract break | Read the full message and top frames |
| Debugging everything in the debugger | Wrong tool for timeline or input-shape questions | Match the tool to the question |
| Closing the ticket with no write-up | The same class of bug returns; knowledge stays personal | Capture symptom, repro, cause, detection gap |

## Success Indicators

- You produce a reliable reproduction before proposing fixes
- Your hypotheses are written so one experiment can kill them
- You can say what your last three experiments ruled out
- Hard bugs leave behind a write-up and usually a regression test
- You name the root cause in one sentence before committing
- Colleagues bring you bugs with repro steps because you taught them the habit

## Related

- [[00_overview]]
- [[01_Reading_Unfamiliar_Codebases]]
- [[05_Testing_in_Daily_Work]]
- [[career-path/07_SRE_and_Platform_Engineer/00_overview|SRE and Platform Engineer]]
- [[software-engineering-note/07_Software_Maintenance/Software Maintenance Overview]]
- [[SWEBOK v4 - Overview]]

## Sources

- *Debugging: The 9 Indispensable Rules for Finding Anything* (David Agans)
- *Working Effectively with Legacy Code* (Michael Feathers)
- *The Pragmatic Programmer* (David Thomas, Andrew Hunt)
- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright)
