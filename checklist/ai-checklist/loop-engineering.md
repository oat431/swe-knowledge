# Loop Engineering Checklist

> Designing the **outer loop** around your agents — the system that finds the work, hands it out, checks the result, records what's done, and decides what happens next. Design once, instead of prompting per turn.
> The inner loop (gather context → act → verify) ships with the model and the harness. **The outer loop is yours to build** — and it's the difference between an agent floor that runs for days and one that melts by morning.
> Siblings: [[graph-engineering|Graph Engineering]] (the topology above the loop), [[general-agents-driven]] §6 (verification loops), [[claude-driven]] (steering stack), [[ai]] §4 (agents & tool use).
> Deep dive in the vault: [[career-path/18_Applied_AI_Engineer/01_LLM_Application_Patterns/03_Agent_Loops_and_Orchestration|Agent Loops and Orchestration]].
> Last updated: 2026-09-17 (new file — synthesized from Anthropic's harness guidance, the Osmani loop anatomy, and the mid-2026 loop-engineering discourse)

---

## 1. Fit — Does This Earn a Loop?

- [ ] **Task repeats** — Loops pay off when the same job recurs (nightly triage, PR review, doc sync, release notes). One-off work gets a prompt, not a loop.
- [ ] **Verification is automated** — If nothing external can check "done" (tests, schema, diff, ground truth), the loop can't converge — it can only look busy. Verification machinery is the entry ticket.
- [ ] **Unit of work is bounded** — One scoped item per run ("fix this failing test", "triage this issue"), never "improve the repo". Open-ended scope is where budgets die.
- [ ] **Mistakes are survivable unattended** — An unattended loop also makes mistakes and spends money unattended. Branch sandboxes, dry-runs, or restricted credentials before giving it the wheel.
- [ ] **Loop beats a pipeline** — If the steps are known in advance, write them as code with model-powered steps. A loop earns its cost only when the path depends on intermediate results you can't enumerate.
- [ ] **Trigger shape chosen** — Scheduled (cron), event-driven (webhook / new issue), or drain (queue, once started). Each has different failure modes — pick one per loop and write it down.

## 2. Trigger & Intake

- [ ] **One deterministic entry point** — A loop with three ways in and no record of which was used is an incident in slow motion. Every run records its trigger, source, and `run_id`.
- [ ] **Work arrives as a queue, not a vibe** — The loop drains an explicit work list (issues, queue table, mailbox). It does not discover scope by browsing.
- [ ] **Deduplication per item** — The same item must not spawn parallel runs: locks or idempotency keys keyed by work-item identity.
- [ ] **Priority is explicit** — When items compete, the order is defined (severity, age, deadline). Free-form agent prioritization is cost you don't control.
- [ ] **Intake validates before spending** — Malformed, empty, or duplicate items are rejected at intake, before burning a model call.

## 3. The Inner Loop (per session)

- [ ] **Plan → act → observe, with verified observations** — The model reads tool results and re-plans when reality contradicts its plan; it doesn't press on through contradictions.
- [ ] **Getting-bearings ritual** — Every fresh session starts the same way: read progress notes → git/state log → pick the highest-priority unfinished item → run the baseline check before starting new work. (Anthropic's long-running harness pattern.)
- [ ] **One item per session** — Incremental progress. Agents that try to one-shot an entire project run out of context halfway and leave undocumented half-states.
- [ ] **Explicit finish condition** — A schema-shaped final output or a finish tool. "Could not complete X" is a designed, acceptable answer.
- [ ] **Tool surface limited per step** — A loop choosing among 30 tools stalls and wanders; one choosing among the 3 relevant ones converges. The decision space is your cost surface.
- [ ] **Context hygiene is part of the cycle** — Compaction/restart scheduled as maintenance, not emergency cleanup; durable state lives outside the context window so a fresh window can resume cleanly.

## 4. Convergence — Bounds, Progress, Exits

> A loop converges when three things are true: **a progress measure that moves, bounds, and defined exits.** A loop missing any one of them is a runaway waiting for a quiet weekend.

- [ ] **Progress measure is external and verified** — Tests passing, cards moving, diffs landing — never the model's self-report. The looper does not certify its own work.
- [ ] **Iteration cap** — Max steps per run, chosen from trace data (start ~5–15 for tool loops), not vibes.
- [ ] **Token / cost budget per run** — The guaranteed-termination clause. Even if every other signal fails, the loop halts at a ceiling you set while calm.
- [ ] **Wall-clock budget** — A loop that can outlive its SLA needs a timeout, with the best partial answer attached.
- [ ] **No-progress tripwire** — External signals only: repeated near-identical actions, the same error N times, spend rising while verified state stays flat, silent stalls (working with no output).
- [ ] **Every exit is designed** — Done-and-verified, budget-exhausted, escalated-to-human. All other terminations (crashed, wedged, orphaned) are bugs.
- [ ] **Fail closed** — On any breach: return the best partial result with a status flag, or escalate. Never silently keep spending.

## 5. Stop Conditions & Done-Certification

- [ ] **Stop checks state, not chatter** — "The model stopped talking" is not done. Done = the item's definition-of-done verified by an external check.
- [ ] **Definition of done per work item** — Written as a check (test green, artifact exists, PR opened and CI-passed), not a feeling. Can't write it? The item isn't ready for the loop.
- [ ] **Maker / checker separation** — A different step, agent, or model adjudicates results and moves the board. The implementer never marks its own homework.
- [ ] **Status advances only on evidence** — No `passes: true` without the artifact trail: command + output, commit hash, screenshot, test report.
- [ ] **Progress files are structured (JSON over Markdown)** — Models tamper less with structured files; entries start as "failing" and may only flip status after real verification. The standing red line — "it is unacceptable to remove or edit tests" — belongs in the loop's own instructions.
- [ ] **Partial results are honest and labeled** — "Here's what I found; X could not be completed" beats a confident fabrication. Make that outcome acceptable in the loop's contract.

## 6. Retry, Recovery & Circuit Breakers

- [ ] **Retry with backoff for transient failure only** — Rate limits, flaky network, wedged terminal. Deterministic errors (bad input, failing assertion) don't get retried into the void.
- [ ] **Identical immediate retries are a smell** — The 40th identical retry is a runaway, not persistence. Escalate instead.
- [ ] **Breaker ladder: steer → constrain → stop** — Graduated response preserves work: corrective nudge into the session → tighten permissions/scope → stop. Binary kill switches throw away recoverable work.
- [ ] **Escalation has a destination** — A stopped loop lands in a human approvals queue, not a silent dead process.
- [ ] **Drain loops have anti-recursion guards** — A blocked stop may continue once per real stop (a `stop_hook_active`-style flag). Without it, forced continuation becomes a hall of mirrors.
- [ ] **Recover the loop, not just the failure** — Revive wedged sessions, catch up missed schedules, re-arm the router. Reliability means the loop self-heals; errors being logged isn't the same thing.
- [ ] **Sandbox breach = stop** — Permission denials, unexpected egress, credential errors: pause for review. Never let the loop "work around" its own boundaries.

## 7. State, Memory & Artifacts

- [ ] **External state is the source of truth** — Board/queue/progress file + git. The loop reads it at start, writes it at end; the context window is scratch space, not state.
- [ ] **Sessions end in a clean state** — Mergeable-quality: committed, documented, no half-features, baseline green. The next session (or the next engineer) starts from clean.
- [ ] **Artifacts are addressable** — Branch names, PR links, file paths recorded per run — work is inspectable and revertible without archaeology.
- [ ] **Compaction preserves decisions, not just narrative** — Summaries carry open questions, next steps, and rejected paths forward.
- [ ] **Memory has an owner and a pruning rule** — What the loop remembers across runs is curated deliberately (learned skills, conventions). Unmanaged memory accumulates poison.

## 8. Human Gates

- [ ] **The human is a designed exit, not an interrupt handler** — The approvals queue is a first-class terminal state of the loop.
- [ ] **Gates sit where consequence concentrates** — Before irreversible action: sends, payments, deletions, prod deploys, public posts, scope changes.
- [ ] **Escalation triggers are explicit** — Spend threshold, scope change, destructive op, low confidence after budget — each maps to "ask a human", not "try again".
- [ ] **The gate has an owner and an SLA** — A queue nobody reads is a stopped loop with extra steps.
- [ ] **Approval resumes, not restarts** — Continue from saved state (checkpoint); side effects are idempotent so the resume can't double-fire.

## 9. Observability & Cost

- [ ] **Every run traced end-to-end** — Plan, tool calls, observations, per-iteration cost. If you can't trace every step, you can't run a loop in production.
- [ ] **Loop health metrics** — Runs started/finished/escalated; iterations per run (p50/p95); tokens and wall-clock per completed item; queue depth. Watch trends, not incidents.
- [ ] **Live budget telemetry** — Spend tracked as it burns against the per-run and per-period budget, not reconciled after the invoice.
- [ ] **Loop alerts page a human** — Stalled run, budget breach, repeated-error tripwire, silent stall. Monitored from outside the loop — never ask the loop whether it's looping.
- [ ] **Alerts have runbooks** — Written for the 3 AM reader: how to pause, inspect, resume, or kill this loop; who owns it.

## 10. Evaluation & Self-Improvement

- [ ] **Outcome metric is business-verified** — Completed work that survived review, not "agent said done". Pair it with a counter-metric (quality, incident rate) so the loop can't win cheap.
- [ ] **Golden tasks for the loop** — A small suite of representative work items with expected outcomes; re-run when prompt, model, tools, or harness change → [[ai-evaluation]].
- [ ] **Failed runs become test cases** — Replay real traces from incidents into the eval set; the loop's suite grows from reality.
- [ ] **Tune on evidence** — Budgets, tool surface, stop conditions, model choice tightened from trace data each iteration — the same discipline as any production system.
- [ ] **Watch the watcher** — Periodically re-verify that the loop's success metric still correlates with the real outcome; metrics decay while dashboards stay green.

## 11. Unattended / Enterprise Grade

- [ ] **Runs outlive the laptop** — Durable runtime (server, managed harness, CI). Laptop loops die with the laptop and take the state with them.
- [ ] **Identity and least privilege per agent** — A resolved identity for each loop/worker, scoped tool access. "The loop did it" is not an audit answer.
- [ ] **Secrets never live beside the loop** — No tokens in config files, cron entries, or on the machine; vault/service-account injection at runtime.
- [ ] **Budgets and rate limits enforced by the platform** — Not by prompt requests. Per-agent caps, spend ceilings, rate limits.
- [ ] **Guardrails on every model and tool call** — Input/output checks and pre/post tool-invoke policy hooks, wherever the loop's traffic flows.
- [ ] **Loop inventory exists** — Every loop listed with owner, trigger, cadence, budget, blast radius, and last review date. Automation sprawl nobody can inventory is the pre-incident state.
- [ ] **Change control** — Loop changes (prompt, tools, budget, permissions) get reviewed like any production change; loop config is versioned alongside code.

## 12. Anti-Patterns (auto-fail)

- **Unattended loop with no budget** — a cost incident scheduled for a quiet weekend
- **Self-certifying loop** — the agent grades its own homework; motion without progress
- **Retrying deterministic failures** — the same error storm at 3 AM, forty times
- **Drain loop without an anti-recursion guard** — the hall of mirrors
- **A loop doing a pipeline's job** — 5–10× cost for zero quality gain
- **Stop condition = "model stopped talking"** — the naive stop, shipped by accident
- **Prompts as configuration, loops as afterthought** — the discipline inverted

---

## Scoping — How Much Loop Do You Need?

> Adapted from the tier model in [[ai]] / [[general-agents-driven]]. Climb only when the tier below demonstrably fails.

| Area | 🧪 POC / Spike | 🏠 Internal Tool | 🟢 Production | 🟣 Enterprise / Unattended |
|---|---|---|---|---|
| Fit (§1) | One loop, one task | Scheduled task list | Bounded units + intake queue | Full inventory & change control |
| Bounds (§4) | Timeout & iteration cap | + token budget | + no-progress tripwire, fail-closed | Platform-enforced budgets & rate limits |
| Verification (§5) | Human eyeballs the output | External check per item | Maker/checker separation, artifact trails | Independent adjudicator + audit trail |
| State (§7) | Just files in a repo | Progress file + git | Structured state, checkpointed | Durable runtime, curated memory |
| Humans (§8) | Always in the loop | Approve destructive ops | Approvals queue with SLA | Structural gates + escalation policy |
| Observability (§9) | Terminal scrollback | Basic logs | Full traces + cost telemetry | Alerting with runbooks, live budget |
| Enterprise (§11) | N/A | Runs on your machine | Sandboxed, scoped creds | Durable identity, guardrails, governed |

---

## Quick Pre-Flight (the minimum bar)

Before letting a loop run unattended even once:

- [ ] Trigger, cadence, and intake queue are defined and recorded per run
- [ ] Definition of done is written, external, and verifiable
- [ ] Iteration, token, and wall-clock bounds are set; budget telemetry is live
- [ ] No-progress tripwire and breaker ladder exist; escalation lands with a human
- [ ] Maker/checker separation — the loop doesn't certify its own work
- [ ] Progress/state files are structured, external, and committed
- [ ] Every run is traced; alerts (stalled, overspend, error storm) page a human with a runbook
- [ ] Sandbox + least-privilege credentials; no secrets reachable by the loop
- [ ] Loop inventory entry created: owner, budget, blast radius, review date

---

## Sources

- Chaitanya Giri / Munder Difflin — "Loop Engineering: Designing Agent Loops That Converge" (2026-07): converging vs runaway loops; the outer-loop toolbox — stop conditions, drain loops, retry with backoff, compaction cycles, breaker ladder steer → constrain → stop, no-progress detection, budgets as loop bounds, human gates as exits.
- Anthropic — "Effective harnesses for long-running agents" (2025-11): initializer/coding-agent split, feature list as JSON, incremental progress, clean-state sessions, end-to-end self-verification, getting-bearings ritual.
- Anthropic — "Building effective agents" (2024-12): workflows vs agents, simplicity/transparency/ACI principles, stopping conditions, sandboxed testing, compounding-error caution.
- TrueFoundry — "Loop Engineering at Enterprise Grade: From Laptop Loops to Governed Runtimes" (2026-06) and its fleet-scale sequel (2026-07): the laptop-loop anatomy (scheduled automations, isolated workspaces, skills, MCP connectors, maker/checker sub-agents, state outside the context window); the enterprise gap (durable runtime, identity, budgets, guardrails, per-step traces).
- Addy Osmani — loop-engineering essay naming the anatomy (2026-06, as summarized by TrueFoundry); Boris Cherny / Peter Steinberger on loops as the actual job (June 2026 discourse).
- The vault: [[career-path/18_Applied_AI_Engineer/01_LLM_Application_Patterns/03_Agent_Loops_and_Orchestration|Agent Loops and Orchestration]] — three bounds, fail-closed exits, orchestration ladder, loop governance table.
- Complements: [[graph-engineering]] (topology), [[general-agents-driven]] (agent-driven dev), [[ai]] §4 (agents & tool use), [[claude-driven]] (per-tool steering), [[ai-evaluation]] (evals for loops).
