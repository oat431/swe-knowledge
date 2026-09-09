---
title: "Few-Shot and Chain-of-Thought Techniques"
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
  - few-shot
  - chain-of-thought
---

# Few-Shot and Chain-of-Thought Techniques

> Teaching the model by demonstration: few-shot examples show what good behavior looks like, and chain-of-thought prompts elicit the reasoning steps that make hard answers reliable.

## Why This Is a Senior Skill

A mid-level engineer adds a few examples when a prompt fails and calls it done. A senior engineer treats examples as a curated dataset with the same hygiene as training data: they encode policy, they drift, they consume budget, and they must be selected for the request at hand — not for the demo they were written on.

Reasoning is a tool with a cost, not a default. Knowing when to elicit it, when to show it, and when to skip it entirely is the difference between a prompt suite and a token furnace.

## Core Frameworks

### Few-Shot

| What to Do | Why |
|------------|-----|
| Use 2–3 high-quality examples before adding more | Small, well-chosen exemplars beat large noisy sets; extra tokens dilute attention |
| Mirror the target format exactly, including refusals | The model imitates what it sees, including "I can't answer that" |
| Show failure handling as an example | A refusal exemplar teaches boundaries more durably than a rule |
| Select examples dynamically per request | Retrieval over an exemplar store beats one static set for varied traffic |
| Re-review examples at every template release | Examples silently encode old policy; they are part of the contract ([[04_Prompt_Templates_and_Versioning]]) |

### Chain-of-Thought

| Technique | What It Does | Cost |
|-----------|--------------|------|
| Zero-shot CoT ("think step by step") | One sentence elicits intermediate reasoning | Small token cost; occasionally low-quality reasoning |
| Few-shot CoT | Demonstrates the reasoning format to follow | Exemplar tokens plus longer outputs |
| Self-consistency | Sample N reasonings, take the majority answer | N× tokens and latency |
| Reasoning-native models | Model reasons internally, exposed as a reasoning-effort dial | Provider-priced compute; no prompt scaffolding needed |
| ReAct (reason-act loops) | Interleaves reasoning with tool calls | Multi-call latency; the pattern lives in [[career-path/18_Applied_AI_Engineer/01_LLM_Application_Patterns/00_overview|LLM Application Patterns]] |

### Choosing the Technique

| Task Shape | Use |
|------------|-----|
| Format-sensitive but simple (extraction, classification) | Zero-shot with an explicit output schema |
| Underspecified style or edge-case behavior | Few-shot exemplars |
| Multi-step reasoning (math, planning, comparison) | CoT, or a reasoning-native model |
| Multiple valid approaches, only the answer matters | Self-consistency |
| Needs tool results mid-reasoning | ReAct agent loop |

### Maturity Levels

| Level | Practice |
|-------|----------|
| 1 Occasional | Examples added ad hoc when a demo fails |
| 2 Curated | Fixed exemplar set reviewed with the template |
| 3 Dynamic | Exemplars selected per request; reasoning used where it pays |
| 4 Measured | Technique choice justified by eval deltas and cost data |

## In Practice

**Curate examples like training data: representative, diverse, and versioned.** A few exemplars pulled from one happy-path demo encode that demo's blind spots. Cover refusals, boundary cases, and the formats that matter most, and re-review them at every template release.

**Select exemplars per request instead of shipping one static set.** Static examples underperform when traffic is heterogeneous; embedding and retrieving exemplars by similarity to the incoming request lets one template serve many behaviors. The retrieval machinery is area 01's; the decision that examples are data worth retrieving is this area's.

**Show the refusal, not just the success.** An exemplar where the model declines — "I don't have that information" — teaches scope boundaries more durably than a rule, because the model sees exactly how to say no while staying in persona.

**Elicit reasoning only when the task genuinely needs it.** A price lookup or schema-fill needs an output contract, not "think step by step". Reasoning on every call burns tokens and latency and can even introduce errors through wrong intermediate steps. Match the technique to the task shape.

**Prefer reasoning-native models for genuinely hard multi-step work.** Where a base model needs an elaborate CoT scaffold, a reasoning model internalizes the same effect behind an effort dial. The senior move is knowing which lever your provider actually gives you — model choice itself belongs to [[career-path/18_Applied_AI_Engineer/05_Model_and_Inference_Operations/00_overview|Model and Inference Operations]].

**Measure the marginal value of each technique before adopting it.** Every technique costs tokens, latency, or both; the only honest justification is an eval delta or a production metric ([[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]). If self-consistency does not lift success rate, drop it.

## Practical Exercise

Benchmark technique choices on one real task:
1. Pick a production task and classify its shape: simple lookup, format-sensitive, or multi-step
2. Build a 20-case test set from real traffic with expected outputs
3. Run the current prompt as a baseline and record success rate and tokens per call
4. Add 2–3 curated exemplars — including one refusal — and re-run
5. Add zero-shot CoT and re-run; compare success, tokens, and latency against the baseline
6. If the task is multi-step, run self-consistency with N=3 and check whether agreement improves outcomes
7. Write a one-paragraph decision on which technique wins at what cost, and codify the choice in the template

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/11_Prompt_Engineering_and_Security]]: few-shot defense examples as part of prompt hardening
- [[01_Prompt_Design_Principles]]: rules and examples are complementary instructions
- [[04_Prompt_Templates_and_Versioning]]: exemplars are versioned artifacts inside the template
- [[03_Context_Window_Management]]: exemplars and reasoning steps consume the budget
- [[career-path/18_Applied_AI_Engineer/01_LLM_Application_Patterns/00_overview|LLM Application Patterns]]: ReAct loops and dynamic exemplar retrieval
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]: proving the technique pays for itself

## Common Pitfalls

- Static exemplars written for the demo, drifting away from real traffic
- Teaching only successes: the model never learns how to refuse
- CoT on trivial tasks: tokens and latency spent, occasional wrong-reasoning regressions introduced
- Using a base model's verbose reasoning where a reasoning model would do it internally, cheaper and better
- Adopting self-consistency or CoT because it is fashionable, not because the eval says so

## Key Takeaways

- Examples are data: curated, diverse, versioned, and selected per request
- Show the refusal — boundaries are taught better than they are stated
- Reasoning is a tool with a cost, not a default; match technique to task shape
- Reasoning-native models replace prompt scaffolding for hard tasks
- Every technique must earn its tokens with an eval delta or a production metric
