---
title: "Agent Evaluation and Trajectories"
note_type: capability-topic
capability_area: agentic-systems
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
created: 2026-10-21
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - agents
  - agent-evaluation
---

# Agent Evaluation and Trajectories

> Evaluating agents over trajectories and episodes: step correctness, tool use, task success, and cost budgets — not single-output quality alone.

## Why This Is a Senior Skill

A mid-level engineer reads the agent's final answer, decides it looks right, and ships — missing the ten-step trajectory where the agent used the wrong tool three times, read a stale value, and lucked into a correct-looking answer that will not survive the next request. A senior engineer evaluates at three levels: the step (was each decision sound), the trajectory (was the path efficient and valid), and the episode (did the task actually complete, within budget). Single-output quality is a rounding error on an agent's real failure surface.

The senior challenge is making trajectory evaluation operational: replayable traces, budget accounting, human review sampling, and regression sets that turn production failures into permanent test cases. Agent evals cost more, take longer, and are noisier than single-output evals, so the discipline is choosing the cheap checks first and spending human and simulation effort where the failures actually live.

## Core Frameworks

### Why Single-Output Evals Miss Agent Failures

| Failure Mode | Looks Fine at the Output | Visible Only at Trajectory Level |
|--------------|--------------------------|----------------------------------|
| Lucky final answer | Correct answer, wrong reasoning | Step check: reasoning contradicts evidence used |
| Wrong tool, right result | Right value from an unintended source | Tool-call validity: selection and arguments reviewed per step |
| Partial progress | Final answer covers one of three subtasks | Episode outcome: task-completion checklist |
| Budget blowout | Quality is fine at any cost | Steps-to-success and cost-per-episode exceed ceiling |
| Silent retry loops | Answer arrives eventually | Efficiency metric: repeated identical calls in the trace |
| Unsafe path | Answer is correct but passed through an irreversible action | Side-effect audit over the trajectory |

### Evaluation Levels and Metrics

| Level | Metrics | What It Catches | Cost |
|-------|---------|-----------------|------|
| Step | Tool-call validity (schema, arguments), reasoning-evidence consistency, plan adherence | Wrong turns at the source | Low; automatable |
| Trajectory | Steps-to-success, redundant-call rate, recovery after error, path validity | Wandering, loops, brittle recovery | Medium; trace analysis |
| Episode | Task success (checklist), completion within budget, output quality | The only outcome the user experiences | Higher; often needs a grader |
| Fleet | Success distribution across tasks, budget adherence, escalation rate | Regressions hiding in averages | Per-release; regression suites |

### Budgets as First-Class Metrics

| Budget | Unit | Breach Meaning | Typical Enforcement |
|--------|------|----------------|---------------------|
| Step budget | Iterations or tool calls | Task wandered or looped | Hard stop with partial result |
| Cost budget | Tokens or money per episode | Value per request below cost floor | Ceiling with fail-closed exit |
| Time budget | Wall-clock per episode | Latency SLA at risk | Timeout with best partial answer |
| Human budget | Reviews or escalations per episode | Autonomy level too low for the task | Escalation cap; flag for redesign |

### Building the Evaluation Infrastructure

| Piece | Purpose | Senior Standard |
|-------|---------|-----------------|
| Replayable traces | Re-run an episode's decisions against fixtures | Every production trace is re-runnable offline |
| Simulation environment | Deterministic fixtures for tools and worlds | Same episode runs identically across releases |
| Human review sampling | Ground truth on a slice of real episodes | Sampled by risk and novelty, not at random only |
| Regression set | Locked episodes that must keep passing | Every incident becomes an episode in the set |
| Graders | Checklists and judges scoring episode outcomes | Rubric versioned with the set; judge calibrated to humans |

Named agentic benchmark families such as SWE-bench, WebArena, and GAIA exist for cross-system comparison; they calibrate capability but never replace your own domain episodes, which are what gate your releases.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Recorded production trajectory"] --> B["Replay against tool fixtures"]
    B --> C["Step checks on every call"]
    C --> D["Trajectory efficiency scoring"]
    D --> E["Episode outcome checklist"]
    E --> F["Budget accounting per episode"]
    F --> G["Human review on risk and novelty"]
    G --> H["Failures locked into regression set"]
    H --> B
