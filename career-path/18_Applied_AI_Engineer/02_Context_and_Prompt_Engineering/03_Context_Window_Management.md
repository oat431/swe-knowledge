---
title: "Context Window Management"
note_type: capability-topic
capability_area: context-and-prompt-engineering
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - prompt-engineering
  - context-window
---

# Context Window Management

> Treating the context window as a scarce, billable resource: allocating budgets across sections, compacting history, and keeping the model's working memory focused.

## Why This Is a Senior Skill

A mid-level engineer treats a 128K-token window as permission to send everything. A senior engineer treats the window as a budget whose overspend shows up as cost, latency, and degraded attention: models attend unevenly across long contexts, the middle of a stuffed window is where facts get missed, and every irrelevant token dilutes the instructions that matter.

The senior task is designing the budget and its enforcement before traffic arrives: which sections may grow, which are capped, what gets evicted or compacted when limits bind, and how memory survives a session.

## Core Frameworks

### Budget Allocation

| Section | Typical Share | Compaction Policy When Over Budget |
|---------|---------------|-------------------------------------|
| System contract | Fixed, small (5–10%) | Never truncated silently; a change is a release ([[04_Prompt_Templates_and_Versioning]]) |
| Current user turn | Uncapped within reason | Truncate with notice; reject pathological length |
| Conversation history | 20–40% | Summarize oldest turns or evict by importance |
| Retrieved evidence | 30–50% | Trim per document; drop lowest-ranked first |
| Tool results | Variable, capped | Truncate or summarize; preserve schemas |
| Profile / memory | Small, fixed | Persistent across sessions; refreshed rarely |

### Truncation vs Summarization vs Eviction

| Strategy | How It Works | Best For | Risk |
|----------|--------------|----------|------|
| Recency truncation | Keep last N turns, drop the rest | Fast-moving chat where old turns are spent | Loses earlier commitments |
| Rolling summarization | Periodically summarize old turns into a fixed block | Long sessions needing continuity | Summary errors compound over time |
| Hierarchical compaction | Summaries of summaries at multiple levels | Very long histories and agent transcripts | Information loss at every level |
| Importance eviction | Score turns, keep only high-value ones | Tasks with clear milestones | Scoring is itself a model call |
| Sliding window over evidence | Keep the best-ranked chunk per topic | RAG-heavy calls | Depends on ranking quality ([[career-path/18_Applied_AI_Engineer/01_LLM_Application_Patterns/00_overview|LLM Application Patterns]]) |

### Maturity Levels

| Level | Practice |
|-------|----------|
| 1 None | Context grows until the API errors |
| 2 Hard caps | Naive truncation when a constant token count is exceeded |
| 3 Section budgets | Per-section caps with a deliberate eviction order |
| 4 Managed memory | Compaction pipeline plus persistent memory; budget enforced and monitored |

## In Practice

**Budgets are per section, not one global number.** A global cap says nothing about which content dies first when it binds. Assign each section a ceiling and an eviction order, and enforce them in the assembler rather than hoping the prompt stays short ([[06_Dynamic_Context_Assembly]]).

**Bigger windows do not mean better attention — budget for quality, not just fit.** Models miss details in the middle of long contexts, so a tight, well-ordered payload usually beats a comprehensive one. The right question is "what must the model see at once", not "what fits".

**Compaction is a pipeline stage with its own tests, not a prompt hack.** A summarization step that rewrites history can silently drop commitments — prices quoted, decisions made, constraints agreed. Version the compaction prompt, test it on recorded transcripts, and keep the raw log for debugging. When compaction fails, the failure compounds across every subsequent turn.

**Make truncation explicit and observable.** When a section is cut, log what was removed and why, and prefer cutting evidence over cutting rules. Silent truncation is the classic debugging trap: the engineer reconstructs a prompt that was never actually sent.

**Persist memory deliberately — session state is not memory.** Decide what crosses sessions: user preferences, standing constraints, durable facts. Refresh it on a schedule; stale memory is worse than no memory because the model will trust it.

**Measure the cost of the budget, then tune it.** Every token beyond what the task needs is billed on every call and every cache miss. Track tokens-per-request and cost-per-request as first-class metrics. The dashboards belong to [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]; the budget that produces those numbers is this area's.

## Practical Exercise

Build a budget and compaction policy for a multi-turn product:
1. Dump 20 real conversations and record token counts per section
2. Define per-section budgets based on the 95th percentile, not the mean
3. Choose an eviction order and a compaction strategy for history (summarize vs evict) and justify the choice
4. Implement enforcement in code with structured logs of every truncation and compaction
5. Replay five of the longest recorded conversations through the policy and compare behavior before and after compaction
6. Define what persists across sessions and write the refresh rule
7. Add tokens-per-request and cost-per-request to the dashboard and alert on the 99th percentile

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]: the context window as the scaling constraint that motivates RAG
- [[06_Dynamic_Context_Assembly]]: budgets are enforced where context is assembled
- [[02_System_and_User_Message_Design]]: assistant history as managed state
- [[04_Prompt_Templates_and_Versioning]]: compaction prompts are prompts — version them
- [[career-path/18_Applied_AI_Engineer/01_LLM_Application_Patterns/00_overview|LLM Application Patterns]]: retrieval decides what enters the evidence section
- [[career-path/18_Applied_AI_Engineer/05_Model_and_Inference_Operations/00_overview|Model and Inference Operations]]: window economics — caching and pricing of what you send

## Common Pitfalls

- Sending everything because the window allows it: cost, latency, and lost-in-the-middle degradation
- One global cap with no eviction order: the model loses instructions before evidence
- Silent truncation: production debugging against a prompt that was never sent
- Compaction prompts edited without tests: history corruption compounds every turn
- Confusing session state with memory: preferences vanish or stale facts persist

## Key Takeaways

- The window is a budget, not a size limit; budgets are per section with an explicit eviction order
- Long windows degrade attention in the middle — send only what must be seen at once
- Compaction is a versioned, tested pipeline stage, and truncation must always be logged
- Memory is deliberate persistence across sessions, not whatever the session left behind
- Cost and token metrics make the budget real; without them the budget is a convention
