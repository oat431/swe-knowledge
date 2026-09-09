---
title: "Dynamic Context Assembly"
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
  - context-assembly
---

# Dynamic Context Assembly

> The per-request pipeline that decides which sources enter the prompt, in what order, at what budget — turning the template and its inputs into the concrete payload the model sees.

## Why This Is a Senior Skill

A mid-level engineer concatenates whatever is handy: system prompt, history, retrieved chunks, done. A senior engineer designs assembly as the product's control surface over model behavior: every section is a typed input, ordering is deliberate, budgets are enforced in code, and the assembled payload is reproducible and observable.

Assembly is where most production LLM failures are actually born — a wrong fact retrieved, an instruction buried under history, a critical rule truncated silently. Owning this layer is what separates a system that is debuggable from one that is haunted.

## Core Frameworks

### Source Inventory

| Source | Feeds From | Refresh Rate | Trust Framing |
|--------|------------|--------------|---------------|
| Static template / system contract | Versioned repo ([[04_Prompt_Templates_and_Versioning]]) | Per release | Trusted instructions |
| User input | The request itself | Per turn | Data, delimited |
| Conversation history | Session log, possibly compacted ([[03_Context_Window_Management]]) | Per turn | Model's own prior output |
| Retrieved evidence | Retrieval layer ([[career-path/18_Applied_AI_Engineer/01_LLM_Application_Patterns/00_overview|LLM Application Patterns]]) | Per turn | Data, delimited, ranked |
| Tool results | Function calls | Per call | Data, delimited, schema-shaped |
| Profile / memory | Persistent store | Session start, rarely | Data, authoritative for the user |
| Temporal context | System clock | Per turn | Data: date, timezone, locale |

### Priority Classes and Budget Allocation

| Priority | Sections | Rule When the Budget Binds |
|----------|----------|----------------------------|
| Mandatory | System contract, current user turn, safety rules | Never dropped; truncating these is a bug |
| Critical | Top-ranked evidence, recent history | Truncate with logs, never silently |
| Optional | Low-ranked evidence, old history, profile extras | Drop first, in declared order |

### Ordering Principles

| Principle | What It Means in the Payload |
|-----------|------------------------------|
| Instructions first, data after | The contract opens; evidence follows, framed as data |
| Recency bias exploited | The constraints that must survive are restated near the end |
| Grouped by source, labeled | Each section delimited and named; no interleaving of data and rules |
| Deterministic order | Same inputs → byte-identical assembled prompt |

### Maturity Levels

| Level | Practice |
|-------|----------|
| 1 Concatenation | Sources appended ad hoc across code paths |
| 2 Centralized | One assembly function, but ordering and caps hard-coded |
| 3 Policy-driven | Section schemas, priorities, and budgets expressed as configuration |
| 4 Observable and reproducible | Assembled payload logged and replayable; assembly unit-tested |

## In Practice

**Assembly must be one pure function, not scattered string building.** Every code path that touches the prompt must go through the assembler. A pure function from inputs to payload is unit-testable, replayable, and audit-ready; scattered concatenation is none of those.

**Order sections deliberately — the model reads position as priority.** Put the contract first so it anchors behavior, delimit and label each data section so its status is explicit, and restate the rules that must survive near the end to exploit recency. Interleaving data and instructions invites both confusion and injection.

**Give every section a priority class and an eviction order before the budget binds.** Decide at design time which content dies first: optional sections drop before critical ones get truncated, and mandatory sections are never touched silently. The budget itself is [[03_Context_Window_Management]]'s; the enforcement point is here.

**Log the assembled payload — never let production debugging reconstruct what the model saw.** Record the exact payload, or its hash plus section metadata, with every request. When a user reports a bad answer, the first artifact you inspect is the payload that produced it; guessing from logs that never captured it is archaeology.

**Keep assembly deterministic; move all nondeterminism to the edges.** Retrieval ranking, exemplar selection, and model sampling may be stochastic — but given the same retrieved set, the same history, and the same template version, the assembled prompt must be identical. Determinism is what makes regression tests and replay debugging possible.

**Treat temporal and profile context as data, not as instructions.** "Today is 2026-09-09" and the user's saved preferences are facts to be delimited and labeled, not rules to be trusted. Refreshing them on a schedule keeps the payload current without touching the contract.

## Practical Exercise

Refactor an existing codebase's prompt building into an assembly layer:
1. Find every code path that touches the prompt and list the sources each one injects
2. Define a section schema: name, source, priority class, token cap, and eviction order for each
3. Implement one assembly function with section ordering expressed as explicit configuration
4. Add a rendering test: fixed inputs must produce a byte-identical payload across runs
5. Log the assembled payload (or hash plus section metadata) on every request, then replay one recorded request end to end
6. Simulate a budget overflow and verify the eviction order drops optional sections first and never touches mandatory ones
7. Review the assembly with a teammate, focusing on the ordering rationale

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]: context injection as the base pattern assembly serves
- [[01_Prompt_Design_Principles]]: delimiting and framing every injected section
- [[02_System_and_User_Message_Design]]: role assignment inside the assembled payload
- [[03_Context_Window_Management]]: the budgets and compaction policies assembly enforces
- [[04_Prompt_Templates_and_Versioning]]: the static template is assembly's first input
- [[career-path/18_Applied_AI_Engineer/01_LLM_Application_Patterns/00_overview|LLM Application Patterns]]: retrieval produces the evidence section
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]: payload logging feeds tracing and debugging

## Common Pitfalls

- String building spread across code paths: no single place that knows what the model saw
- Interleaved data and instructions: ambiguity, dilution, and injection surface
- Silent truncation of mandatory sections when the budget binds
- Payload never logged: incidents investigated against a reconstructed, imagined prompt
- Nondeterministic assembly: regression tests and replay debugging become impossible

## Key Takeaways

- Assembly is a pure, deterministic function — the only control surface for what the model sees
- Order encodes priority: contract first, data delimited and labeled, critical rules restated last
- Priority classes and eviction orders are design-time decisions, not runtime improvisations
- The assembled payload is your primary debugging artifact; log it or lose it
- Everything the model reads passes through assembly: if it is not assembled deliberately, it is assembled accidentally
