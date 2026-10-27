---
title: "Agent Memory and State"
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
  - agent-memory
---

# Agent Memory and State

> Giving agents memory and durable state so long-running work survives context limits, restarts, and interruptions without corrupting later behavior.

## Why This Is a Senior Skill

A mid-level engineer stuffs the last fifty messages into the context window and calls it memory, then watches quality collapse once the session runs long, the process restarts, or an old wrong answer keeps getting re-read as truth. A senior engineer separates concerns deliberately: working context is transient and curated, episodic memory is retrieved on demand, semantic memory is vetted like any knowledge base, and task state is explicit, checkpointed, and idempotent to replay. Each kind of memory has a different failure mode, and treating them as one "history" blob mixes all of them together.

The senior challenge is memory hygiene over time. Memory is a write path for everything the agent reads, including untrusted content, so staleness, contradictions, and poisoning are engineering problems with engineering controls — not things a longer context window solves.

## Core Frameworks

### Memory Taxonomy

| Type | What Lives There | Lifetime | Typical Store | Failure When Wrong |
|------|------------------|----------|---------------|--------------------|
| Working context | Current task, recent turns, active plan | One request or episode | Context window | Overflow; attention diluted by stale turns |
| Episodic memory | What happened: past sessions, decisions, outcomes | Days to months | Session store or vector index over transcripts | Wrong recollection steers the current task |
| Semantic memory | Vetted facts and knowledge: preferences, policies, entity facts | Long-lived | Knowledge base or curated store with retrieval | Stale or false facts persist and compound |
| Task state | Checkpoint of an in-flight task: step, inputs, partial results, budgets spent | Until task completes | Durable state store keyed by task ID | Lost work on restart; duplicate side effects on replay |

### Retrieval-Based Memory vs State Stores

| Dimension | Retrieval-Based Memory | Plain State Store |
|-----------|------------------------|-------------------|
| Access pattern | Query by similarity or recency; top-k into context | Exact lookup by task/session key |
| Good for | Open-ended recall: "what did we decide last time" | Precise resume: "continue at step 4 with these results" |
| Failure mode | Retrieves irrelevant or contradictory memories | No recall ability at all beyond known keys |
| Hygiene burden | High: relevance, staleness, deduplication | Low: versioning and schema only |
| Trust boundary | Retrieved text can be poisoned; treat as data, never instructions | Values are written by the system, not by the world |

### Session, Checkpoint, and Replay Design

| Concern | Design Choice | Why It Matters |
|---------|---------------|----------------|
| Session model | Session groups turns; thread groups related sessions; both keyed and versioned | Lets memory scope to a user, task, or project instead of one global blob |
| Checkpointing | Persist task state (step, inputs, partial results, budgets) after every side-effecting step | Restart resumes instead of redoing work |
| Idempotent replay | Every step can be re-run safely: dedupe keys on side effects, treat results as content-addressed | Crashes and retries stop causing double charges and double writes |
| Interruption | Explicit pause point with resumable state, not a suspended process | Long tasks survive deploys and timeouts |
| Compaction | Summarize old working context into episodic notes with provenance | Keeps the window usable without losing the trail |

### Memory Hygiene

| Threat | How It Appears | Control |
|--------|----------------|---------|
| Staleness | Old facts contradict current reality and keep getting retrieved | Timestamps, TTLs, refresh jobs; prefer recent evidence |
| Contradiction | Two memories disagree; the agent picks whichever surfaced first | Single source of truth per fact type; conflict detection on write |
| Poisoning | Untrusted content (web pages, tool outputs, user uploads) is stored then trusted later | Never store untrusted text as instructions; provenance tags; write filters (see [[career-path/18_Applied_AI_Engineer/04_AI_Security_and_Guardrails/02_Prompt_Injection_Defense|Prompt Injection Defense]]) |
| Over-storage | Everything is remembered; retrieval drowns in noise | Curate on write; dedupe; summarize instead of accumulating |
| Memory as database | Business-critical facts live only in agent memory | Keep authoritative data in real tools/databases; memory holds pointers and working notes |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Turn or event arrives"] --> B["Working context updated"]
    B --> C["Side-effecting step executes"]
    C --> D["Checkpoint task state"]
    D --> E["Compact into episodic notes"]
    E --> F["Tag provenance and filter untrusted text"]
    F --> G["Semantic store with TTL and reconciliation"]
    B --> H["Retrieval pulls episodic and semantic memory"]
    H --> I["Curated context for the next step"]