```

## In Practice

**Evaluate the episode outcome first, then the trajectory that produced it.** Start with a task-success checklist: what must be true when the agent claims completion. Only then ask why it succeeded or failed; step metrics explain outcomes, they do not replace them.

**Score step correctness and tool-call validity on every trajectory, not samples of them.** Schema-valid, argument-correct, semantically appropriate tool calls are cheap to check mechanically and catch the failure modes that lucky answers hide. Automate these checks so they run on every episode in CI, and reserve humans for judgment calls.

**Treat cost, steps, and time as quality metrics, not billing afterthoughts.** An agent that solves a task in thirty steps where fifteen suffice has a quality problem even when the answer is right: the extra steps are extra failure surface and extra spend. Report steps-to-success and cost-per-episode beside success rate in every evaluation run.

**Make traces replayable and tools fixture-backed before you build any harness.** Evaluation without replay means every run is a live run: slow, expensive, and non-deterministic. Record tool inputs and outputs in the trace, then re-run episodes offline against fixtures so a regression costs a CI run instead of a production incident.

**Sample human review by risk and novelty, and calibrate automated graders against it.** Humans are the gold standard but too slow to review everything: review irreversible actions, low-confidence episodes, and novel task types first. Use the reviewed slice to calibrate judges and checklists so automation stays honest.

**Convert every production failure into a regression episode the week it happens.** An incident not captured as a replayable episode will recur silently. Lock the failing episode into the regression set with the expected behavior spelled out, so the fix and the future both run against it.

**Report distributions, never just averages.** Success rate can hold steady while a whole task class collapses into a tail nobody watches. Slice metrics by task type, budget tier, and autonomy level, and investigate every downward step between runs.

## Practical Exercise

1. Take an agent task and write an episode-outcome checklist: five to ten assertions that must hold when the task is complete.
2. Instrument the agent to record a full trajectory: plan, tool calls with arguments and results, budgets spent per step, final output.
3. Implement two mechanical step checks: tool-call schema validity and redundant-call detection; run them over ten episodes.
4. Build fixture-backed tool responses so one recorded episode replays identically offline; verify the replay reproduces the recorded trajectory.
5. Score the ten episodes on three levels (step validity, trajectory efficiency, episode success) and record cost and steps per episode.
6. Have a human review the three riskiest episodes and compare their verdicts to your automated checks; adjust the rubric where they disagree.
7. Convert one deliberately injected failure into a locked regression episode and wire the suite into CI.

## Practical Applications

- [ ] Every agent task has an episode-outcome checklist, versioned with the evaluation set
- [ ] Tool-call validity and argument correctness are checked mechanically on every trajectory
- [ ] Steps-to-success, cost-per-episode, and time-per-episode are reported beside success rate
- [ ] Traces are replayable offline against fixtures; replay reproduces recorded trajectories
- [ ] Human review samples are risk-weighted: irreversible actions, low confidence, novel tasks
- [ ] Automated graders are calibrated against human verdicts on a shared sample
- [ ] Production failures become locked regression episodes within the week
- [ ] Metrics are sliced by task type and budget tier; averages never stand alone

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Grading final answers only | Lucky answers and wrong paths pass; regressions hide | Three-level evaluation: step, trajectory, episode |
| Success rate without cost or steps | Wasteful wandering looks like quality | Budget metrics beside success in every report |
| Live-only evaluation runs | Slow, expensive, non-deterministic; nobody runs them | Fixture-backed replay of recorded trajectories |
| Random human review sampling | Review effort lands on routine episodes | Sample by risk, novelty, and low confidence |
| Averaged metrics across task classes | One class collapses inside a healthy mean | Slice by task type, budget tier, autonomy level |
| Incidents that never become test cases | The same failure recurs silently | Regression episode per incident, locked and versioned |

## Knowledge Connections

- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/02_Offline_Evaluation_Suites|Offline Evaluation Suites]]: suite construction and curation discipline agents extend
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/01_Evaluation_Fundamentals_and_Strategy|Evaluation Fundamentals and Strategy]]: the core evaluation thinking behind trajectory scoring
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/05_Online_Evaluation_and_Monitoring|Online Evaluation and Monitoring]]: production episodes as the replenishment source
- [[03_Tool_Protocols_and_Integration]]: tool-call validity depends on the contracts being testable
- [[05_Human_in_the_Loop_and_Autonomy_Levels]]: human review sampling is also the autonomy feedback loop
- [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]]: evaluation-to-guardrail lifecycle

## Sources

- *Designing Machine Learning Systems* (Chip Huyen) — evaluation infrastructure and regression discipline
- *AI Engineering* (Chip Huyen) — evaluating foundation-model applications end to end
- *Natural Language Processing with Transformers* (Tunstall, von Werra & Wolf) — evaluation methodology and metric design
