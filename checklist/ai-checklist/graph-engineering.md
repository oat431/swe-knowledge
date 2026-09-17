# Graph Engineering Checklist

> Designing the **topology** of an agent system: which nodes exist (agents, deterministic functions, routers, joins, tools, human checkpoints), which transitions are permitted, and how state flows between them. The model contributes judgment only where judgment is needed; **the graph decides what happens next.**
> Where this sits: prompt engineering controls one response; context engineering controls what the model sees; [[loop-engineering|loop engineering]] controls one agent's behavior cycle; **graph engineering controls the system the loops live in.** The layers compose — they don't replace each other.
> Siblings: [[loop-engineering]], [[general-agents-driven]] §9 (multi-agent orchestration), [[ai]] §4 (agents & tool use), [[llm-security]] (injection across nodes).
> ⚠️ Status note: "graph engineering" is an **emerging framing (mid-2026), not a settled standard** — the label is contested and collides with knowledge-graph engineering. This checklist covers the practice: graph-based orchestration over heterogeneous nodes.
> Last updated: 2026-09-17 (new file — synthesized from Anthropic's workflow patterns, LangGraph-style runtime semantics, and the July 2026 graphs-vs-loops discourse)

| Layer | Controls | Primitive |
|---|---|---|
| Prompt engineering | one model response | instruction text |
| Context engineering | what the model sees | retrieval, memory, window budget |
| Loop engineering | one agent's behavior cycle | the run — bounds, stops, exits |
| **Graph engineering** | **topology across nodes** | **nodes, edges, state, transitions** |

---

## 1. Fit — Graph, Loop, or Pipeline?

- [ ] **Simplest rung first** — single call → deterministic pipeline → routed flow → bounded loop → graph → multi-agent org. Climb only when the rung below demonstrably fails — evidence, not enthusiasm.
- [ ] **Branching + persistence justifies the graph** — When a workflow branches, retries with different strategies, or must survive across sessions, an explicit graph beats a pile of if-statements inside one loop.
- [ ] **Single-purpose tasks stay loops** — Most agentic work doesn't need a state graph: a cron-triggered, bounded loop with a human review point beats open-ended autonomy when reliability matters more than novelty.
- [ ] **"Graph" disambiguated** — Knowledge graph = how information connects (entities + relationships, GraphRAG-style). Agent graph = how work moves (nodes, edges, state). They coexist in one architecture; don't let the shared noun derail the design review.
- [ ] **Not every node is an agent** — Deterministic functions, routers, joins, validators, and human checkpoints belong in the topology. If you already know the business rule, put it in code — use the model only where judgment is genuinely required.
- [ ] **Manual-first rule accepted** — Prove the shape by hand before adopting a framework. A workflow-design problem can't be fixed by tooling. (See §2.)

## 2. Prove the Shape Before You Automate

- [ ] **One-sentence output defined** — "A one-page recommendation on whether X is worth testing", not "help me research X". Can't write the sentence? It's not ready to become a graph.
- [ ] **5–7 jobs listed** — The jobs a competent human would do to produce that output. Twenty jobs means you haven't found the real structure yet.
- [ ] **Dependencies drawn, not sequences** — Jobs that don't need each other's output run side by side; edges follow data dependencies, not habit.
- [ ] **One human gate where mistakes matter** — Placed before the action whose failure is expensive (send, publish, pay, deploy).
- [ ] **Run the graph manually once** — One prompt per node, a fresh chat for each, outputs handed off by hand. If it doesn't beat a one-shot answer, you have a workflow problem, not an automation problem.
- [ ] **Paper trail before framework** — Planner writes `plan.md`, workers write their files, reviewer writes `review.md`, the final node writes the deliverable. Files make handoffs explicit and runs debuggable.
- [ ] **Automate only what earned it** — After ~3 good manual runs, scaffold the files first; adopt a framework only when persistence, checkpointing, or scale genuinely demands it.
- [ ] **Framework choice recorded with reasons** — LangGraph (checkpoints, human-in-the-loop interrupts), AutoGen GraphFlow (branching/conditional logic), n8n / Make (connectors without code) — pick for the missing capability, and note version churn as a known tax.

## 3. Nodes

- [ ] **Each node is one self-contained unit of work** — A single, clearly defined step. Decomposition is the design work.
- [ ] **Node type declared** — Agent / deterministic function / router / join / tool / human checkpoint. A mixed topology is the point, not a compromise.
- [ ] **Inputs and outputs are contracts** — What it reads from state, what it writes back, schema-validated. Downstream nodes should never parse prose that should have been a field.
- [ ] **Side-effecting nodes are idempotent** — Emails, charges, writes, publishes: safe to re-execute, because retries and interrupt-resumes are guaranteed to happen someday.
- [ ] **Model sized per node** — Routing/formatting gets cheap models; judgment gets the strong model. Node cost is a design variable, not a constant.
- [ ] **Human checkpoints are structural, not improvised** — Defined in the topology at the edges where consequence concentrates — before sensitive tool calls, not as a mid-flight interruption.

## 4. Edges

- [ ] **Edge type chosen deliberately** — Direct, conditional, parallel, looping, error, human-controlled, event-triggered. Each is a design decision with a failure mode; document it on the diagram.
- [ ] **Routing is tested against labeled examples** — And on low confidence it defaults to the broader/safer path. Misrouting fails quietly: a confident, under-researched answer with no error anywhere.
- [ ] **Fan-in semantics defined** — Every join declares how it merges (all / any / quorum) and what happens to late, failed, or cancelled branches.
- [ ] **Cycles have exit criteria** — Any looping edge needs a bound and a progress check, same as any loop (→ [[loop-engineering]] §4). Evaluator–optimizer rounds cap at N, plus a "stopped improving" detector.
- [ ] **Error edges lead somewhere real** — Retry policy, fallback node, or human queue. Never back into the same node with the same input forever.
- [ ] **Transitions are exhaustive** — Every node's possible outcomes map to a defined next step; no silent "falls through to whatever happens next".

## 5. State

- [ ] **State is structured and shared** — Task, artifacts, intermediate results, verdicts. Each step reads and writes only what it needs instead of growing one endless chat history.
- [ ] **Reducers defined for concurrent writes** — When parallel branches update the same field, the merge rule (append, combine, resolve) is explicit. Last-write-wins silently drops work.
- [ ] **State is treated like a database** — Versioned schema, migrations considered for long-lived graphs, PII classified, encrypted at rest, retention defined. It persists; govern it.
- [ ] **State size bounded** — Large payloads go to artifacts (files/objects) with references in state; a checkpoint store full of raw documents gets expensive and slow.

## 6. Persistence & Checkpoints

- [ ] **Durable checkpointer configured** — Resume from the last saved state instead of re-paying for a 40-minute run that crashed at minute 40.
- [ ] **Stable run identity** — `graph_id` / `run_id` / `node_id` propagated through every call, trace, and budget record. Correlation starts with identifiers.
- [ ] **Interrupts for human gates** — Pause for approval, resume the same thread later — and make everything around the interrupt idempotent so a resume can't double-fire side effects.
- [ ] **Replay safety verified** — A replayed or retried node must not duplicate external effects. Test the resume paths, not just the happy paths.
- [ ] **Checkpoint lifecycle owned** — Retention, cleanup, and the cost of the checkpoint store itself.

## 7. The Canonical Patterns (and Their Failure Modes)

- [ ] **Prompt chaining** — Fixed decomposition, gate check between steps, each node gets only what it needs. Failure mode: chaining when the next node needs everything the previous saw — extra latency, no gain.
- [ ] **Routing** — Distinct input classes dispatched to specialized paths. Failure mode: misclassification arriving as a confident shallow answer.
- [ ] **Parallelization (sectioning + voting)** — Independent subtasks in parallel; the same task N times for confidence. Failure mode: parallel branches that secretly depend on each other.
- [ ] **Orchestrator–workers** — Worker count discovered at runtime. Failure mode: the orchestrator "discovers" 40 categories and launches 40 calls — cap workers and spend *before* execution.
- [ ] **Evaluator–optimizer** — Produce → critique → revise against clear criteria. Failure mode: iteration without improvement ("repetition"); needs max rounds + a delta check.
- [ ] **Pattern recorded with its rationale** — The design doc names the pattern, why it fits, and the failure mode being watched. No unnamed patterns in production.

## 8. Reliability

- [ ] **Every node's failure is survivable** — Retry policy, fallback, or graceful degradation per node class; partial results stay consistent.
- [ ] **Timeouts and cancellation** — A stuck node can't wedge the graph; cancellation unwinds in-flight work and compensates side effects.
- [ ] **Concurrency bounded, backpressure handled** — Fan-out caps; slow downstreams slow the fan-out instead of silently dropping work.
- [ ] **Retries at the right layer** — Node vs edge vs run; idempotency respected at each level.
- [ ] **Degradation is defined** — Which nodes may be skipped, and what the deliverable looks like when they are.

## 9. Cost & Performance

- [ ] **The graph's budget is the unit** — Every parallel branch and retried edge multiplies calls; budget the workflow, not just each node.
- [ ] **Per-node attribution** — Cost and latency measured per node from traces; find the expensive node and downgrade, cache, or restructure it.
- [ ] **Caching where safe** — Semantic caching for repeated sub-tasks; deterministic nodes re-run free.
- [ ] **Critical path measured** — Optimize the longest path, not the average node; parallelize the right things.
- [ ] **Model routing per node** — Cheap models for classification/formatting, strong models for judgment; isolate model swaps behind routing so changes don't ripple through the topology.

## 10. Governance & Security

- [ ] **Identity per governed node or caller** — Each independently acting agent/worker has a resolved identity. "The graph did it" is not an audit answer.
- [ ] **Tool scope per node** — Which nodes may reach which tools, enforced at the platform/registry level (scoped MCP servers per node), not by prompting.
- [ ] **Guardrails on model and tool traffic** — Input/output checks and pre/post tool-invocation hooks. Cross-node prompt injection is real: a poisoned artifact can carry instructions to the next node → [[llm-security]].
- [ ] **Budgets and rate limits mapped to nodes** — Enforced through accounts or propagated identifiers, so a runaway node is visible and stoppable.
- [ ] **Approval checkpoints guard sensitive actions** — Before destructive or external-effect tool calls, at the exact edges where consequence concentrates.
- [ ] **Audit trail end-to-end** — Who ran what, under which identity, with which policy decisions and outcomes — reconstructable after the fact.

## 11. Observability & Operations

- [ ] **The orchestrator records the actual runtime graph** — Which nodes ran, retries, spawns, mutations — not just the intended diagram. Intended vs actual is where debugging starts.
- [ ] **Traces correlate across layers** — Workflow trace ↔ model/tool records via stable ids: cost, latency, and policy outcomes per node.
- [ ] **Runs are visualizable** — You can look at the DAG and see exactly what path a run took. That inspectability is the main thing a graph buys you over an ad-hoc loop.
- [ ] **Structural alerts** — Fan-out explosions, runaway spawning, node error storms, budget breaches, orphaned runs.
- [ ] **Runbook per graph** — What it does, how to pause, resume, abort safely, and who owns it — written for the 3 AM reader.

## 12. The Meta-Layer — Graphs of Loops

> Don't just improve the loops: design the **graph of loops**. Four predictable failures at scale — **Goodhart** (the metric detaches from what it meant), **blindness upward** (no loop can question its own target), **conflict** (independently-built loops fight), **measurement decay** (nobody watches the watcher).

- [ ] **Metrics never travel alone** — Every optimized metric paired with a counter-metric and anchored to an external ground truth (revenue, retention, physical counts). Pairing raises the cost of gaming.
- [ ] **References have owners** — Targets and thresholds are owned by a slower, higher-level loop; fast loops can't silently rewrite their own objectives.
- [ ] **Speeds are separated** — Per-run, weekly, and quarterly cadences don't override each other; fast loops escalate signals upward instead of thrashing policy.
- [ ] **Some loops are frozen on purpose** — Held-out eval sets, safety/legal constraints, and ground-truth checks are read-only to the system — precisely because the optimizer would be tempted to weaken them.
- [ ] **Anchors are exogenous** — The graph is forbidden to rewrite its own anchors. A graph without anchors is an elaborate echo chamber.
- [ ] **Work graph vs improvement graph kept distinct** — What the system does (tasks, tools, artifacts) vs how it decides to change itself (eval, governance, audit loops). Both documented.

## 13. Anti-Patterns (auto-fail)

- **Graph for a single-purpose task** — complexity tax with no dividend; the cron loop + human gate would have shipped already
- **Unbounded fan-out** — orchestrator spawns workers proportional to what it "discovered"; rate-limit + budget incident
- **Last-write-wins on parallel merge** — silent data loss behind a green dashboard
- **Looping edge without exit criteria** — evaluator–optimizer iterating without improving
- **Non-idempotent side effects around retries/interrupts** — double sends, double charges after resume
- **Guardrails assumed, not enforced** — "it can't reach that tool" without registry-level scoping
- **"It's basically just LangGraph"** — framework adopted before the shape was proven; now the abstraction hides the bug
- **State as an ever-growing chat log** — no structured state means no checkpointing, no debugging, no reuse

---

## Scoping — How Much Graph Do You Need?

> Adapted from the tier model in [[ai]] / [[general-agents-driven]]. Most tiers don't need a graph yet — and that's the point.

| Area | 🧪 POC / Spike | 🏠 Internal Tool | 🟢 Production | 🟣 Enterprise |
|---|---|---|---|---|
| Fit (§1) | Prose pipeline is fine | 5–7 jobs, drawn first | Explicit graph, patterns named | Full topology + ownership |
| Design (§2) | Sketch on paper | Manual runs, paper trail | Files + framework where needed | Reviewed design, versioned graph |
| State (§5) | Local files | Shared state file | Schema'd state + reducers | Governed, versioned, classified |
| Persistence (§6) | None | Restart from scratch | Checkpoints + interrupts | Durable runtime, replay tested |
| Security (§10) | N/A | Scoped creds | Identity + tool scope + guardrails | Enforced registry, audit trail |
| Observability (§11) | Terminal logs | Run history visible | Traces + per-node cost | Correlated telemetry + runbooks |
| Meta-layer (§12) | N/A | — | Counter-metrics | Anchors, cadence separation, frozen loops |

---

## Quick Pre-Flight

Before automating a graph:

- [ ] Output defined in one sentence; 5–7 jobs listed; dependencies drawn as edges
- [ ] Human gate placed before the consequential action
- [ ] Manual run beat the one-shot baseline (~3 runs); paper trail produced
- [ ] Nodes typed; contracts schema-validated; side effects idempotent
- [ ] Fan-in reducers defined; cycles bounded with exit criteria
- [ ] Checkpointer durable; run identifiers propagated; resume tested
- [ ] Budget per graph and per node; fan-out capped; structural alerts wired
- [ ] Identity, tool scope, guardrails, approvals — enforced at the platform layer
- [ ] Runbook + owner; the runtime graph is recorded for every run

---

## Sources

- Nhu Hoang / Towards Data Science — "Graph Engineering for AI Agents: From Prompts and Loops to Workflows" (2026-09): nodes/edges/state, the 7 edge types, reducers, checkpoints/interrupts, five orchestration patterns + failure modes, the manual-first design procedure, framework comparison.
- Anthropic — "Building effective agents" (2024-12): prompt chaining, routing, parallelization, orchestrator–workers, evaluator–optimizer; workflow-vs-agent distinction; simplicity/transparency/ACI principles.
- TrueFoundry — "Graph Engineering for Multi-Agent Systems: Architecture, Governance, and Observability" (2026-07): the enterprise seven questions (identity, id propagation, runtime graph recording, correlation, budgets, approval checkpoints, model isolation); heterogeneous node types; "the layers compose rather than supersede" with loop engineering.
- Eigent — "Graph Engineering for AI Agents: Beyond Single Feedback Loops" (2026-07): the four scale failures (Goodhart, blindness upward, conflict, measurement decay); metrics + counter-metrics + anchors; owners for references; cadence separation; deliberately frozen loops; work graph vs improvement graph.
- explainx.ai — "Graphs vs. Loops: The Agentic AI Orchestration Debate, Explained" (2026-07): both patterns are correct for different task shapes; "state machines rediscovered"; knowledge graph vs agent graph disambiguation; Linear Loops as cron-triggered bounded loop with human review.
- Ken Huang — "Graph Engineering for Agentic AI Systems" (book, 2026): declaring how an agent system is allowed to run — specialized nodes, permitted edges, checkpoints.
- The vault: [[career-path/18_Applied_AI_Engineer/01_LLM_Application_Patterns/06_Pattern_Selection_and_Fallback_Design|Pattern Selection and Fallback Design]] — choosing the right rung; [[career-path/18_Applied_AI_Engineer/01_LLM_Application_Patterns/03_Agent_Loops_and_Orchestration|Agent Loops and Orchestration]].
- Complements: [[loop-engineering]] (per-node execution discipline), [[general-agents-driven]] §9 (multi-agent orchestration), [[ai]] §4, [[llm-security]].