```

## In Practice

**Separate working context, episodic memory, semantic memory, and task state explicitly.** They have different lifetimes, stores, and failure modes, so one shared history blob guarantees you get all four failure modes at once. Name each layer, decide what may enter it, and decide what gets evicted.

**Persist task state after every side-effecting step and make replay idempotent.** Long-running work will be interrupted by timeouts, deploys, and crashes; the question is whether restart resumes or doubles the work. Key side effects by a dedupe token, checkpoint the step index and budgets spent, and prove with a forced kill test that resume neither loses progress nor repeats writes.

**Retrieve memory on demand instead of loading history wholesale.** A relevance-filtered top-k of episodic notes beats a transcript dump: the window stays focused and old wrong turns stop steering new ones. Keep retrieval cheap and biased toward recency and task relevance.

**Treat everything read from the world as data, never as instructions.** Tool outputs, documents, and web pages enter memory through the same door as trusted notes, and a poisoned memory is an attack that keeps working after the original session ends. Tag provenance on write, filter on read, and quarantine anything that smells like embedded instructions.

**Keep authoritative facts in tools and databases, not in agent memory.** Memory is for working notes, task state, and curated experience; account balances, entitlements, and policies live in systems of record that the agent queries. When memory and the database disagree, the database wins, and the memory gets corrected.

**Run memory hygiene as scheduled work: dedupe, expire, and reconcile contradictions.** Memory degrades silently the way any append-only log does. Review what is being retrieved in production traces, retire what no longer earns its place, and treat contradictions as bugs with an owner.

## Practical Exercise

1. Pick a long-running agent task (multi-hour research, migration planning, multi-session customer case) and list the four memory types with what each must hold for that task.
2. Define the task-state schema: step index, inputs, partial results, budgets spent, side-effect dedupe keys.
3. Implement checkpointing after each side-effecting step and force-kill the process mid-task; verify resume continues without repeating side effects.
4. Build a small episodic store (even a file-based one) with provenance and timestamps; wire relevance-and-recency retrieval into the loop.
5. Inject a poisoned document containing embedded instructions into the store; verify it is stored as data and cannot steer later behavior.
6. Write one stale fact into semantic memory; verify a refresh or TTL path corrects it instead of the agent trusting it forever.
7. Document the boundary: which facts must live in a system of record rather than memory, and how the agent fetches them.

## Practical Applications

- [ ] Memory layers are named and separated: working context, episodic, semantic, task state
- [ ] Task state is checkpointed after every side-effecting step and survives restarts
- [ ] Replay is idempotent: side effects carry dedupe keys; retries cannot double-write
- [ ] Retrieved memories carry provenance and timestamps; staleness has a TTL or refresh path
- [ ] Untrusted content is stored as data with provenance tags, never as instructions
- [ ] Contradictions between memories are detected on write and resolved against systems of record
- [ ] Authoritative business facts live in databases; memory holds pointers and working notes
- [ ] Memory hygiene (dedupe, expiry, reconciliation) is scheduled and owned, not aspirational

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Treating full conversation history as memory | Context overflows; stale turns dilute and mislead | Layered memory with retrieval and compaction |
| No checkpointing of in-flight tasks | A restart loses hours of work or repeats side effects | Checkpoint after each side-effecting step; idempotent replay |
| Storing untrusted content as trusted memory | A single poisoned document steers every later session | Provenance tags, write filters, treat retrieved text as data |
| Memory as the system of record | Facts drift from reality with no reconciliation path | Authoritative data in databases; memory holds pointers and notes |
| Append-only memory forever | Retrieval quality decays into noise | Curate on write; dedupe, expire, summarize |
| Silent contradictions | The agent acts on whichever stale fact surfaced first | Conflict detection on write; resolve against the system of record |

## Knowledge Connections

- [[career-path/18_Applied_AI_Engineer/02_Context_and_Prompt_Engineering/03_Context_Window_Management|Context Window Management]]: working-context mechanics underneath the memory layers
- [[career-path/18_Applied_AI_Engineer/04_AI_Security_and_Guardrails/02_Prompt_Injection_Defense|Prompt Injection Defense]]: defending the memory write path from poisoning
- [[04_Agent_Evaluation_and_Trajectories]]: long-horizon episodes are where memory failures surface
- [[career-path/18_Applied_AI_Engineer/01_LLM_Application_Patterns/03_Agent_Loops_and_Orchestration|Agent Loops and Orchestration]]: where checkpoints and budgets attach
- [[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]: production patterns background for stateful LLM systems

## Sources

- *Designing Data-Intensive Applications* (Martin Kleppmann) — durable state, idempotence, and replay reasoning
- *AI Engineering* (Chip Huyen) — memory and context patterns for foundation-model applications
- *Designing Machine Learning Systems* (Chip Huyen) — production state and data lifecycle discipline
