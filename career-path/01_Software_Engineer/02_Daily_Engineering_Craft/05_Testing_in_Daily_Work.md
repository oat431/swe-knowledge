---
title: "Testing in Daily Work"
role: Software Engineer
capability_area: Daily Engineering Craft
topic: Testing in Daily Work
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - craft
  - testing
---

# Testing in Daily Work

> **Core skill:** Choosing the right test level for the risk in the change

## What Changes from Junior to Mid

A junior engineer writes tests to make the coverage number look good and tests whatever the framework makes easy. A mid-level engineer reads the risk in the change first (money moves, state corrupts, a parser breaks) and chooses the test level that would catch that risk if it regressed. The question is never "how many tests?" but "which failure would hurt, and what test notices it first?"

| Aspect | Junior | Mid |
|--------|--------|-----|
| Test selection | Whatever the framework scaffolds | Level chosen by the risk in the change |
| Typical feature coverage | Happy path only | Happy path, edge cases, error handling, regression for the bug |
| Test style | Asserts implementation details | Asserts behavior; survives refactors |
| Naming | test1, test_parse_ok | Names state the behavior and the case |
| Flaky tests | Retried until green | Treated as defects: diagnosed or quarantined |
| Coverage goal | A number to raise | A signal to read alongside risk |

## Core Frameworks

### Test Level Trade-Offs

| Level | What It Proves | Speed | Failure Localization | Cost to Maintain | Best Used For |
|-------|----------------|-------|----------------------|------------------|---------------|
| Unit | Logic is correct in isolation | Fast | Exact | Low if behavior-focused | Pure logic, edge cases, error branches |
| Integration | Modules work together against real collaborators | Medium | Coarse | Medium | Queries, serialization, contracts, wiring |
| End-to-end | The user-visible path works in a deployed shape | Slow | Poor | High | A few critical journeys, not every branch |
| Manual / exploratory | Behavior under real judgment and surprise | N/A | N/A | Time each run | New UX, unclear requirements, one-off checks |

### What to Test in a Typical Feature

| Category | Question It Answers | Example Shape |
|----------|--------------------|---------------|
| Happy path | Does the intended flow produce the intended result? | Given valid input, the output and state change are correct |
| Edge cases | What happens at boundaries? | Empty list, one item, max size, first and last, zero, duplicate |
| Error handling | What happens when a dependency fails? | Timeout, invalid input, permission denied; errors surfaced, not swallowed |
| Regressions | Does the bug I just fixed stay fixed? | A test that fails without the fix and passes with it |
| Contracts | Do both sides of a boundary agree? | Request/response shape, DB constraints, event schemas |

### Coverage as a Signal, Not a Goal

| Signal | Read It As | Action |
|--------|-----------|--------|
| High coverage, low risk overlap | Tests exist where they are cheap, not where it hurts | Add tests at the risky boundaries instead |
| Low coverage on risky code | Unprotected blast radius | Test the risky paths first |
| Coverage drop on a PR | Behavior changed without a test noticing | Add the missing assertion before merge |
| High coverage, frequent escapes | Tests assert implementation, not behavior | Rewrite asserts around observable outcomes |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Name the risk in the change"] --> B["Choose the test level"]
    B --> C["Write the happy path test"]
    C --> D["Add edge and error cases"]
    D --> E["Add regression tests for bugs"]
    E --> F["Run and check for flakes"]
    F --> G["Read coverage as a risk map"]
