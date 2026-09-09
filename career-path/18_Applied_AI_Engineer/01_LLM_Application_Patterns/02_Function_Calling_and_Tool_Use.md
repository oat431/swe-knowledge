---
title: "Function Calling and Tool Use"
note_type: capability-topic
capability_area: llm-application-patterns
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - tool-use
  - function-calling
---

# Function Calling and Tool Use

> The pattern where the model emits a structured call to a developer-provided function instead of a plain answer; your code executes the function and returns the result for the model to synthesize.

## Why This Is a Senior Skill

A mid-level engineer registers a few tools in a framework and trusts the model to use them. A senior engineer designs tool contracts: schemas precise enough that malformed arguments are nearly impossible, descriptions that delimit exactly when a tool applies, validation at the boundary, bounded side effects, idempotency for writes, and audit logging. The senior also knows when tool use is unnecessary — a deterministic, pre-computed lookup injected into context beats a model-mediated call every time.

The senior challenge is this: the model is an untrusted caller of your internal APIs. Every property of a safe API — validation, least privilege, rate limits, idempotency, observability — must be rebuilt at the tool boundary, because a wrong call from a human is a bug, but a wrong call from a model is the expected failure mode.

## Core Frameworks

### The Call Cycle

| Step | What Happens | Failure Modes |
|------|-------------|---------------|
| 1. Schema definition | Tools declared with name, description, JSON schema for arguments | Ambiguous schema → malformed args |
| 2. Selection | Model decides whether and which tool to call | Wrong tool, wrong time |
| 3. Emission | Model outputs name plus structured arguments | Hallucinated arguments |
| 4. Execution | Your code runs the tool with those arguments | Side effects on bad input |
| 5. Result return | Tool output goes back into context | Oversized results, truncation |
| 6. Synthesis | Model produces the final answer | Misreading the result |

### When Tool Use Is the Right Pattern

| Situation | Pattern | Why |
|-----------|---------|-----|
| Data is deterministic and known at build time | Context injection | A lookup table in the prompt is cheaper and more reliable |
| Real-time, per-request data (price, stock, status) | Tool call | Only the source system has current truth |
| Side-effecting action (create order, send email) | Tool call plus confirmation gate | Model proposes, human or policy confirms |
| Unstructured knowledge over a corpus | RAG, not a tool | Retrieval beats forcing a model to pick API parameters |

### Tool Contract Design

| Element | What to Do | Why |
|---------|-----------|-----|
| Name and description | Say exactly what the tool does and when to use it, with counterexamples | Selection quality depends on the boundary being legible |
| Parameter schema | Exact types, enums over free text, constraints (min/max, patterns), required fields | Prevents malformed calls at emission time |
| Return shape | Small, flat, string-safe result; truncate large payloads | Oversized results blow the context budget |
| Errors | Return errors as structured results (e.g., `{"error": "insufficient_stock"}`) | The model recovers from a result it can read |

### Execution Safety

| Layer | Measure |
|-------|---------|
| Validation | Pydantic/JSON Schema check on every argument before execution — never trust model-chosen values |
| Least privilege | Read-only tools by default; writes behind policy gates |
| Idempotency | Writes carry idempotency keys; retries must not double-charge |
| Rate limits | Per-user, per-session caps on tool invocations |
| Audit | Log tool name, arguments, result, and caller context for every invocation |

### Tool Orchestration Frameworks (examples, not commitments)

| Framework | Strengths | Weaknesses |
|-----------|-----------|------------|
| LangChain tools | Wide integration catalog | Abstraction weight; opinionated |
| LlamaIndex FunctionCallingAgent | Tight RAG plus tool coupling | Ecosystem smaller outside retrieval |
| OpenAI/Anthropic native tool schemas | Provider-native reliability | Lock-in to one provider's dialect |
| MCP (Model Context Protocol) | Standard tool surface across apps | Young; security model still maturing |

## In Practice

**A tool's schema is its API contract with the model — specify types, enums, and constraints exactly.** The model infers what a good call looks like from the schema; every degree of freedom is a place to hallucinate. Prefer enums over free-text fields, mark required fields, and constrain string lengths and numeric ranges. Schema precision at definition time is the cheapest reliability win the pattern offers.

**Validate every argument in code before execution, no matter how constrained the schema.** Schema-constrained decoding reduces malformed calls; it does not guarantee correct ones — the model can still emit an out-of-range date or a wrong-but-valid ID. Validation is the last line between a model mistake and a real system effect, so treat every argument as hostile input.

**Keep writes behind confirmation and make them idempotent.** A charge, an order, or a message send should never fire on model intent alone: require human or policy confirmation for irreversible actions, and design the tool so a retried call is harmless. If a retry can double the effect, the tool is broken, not the model.

**Return tool errors as structured results, not exceptions.** If the execution layer throws, the model sees a crash and may retry the same bad call or fabricate an outcome. Returning `{"error": "..."}` lets the model adjust its next step, which is the whole point of the pattern — a readable outcome is information; an exception is noise.

**Bound what each tool can do and how often it can be called.** Least privilege shrinks the blast radius of a wrong call or an injected one; per-user rate limits stop a looping caller from hammering internal systems. This is the tool-use side of the guardrail story, fully developed in the security area — the pattern must leave room for those defenses.

**Log every invocation with arguments, result, and trace context.** Tool calls are where the model touches the real world; when a customer says "your system ordered twice," the audit log is the only ground truth. Trace IDs tie each call back to the conversation and the model version that made it, so incidents are reconstructable instead of anecdotal.

## Practical Exercise

1. Pick a real internal API (e.g., order status lookup or calendar read) and write its tool schema: name, description, JSON Schema for arguments with enums and constraints.
2. Write the description deliberately: what the tool does, when to use it, and when NOT to use it.
3. Implement the execution layer with Pydantic validation before the API call.
4. Run 10 adversarial queries (unexpected units, wrong ID formats, injection-flavored arguments) and record whether validation catches each.
5. Change the execution layer to return errors as structured results; verify the model recovers from a simulated `{"error": "not_found"}`.
6. Add idempotency for writes, per-user rate limits, and an audit log line per invocation.
7. Compare against the deterministic alternative: could any of these tools be replaced by pre-computed context injection?

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]: function-calling fundamentals and safety considerations
- [[computing-foundation-note/Artificial_Intelligence/11_Prompt_Engineering_and_Security]]: injection risks that flow through the tool boundary
- [[04_Structured_Outputs_and_Schema_Enforcement]]: the same schema discipline applied to outputs
- [[03_Agent_Loops_and_Orchestration]]: tool use is the loop's action primitive
- [[career-path/18_Applied_AI_Engineer/04_AI_Security_and_Guardrails/00_overview|AI Security and Guardrails]]: tool-boundary defense in depth

## Common Pitfalls

- Calling a tool for data that could have been injected deterministically at build time
- Treating schema-constrained output as validated output: constraint and validation are different layers
- Letting exceptions surface to the model instead of structured error results
- Write tools without idempotency or confirmation gates
- Unbounded result payloads that silently consume the context window

## Key Takeaways

- The model is an untrusted caller: validate, least-privilege, idempotent, rate-limited, audited
- Schema precision at definition time is the cheapest reliability win available
- Errors as results, not exceptions: the model recovers from readable outcomes
- Every tool call is a decision the model makes — make that decision legible with a precise contract
- When data is deterministic and pre-computable, a tool call is over-engineering
