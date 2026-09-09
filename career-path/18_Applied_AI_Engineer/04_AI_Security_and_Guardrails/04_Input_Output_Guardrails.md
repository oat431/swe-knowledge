---
title: "Input Output Guardrails"
note_type: capability-topic
capability_area: ai-security-and-guardrails
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - guardrails
  - input-validation
  - output-filtering
---

# Input Output Guardrails

> Runtime policy checks on every request and response — the enforcement layer that decides what reaches the model and what reaches the user, engineered as a testable system rather than a pile of if-statements.

## Why This Is a Senior Skill

Mid-level engineers wire a moderation API onto the chat endpoint and move on. Senior engineers build guardrails as a policy engine: every check has a defined trigger, an action, a documented failure mode (fail-open or fail-closed), a latency budget, and a regression suite. They know guardrails are security controls that are themselves attack surface, that false positives cost real users, and that a guardrail no one monitors is decoration.

The senior framing is that guardrails are the *enforcement* layer between the threat model ([[01_LLM_Threat_Modeling]]) and the world. Each OWASP risk that can be stopped at runtime — injection patterns, harmful content, PII, malformed output, DoS — becomes one or more checks with an owner and a metric. Guardrail effectiveness is measured in [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]: trigger rates, bypass rates, false-positive rates, added latency — not in the existence of a policy document.

## Core Frameworks

### Input Guardrails

| Guardrail | What it checks | Action on trigger | Evasion risk |
|-----------|----------------|-------------------|--------------|
| Moderation | Harm, illegal, or policy-violating content | Block with policy message | Semantic novelty, obfuscation |
| Injection pattern detection | Instruction-override patterns | Block or flag for review | Encodings, multi-turn, indirect channels |
| PII detection | Personal identifiers in user input | Mask before the model sees it | Non-standard formats |
| Rate and token limits | Request frequency, input length, token budget (LLM04) | 429, truncation, or escalation | Distributed attacks |
| Scope eligibility | Out-of-scope or unsupported requests | Decline or route to the right handler | Scope-creep wording |

### Output Guardrails

| Guardrail | What it checks | Action on trigger |
|-----------|----------------|-------------------|
| Format validation | Schema, types, required fields | Retry with format feedback, then fallback |
| Grounding check | Claims not supported by retrieved context | Flag, re-query, or route to human |
| PII and secret scan | Leaked identifiers, secrets, canary tokens | Strip, block, or alert |
| Toxicity and harm policy | Violating content the model produced | Block and return a standard fallback |
| Instruction-pattern detection | Output that looks like a subverted model obeying an injection | Block; investigate the request |
| Domain policy | Medical, legal, financial advice claims | Append disclaimer or decline |

### Implementation Pattern Trade-Offs

| Pattern | Strengths | Weaknesses |
|---------|-----------|------------|
| Deterministic checks (regex, schemas, blocklists) | Fast, cheap, auditable, no latency surprises | Brittle; bypassed by novelty; high false-positive risk on broad rules |
| Classifier models (moderation APIs, small fine-tuned models) | Generalize beyond keywords; consistent policy | Training data needed; drift over time; per-call cost |
| LLM-as-guard | Catches novel and context-dependent violations | Cost and latency; nondeterministic; itself prompt-injectable |
| Cascade (cheap first, LLM last) | Bounds cost and latency; deterministic cases never pay the LLM tax | Complexity; ordering requires tuning |
| Fail-open vs fail-closed | Chosen per risk: UX for low-risk, safety for high-risk | The wrong default is a liability in either direction |

### Guardrail Architecture Maturity

| Level | Evidence |
|-------|----------|
| Ad hoc | Checks scattered in endpoint code; every feature reinvents them |
| Centralized | One guardrail module or gateway every model call passes through |
| Engineered | Policy-as-code, per-check metrics, latency budgets, regression suite in CI |
| Independent service | Guardrails deployed and red-teamed separately from the features they protect |

## In Practice

**Enforce the invariant: every request and response passes a defined policy boundary.** Route all model calls through one gateway or guardrail module so no code path can bypass policy. If individual endpoints can call the model directly, the guardrails are advisory, and advisories do not hold in production.

**Design each guardrail with a trigger, action, and failure mode.** For every check, write down what triggers it, what happens (block, mask, rewrite, retry, escalate, fallback), and what happens when the check itself errors. Fail closed for high-risk checks — never let a crashed PII scanner pass data — and fail open only for low-risk UX paths, deliberately and in writing.

**Cascade from cheap to expensive checks.** Deterministic schema and blocklist checks first, classifiers second, LLM-as-guard last for the ambiguous remainder. Measure the added p50 and p99 latency per check and keep the whole pipeline inside the product's latency budget — a guardrail that doubles response time is a performance regression with a policy attached.

**Treat guardrails as part of the eval loop, not a firewall.** Every bypass found in offline evaluation or production becomes a new test case and usually a new rule. The loop — eval finds failure, guardrail prevents it, monitoring confirms it — is the mechanism by which guardrails get better; without it they only get older.

**Watch false positives as a product metric.** Overblocking is silent churn: legitimate users get rejected, deflect to support, or leave. Track override and human-review rates per rule, and tune or remove rules whose false-positive cost exceeds their protection value. Precision is a safety property of the business, not a nicety.

**Make guardrail decisions audit records.** Log the trigger, the redacted input or output, the action taken, and the latency of each check. These records are what [[06_Incident_Response_for_AI_Abuse]] depends on: without them, an incident investigation starts from zero.

**The guardrail itself must not leak policy.** Verbose rejection messages teach attackers which rules exist and how to phrase around them. Denials should be minimal and uniform — "I can't help with that" — while detailed reasons go to logs and internal dashboards, never to the requester.

## Practical Exercise

1. Enumerate the checks your feature needs: list input-side and output-side policies with the risk each one maps to
2. Implement the input layer: moderation, injection patterns, PII masking, rate and token limits
3. Implement the output layer: schema validation, PII and canary scan, harm check, and a standard fallback response
4. Decide fail-open vs fail-closed per check based on risk; write the decision into the config, not a comment
5. Measure p50/p99 added latency; reorder checks cheap-first until the budget is met
6. Build a small bypass and regression suite for the guardrails and run it in CI
7. Add dashboards: trigger rate per rule, action distribution, false-positive review queue

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]]: guardrail patterns and the eval-to-guardrail lifecycle
- [[computing-foundation-note/Artificial_Intelligence/11_Prompt_Engineering_and_Security]]: layers 1 and 3 of injection defense
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]: guardrail metrics and the eval loop
- [[02_Prompt_Injection_Defense]]: injection-specific rules and blast-radius limits
- [[03_Data_Leakage_and_Privacy_Controls]]: PII and canary checks in context
- [[06_Incident_Response_for_AI_Abuse]]: guardrail telemetry as the detection layer