```

## In Practice

**Choose the test level by asking what could hurt.** A pricing calculation deserves dense unit tests on edge cases; a SQL migration deserves integration tests against a real schema; a login flow deserves one end-to-end check. Writing three unit tests for an integration risk protects nothing; match the instrument to the failure you are guarding against.

**Write the regression test first when fixing a bug.** A bug fix without a test is a promise nobody can check. The test should fail without the fix and pass with it; that pair proves you found the cause. Every bug report is a free test case specification.

**Assert behavior, not implementation.** Tests that reach into private state or assert exact call sequences break when you refactor without changing behavior, teaching the team to fear cleanups. Assert what the caller observes: return values, state changes, side effects, error messages. Refactor-proof tests are tests you will keep.

**Name tests so the failure is a sentence.** A failing test name should state the missing behavior: "rejects an expired token" beats test_token_2. When the suite breaks at 2 a.m., readable names are the triage tool. The name plus the assertion should be enough to know what broke without opening the implementation.

**Treat flaky tests as defects, not weather.** A test that fails without a code change is either wrong or the code is nondeterministic: both are real bugs. Diagnose the cause (shared state, timing, order dependence), fix it, or quarantine it with a ticket; never let the team absorb a retry ritual. Flakiness trains everyone to ignore red builds.

**Use manual and exploratory testing where judgment matters.** New interfaces, unclear requirements, and one-off data problems are better explored by a human clicking around with intent than by scripted checks. Automate what must stay true; explore what you are still learning. Exploratory sessions belong in the plan, not as guilt afterthoughts.

**Read coverage as a risk map, not a score.** Coverage tells you where tests are absent, not where behavior is safe. Aim your effort at the intersections of low coverage and high consequence, and let trivial getters and framework glue sit uncovered without guilt. The number moves; the risk map is the point.

## Practical Exercise

Add tests to a real feature you are shipping:

1. List the risks in the change: what could corrupt state, lose money, break a contract, or annoy a user if it regressed.
2. Choose a test level per risk: unit for logic, integration for boundaries, one end-to-end check for the critical journey.
3. Write the happy path test with a name that reads as a behavior sentence.
4. Add edge cases at the boundaries you listed (empty, one, maximum, first/last) and one error-handling test per failing dependency.
5. If this feature fixed a bug, write the regression test first and confirm it fails without the fix.
6. Run the suite twice in a row and in shuffled order if your framework supports it; investigate any non-determinism.
7. Look at the coverage diff for your PR and pick one uncovered risky line or branch to cover; ignore the trivial ones.

## Practical Applications

- [ ] I named the risk in each part of my change before choosing a test level
- [ ] The feature has happy path, edge case, and error handling tests where the risk warrants them
- [ ] Every bug I fixed this month has a regression test that fails without the fix
- [ ] My tests assert observable behavior and survive refactors
- [ ] Test names read as sentences describing the guaranteed behavior
- [ ] No test in my suite is flaky; if one is, it is quarantined with a ticket
- [ ] I ran at least one exploratory pass on anything user-visible
- [ ] I read the coverage diff as a risk map, not a number to raise

Risk-to-test checklist template:

```text
# Test plan: <feature or fix>

| Risk | If It Regresses | Test Level | Test Name |
|------|-----------------|------------|-----------|
| <e.g. wrong total on discount> | <user overcharged> | unit | applies discount before tax |
| <e.g. schema mismatch> | <write fails in prod> | integration | persists and reloads order |
| <e.g. broken login flow> | <nobody can sign in> | e2e | user signs in with valid credentials |

Regression tests to add: <bug fixes in this change>
Manual / exploratory pass: <what a human should click through>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Testing only the happy path | Regressions hide in edges and error paths | Test boundaries and failing dependencies too |
| Chasing a coverage number | Trivial tests add noise, not safety | Cover risk intersections, ignore the boilerplate |
| Asserting implementation details | Refactors break tests without changing behavior | Assert observable outcomes |
| Retrying flaky tests | Determinism rot spreads; red builds lose meaning | Diagnose, fix, or quarantine with a ticket |
| Fixing a bug with no regression test | The same bug returns silently | Test first: fails without fix, passes with it |
| End-to-end tests for everything | Slow suite, poor localization, constant upkeep | Reserve e2e for a few critical journeys |
| Skipping exploratory testing | Scripted checks miss what nobody thought to script | Schedule judgment-driven passes for new UX |

## Success Indicators

- Your test choices trace clearly to named risks in the change
- Bug fixes you ship come with regression tests automatically
- Your suite runs fast enough that people run it locally before pushing
- Refactors do not break your tests; behavior changes do
- A failing test name tells the story of what broke without opening code
- You can say what your tests do not cover and why that is acceptable

## Related

- [[00_overview]]
- [[02_Debugging_Method]]
- [[04_Code_Review_Giving_and_Receiving]]
- [[software-engineering-note/05_Software_Testing/Software Testing Overview]]
- [[career-path/10_Quality_and_Test_Engineering/00_overview|Quality and Test Engineering]]
- [[career-path/02_Senior_Software_Engineer/05_Quality_Reliability_Security/00_overview|Quality, Reliability, Security]]

## Sources

- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright)
- *Working Effectively with Legacy Code* (Michael Feathers)
- *The Pragmatic Programmer* (David Thomas, Andrew Hunt)
