---
title: "Regression Gates and Continuous Evaluation"
note_type: capability-topic
capability_area: evaluation-and-observability
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - regression-gates
  - continuous-evaluation
  - ci
---

# Regression Gates and Continuous Evaluation

> Wiring the eval suite into CI and scheduled pipelines so no prompt, model, or pipeline change ships without measured quality — and no drift goes unmeasured.

## Why This Is a Senior Skill

A mid-level engineer runs evals manually before a demo and "re-runs them when something changes." A senior engineer makes gates mandatory and fast: the fast subset on every PR, the full judged suite on merge, a production-shaped check before deploy, and scheduled continuous evaluation against live samples afterward. They gate on deltas from baselines rather than absolute scores, and they treat the suite itself as code subject to review.

The senior challenge is economics and trust: judge-based gates cost real money per run and can be flaky, so the pipeline must be cheap enough to run constantly and trustworthy enough that nobody bypasses it.

## Core Frameworks

### Gate Stages

| Stage | When | What Runs | Catches |
|-------|------|-----------|---------|
| PR check | Every pull request | Deterministic assertions, golden set, fast judge subset | Obvious regressions before review |
| Merge gate | Pre-merge | Full suite with per-metric deltas | Quality regressions across all strata |
| Pre-deploy check | Before release | Smoke tests with production-shaped traffic | Integration and config breakage |
| Continuous eval | Scheduled (nightly/weekly) | Suite against fresh production samples | Silent drift, provider and model changes |

### Gate Design Decisions

| Decision | Sound Default | Why |
|----------|---------------|-----|
| Absolute vs delta | Gate on deltas from the last accepted baseline | Absolute thresholds pass everything or block everything; deltas show what the change did |
| Per-metric thresholds | One threshold per metric, per stratum | Averages mask long-tail regressions |
| Allowed trade-offs | Define which metrics may trade (e.g., latency for faithfulness) | Prevents endless negotiating during release |
| Flakiness policy | Automatic single retry, then human decision | Judge non-determinism is real; unlimited retries defeat the gate |
| Eval budget | Per-run cost and time caps with sampling | Unbudgeted judge costs kill the practice |

### Example Tooling

| Tool | Approach | Fits |
|------|----------|------|
| promptfoo | Config-file eval definitions, CLI/CI native, assertions + judges | Teams wanting eval-as-code with minimal glue |
| Braintrust | Dataset-centric eval with CI integration | Eval-first teams with heavy curation |
| LangSmith | Traces + evals in one platform, CI hooks | LangChain stacks |
| Langfuse | Open-source tracing with eval API, self-hostable CI runners | Cost-sensitive, self-hosting teams |

As elsewhere: the capability is the gate — stage structure, delta comparison, budget, and runbook. The tools are interchangeable examples.

### Gate Anti-Patterns

| Anti-Pattern | What Happens | Fix |
|--------------|--------------|-----|
| Single absolute threshold | Passes everything until it blocks everything | Baseline-delta comparison per metric |
| Gating on the average | Long-tail regressions ship unnoticed | Per-stratum thresholds with the golden set as hard pass/fail |
| Retry-until-green | Flaky judge retried until the gate passes | One automatic retry, then a human decision |
| Suite drift | Thresholds lowered quietly to unblock a release | Suite changes reviewed like code; threshold changes need evidence |
| Eval as merge-only ritual | PRs ship blind; the gate sees code late | Fast tier on every PR; judged tier on merge |

## In Practice

**Gate on deltas from the baseline, not absolute scores.** "Faithfulness ≥ 0.95" passes everything until it blocks everything; "faithfulness may not drop more than 0.02 against the accepted baseline" says exactly what the change did. Deltas make the gate a comparison machine, which is what a regression gate is for.

**Split the suite into speed tiers for CI.** Deterministic checks run in seconds on every PR; the full LLM-judge suite runs on merge or on demand; humans review sampled judge disagreements. A gate that takes 45 minutes and $40 per PR will be bypassed; a gate that takes 90 seconds and $0.50 will not.

**Version the suite alongside the code and review changes to it.** A failing test is sometimes a wrong test. Suite changes — thresholds, rubrics, case removal — go through the same review as production code, with the same requirement: explain why the measurement changed.

**Automate provider and model update evaluation.** When the API provider ships a new model version or the team swaps models, scheduled continuous evaluation against a frozen sample set detects the behavior change within one cycle — before users have found it. Model upgrades become routine gated changes instead of leap-of-faith migrations.

**Budget evaluation like production infrastructure.** Compute the per-run cost of the judged suite and cap monthly spend; sample long-tail strata rather than cutting them; cache judge outputs keyed by input + rubric + judge version. The gate survives only if it stays boring and affordable.

**Run gates on every PR or they stop being gates.** The first time a team merges "around" the eval gate because it was slow or noisy, the gate is dead. Keep runtime and cost low, keep the failure output actionable — per-stratum deltas and example failures — and keep the runbook for failing gates one page long.

## Practical Exercise

Turn an existing eval suite into a release gate:
1. Split the suite into tiers: fast deterministic checks, judged subset, golden set
2. Add a CI workflow that runs the fast tier on every PR with a hard golden-set pass/fail
3. Implement baseline-delta comparison for the judged subset with per-metric thresholds
4. Simulate a prompt change you believe is an improvement; verify the gate reports the deltas
5. Add a scheduled weekly run of the suite against freshly sampled production queries
6. Write the one-page runbook: what a failing gate means, who acts, and what "fix the suite" requires
7. Measure the gate's cost and runtime for one week and tune tiers to fit the budget

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]]: "run on every PR; don't let quality degrade" — the mandate this topic implements
- [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]]: regression safety as an ROI claim
- [[02_Offline_Evaluation_Suites]] and [[03_Metrics_for_LLM_Outputs]]: the machinery the gate runs
- [[05_Online_Evaluation_and_Monitoring]]: the fresh samples continuous eval consumes
- [[career-path/09_Data_and_ML_Engineer/07_Production_Engineering/05_CI_CD_for_Data_and_ML|CI/CD for Data and ML]]: CI discipline from the data platform world
