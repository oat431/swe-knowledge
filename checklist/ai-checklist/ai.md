# AI / LLM Application Checklist

> The **AI application** checklist — building production LLM features that don't burn money, leak data, or hallucinate dangerously.
> Horizontal: the deep reference for the AI/LLM sections in [[API Launch]], [[Frontend Launch]], [[Security]] §12, and the framework checklists.
> Covers: model selection, prompting, RAG, agents, evals, guardrails, cost control, observability.
> Last updated: 2026-08-05

---

## 1. Use-Case & Model Selection

- [ ] **Use case defined before model** — What does the feature *do*: classification, extraction, chat, summarization, codegen, embedding, tool use? Each has different model families and cost profiles.
- [ ] **Model choice deliberate** — Frontier API (GPT/Claude/Gemini) vs open-weights self-hosted (Llama/Qwen/Mistral via Ollama/vLLM) vs small local (Phi, Gemma). Trade-off: quality × cost × latency × data residency × privacy.
- [ ] **Model version pinned** — `gpt-4o-2024-08-06`-style pins, not floating `latest`. Model updates change behavior silently — pin, test, upgrade deliberately.
- [ ] **Fallback model defined** — Primary provider down → fallback provider/model with degraded but working UX. Circuit breaker + retry like any external dependency → [[API Launch]] resilience.
- [ ] **Serverless/edge considered for latency** — Streaming + provider edge regions. Cold start and token TTFB (time-to-first-token) measured.

## 2. Prompting & Context Engineering

- [ ] **System prompt separated from user input** — System prompt defines role/rules; user input stays untrusted. Never concatenate user content into the system prompt (injection surface) → [[Security]] §12.
- [ ] **Prompts versioned** — Prompts in version control (files or prompt registry), not string literals sprinkled in code. Prompt changes are code changes: reviewed, tested, deployed, rolled back.
- [ ] **Context window budgeted** — Token budget per request: system + instructions + retrieved context + history + response. Truncation strategy defined (drop oldest history first, then context).
- [ ] **Structured output used** — JSON mode / function calling / constrained decoding (grammar) instead of "please return JSON". Parse failures drop from 20% to ~0%.
- [ ] **Few-shot examples curated** — 2-5 examples that cover edge cases, not just the happy path. Examples are the cheapest fine-tuning.
- [ ] **Temperature/sampling tuned** — 0 for extraction/classification (deterministic), higher for creative. Seed for reproducibility where supported.

## 3. RAG (Retrieval-Augmented Generation)

- [ ] **Chunking strategy designed** — Chunk size (300-1000 tokens typical), overlap, and *splitter* (recursive character, semantic, document-structure-aware) chosen per document type. Headings/sections preserved.
- [ ] **Embedding model consistent** — Same embedding model for indexing and querying (mismatch = garbage retrieval). Version the embedding model like the LLM.
- [ ] **Vector store chosen** — pgvector ([[PostgreSQL]]), Qdrant, Milvus, Chroma, or managed (Pinecone). Consider: scale, filtering, hybrid search, operational burden.
- [ ] **Hybrid search considered** — Vector + keyword (BM25) + metadata filters. Pure vector misses exact terms (product codes, names). RRF (reciprocal rank fusion) to combine.
- [ ] **Metadata filtering** — Tenant, date, source, permissions as indexed filters. RAG without permission filtering = data leakage → [[Security]] §12.
- [ ] **Retrieval quality measured** — Recall@k / precision@k on a golden set. "Retrieval feels fine" is not a metric. Indexing pipeline tested in CI.
- [ ] **Citations surfaced** — Answers link to source chunks. Users (and auditors) can verify. Hallucination mitigation + trust + compliance.
- [ ] **Re-ranking considered** — Cross-encoder re-ranker on top-k (e.g. top-50 → re-rank → top-5). Big quality win for medium+ tiers.

## 4. Agents & Tool Use

- [ ] **Agent design justified** — Single-shot prompt with tools vs multi-step agent loop. Agents add latency, cost, and failure modes — use them only when the task genuinely needs iterative reasoning.
- [ ] **Tool definitions strict** — Every tool: name, description, JSON schema, *when to use* guidance. Vague tool descriptions = wrong tool calls.
- [ ] **Tool calls sandboxed** — Tools that execute actions (DB writes, API calls, code exec) validated and permissioned. Never give the model unfettered tools → [[Security]] §12.
- [ ] **Max iterations bounded** — Loop limit (e.g. 5-10 steps), total token budget, timeout. Runaway agent loops are a real production incident class.
- [ ] **Human-in-the-loop for consequential actions** — Confirm before irreversible effects (payments, deletions, sends). Agent autonomy grows with trust, not by default.
- [ ] **Agent state persisted** — Conversation state, tool results, and partial work recoverable across retries. Idempotent tool executions.

## 5. Evaluation (Evals)

- [ ] **Golden dataset exists** — 50-500 curated input → expected output pairs per use case. The backbone of every AI feature; without it you're guessing.
- [ ] **Offline evals in CI** — Model/prompt changes run against the golden set before deploy. Regression = blocked merge, like unit tests → [[QA]] §7.
- [ ] **Metrics chosen per task** — Exact match / F1 for extraction, ROUGE/BLEU for summarization (weak but fast), LLM-as-judge for quality, retrieval metrics (recall@k) for RAG.
- [ ] **LLM-as-judge calibrated** — Judge prompt validated against human labels on a sample (agreement ≥ 80% typical). Judge bias known (self-preference, length bias).
- [ ] **Online evals / production monitoring** — Sampling production traffic, thumbs up/down, feedback capture. Offline evals drift from reality — production feedback is the correction.
- [ ] **A/B testing for AI features** — Prompt/model changes shipped behind flags with measurable business metrics (conversion, task success). "Feels better" is not data → [[Release]] §5.

## 6. Guardrails & Safety

- [ ] **Input guardrails** — Prompt injection detection (at minimum: system/user separation + output validation). Content policy filters on user input where required.
- [ ] **Output validation** — Structured outputs schema-validated before use. Code output never executed without review. SQL/commands generated by the model validated against allowlists.
- [ ] **PII handling** — PII redaction before sending to external providers. Never send more data than the feature needs. Data residency respected (EU data → EU provider) → [[Security]] §12.
- [ ] **Abuse controls** — Rate limits per user on AI endpoints (cost + abuse), token caps, spend alerts. An LLM endpoint with no rate limit is a DoS + bankruptcy vector → [[Security]] §2.
- [ ] **Content policy for user-facing output** — Harmful/unsafe output filtered or flagged. "AI can be wrong" disclaimers where output is consequential.
- [ ] **Bias/fairness spot-checks** — Sample of outputs reviewed for systematic bias on protected attributes. Especially for decisions affecting users.

## 7. Cost Control & Performance

- [ ] **Cost per request tracked** — Input + output tokens × price, per feature, per user. Dashboards, not spreadsheets. Cost regressions caught in review.
- [ ] **Caching strategy** — Exact-match prompt cache (provider-side prompt caching), semantic cache (embedding similarity → cached answer) for repeated questions. Biggest lever after prompt size.
- [ ] **Token reduction discipline** — Shorter system prompts, trimmed history, retrieved-only-relevant context. Every 1K tokens × request volume × price = real money.
- [ ] **Model routing by task complexity** — Small/cheap model for simple tasks, large model only when needed. Per-request model selection saves 50-80% on mixed workloads.
- [ ] **Latency budget** — TTFB and total time per feature budgeted (UX requirement). Streaming mandatory for chat; batch/async for heavy processing.
- [ ] **Batch processing off-peak** — Non-interactive workloads (embeddings, summarization) queued and run at lower-cost times/tiers → [[Batch]] checklists.

## 8. Observability & Operations

- [ ] **Every LLM call logged** — Model, prompt hash, input/output tokens, latency, cost, status (success/error/refused), trace ID. The audit trail for AI features.
- [ ] **Traces span the pipeline** — Retrieval → prompt assembly → LLM call → output validation as one trace. Debugging "why did the AI say that" without traces is impossible → [[Release]] observability.
- [ ] **Metrics** — Error rate, latency percentiles, token usage, cost, cache hit rate, guardrail trigger rate. Dashboards + alerts.
- [ ] **Alerts** — Provider error spike, latency degradation, cost anomaly (spend > X/day), refusal rate spike (prompt regression?).
- [ ] **Provider incidents handled** — Status page monitoring, fallback activation, degraded-mode UX (cached answers, "AI unavailable" messaging).
- [ ] **Data retention for prompts** — Prompt/response storage policy: what's kept, how long, who can see it. Provider-side retention settings understood (training opt-out) → [[Security]] §12.

---

## Quick Sanity Check Before Launch

- [ ] Model pinned, fallback defined, circuit breaker + retry in place
- [ ] Prompts versioned in VCS, system/user separation enforced
- [ ] Structured output (JSON mode/grammar) — no "please return JSON"
- [ ] Golden dataset + offline evals in CI, regression blocks merge
- [ ] RAG: chunking + embedding pinned, metadata filtering, citations surfaced
- [ ] Agent loops bounded (iterations, tokens, timeout), tools permissioned
- [ ] Rate limits + spend alerts on AI endpoints
- [ ] PII redacted before external calls, data residency respected
- [ ] LLM calls logged with tokens/cost/trace; metrics + alerts armed
- [ ] Output validation schema-checked before use

---

## Project Tier Scoping Matrix

> **How to use this table:** Pick your tier first, then focus only on the sections marked ✅ (required) or 🟡 (recommended). Skip ❌ sections entirely — they'd be over-engineering for your context.
>
> **Legend:** ✅ Required · 🟡 Recommended / partial · ❌ Skip

### Tier Descriptions

| # | Tier | Description | Typical Team | Users | Lifespan |
|---|---|---|---|---|---|
| 1 | 🧪 **POC / Spike** | Validate an idea. Raw API calls are fine. | 1 dev | Internal only | Days–weeks |
| 2 | 🔧 **Prototype / MVP** | Waiting for integration or user validation. Might become real. | 1–2 devs | Beta testers | Weeks–months |
| 3 | 🏠 **Internal Tool** | Real users (employees), real usage. No external exposure or paying customers. | 1–3 devs | Employees | Ongoing |
| 4 | 🟢 **Small Production** | Single feature, low traffic. Real users, maybe early revenue. | 1–2 devs | < 1K users | Ongoing |
| 5 | 🔵 **Medium Production** | Multiple features or higher traffic. Real revenue or user base that matters. | 2–5 devs | 1K–100K users | Ongoing |
| 6 | 🟣 **Production Grade** | Full rigor — high-stakes SaaS, enterprise product, or large user base. | 5+ devs | 100K+ users | Long-term |
| 7 | 🔴 **Mission-Critical / Regulated** | Healthcare (HIPAA), finance (PCI-DSS), safety systems. Failure = severe harm. | 10+ devs | Varies | Decades |

### Which Tier Am I?

```mermaid
flowchart TD
    A[Is this throwaway / exploratory?] -->|Yes| T1[🧪 Tier 1 or 2<br/>POC / Prototype]
    A -->|No| B[Are the users internal<br/>employees?]
    B -->|Yes| T3[🏠 Tier 3<br/>Internal Tool]
    B -->|No| C[Do paying users or real<br/>revenue depend on it?]
    C -->|No| T4[🟢 Tier 4<br/>Small Production]
    C -->|Yes| D[Multiple services or<br/>1K+ users?]
    D -->|No| T4
    D -->|Yes| E[Enterprise / high-stakes<br/>/ regulated industry?]
    E -->|No| T5[🔵 Tier 5<br/>Medium Production]
    E -->|Yes| F[Failure could cause<br/>severe harm?]
    F -->|No| T6[🟣 Tier 6<br/>Production Grade]
    F -->|Yes| T7[🔴 Tier 7<br/>Mission-Critical]
    
    style T1 fill:#e1f5ff
    style T3 fill:#fff4e1
    style T4 fill:#e8f5e9
    style T5 fill:#e3f2fd
    style T6 fill:#f3e5f5
    style T7 fill:#ffebee
```

### Checklist Applicability by Tier

| # | Section | 🧪 POC | 🔧 Prototype | 🏠 Internal | 🟢 Small Prod | 🔵 Medium Prod | 🟣 Production Grade | 🔴 Mission-Critical |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 | Use-Case & Model Selection | 🟡 any model | 🟡 pinned | ✅ | ✅ + fallback | ✅ + routing | ✅ + multi-provider | ✅ + formal |
| 2 | Prompting & Context | 🟡 inline prompts | 🟡 + versioned | ✅ + system sep | ✅ + structured out | ✅ + budgeted | ✅ + registry | ✅ + audit |
| 3 | RAG | ❌ | 🟡 naive chunks | 🟡 if used | ✅ + citations | ✅ + hybrid search | ✅ + re-ranking | ✅ + permission filters |
| 4 | Agents & Tool Use | ❌ | ❌ | 🟡 if used | 🟡 bounded loops | ✅ + sandboxed tools | ✅ + HITL | ✅ + formal |
| 5 | Evaluation | ❌ | 🟡 manual spot-check | 🟡 golden set | ✅ + evals in CI | ✅ + LLM-judge | ✅ + online evals | ✅ + formal V&V |
| 6 | Guardrails & Safety | 🟡 if AI is the POC | 🟡 output validation | ✅ + injection guard | ✅ + rate limits | ✅ + PII redaction | ✅ + content policy | ✅ + regulatory |
| 7 | Cost Control & Performance | ❌ | ❌ | 🟡 track spend | ✅ + per-req cost | ✅ + caching | ✅ + model routing | ✅ + capacity plan |
| 8 | Observability & Ops | ❌ | ❌ | 🟡 basic logs | ✅ + token logging | ✅ + traces + metrics | ✅ + cost alerts | ✅ + full audit trail |

---

## Sources

- Complements [[Security]] §12 (AI security), [[QA]] §7 (evals as testing), [[Database]] §1 (pgvector), [[Release]] (flag-based rollout of AI features).
- AI/LLM sections in the framework checklists: [[API Launch]], [[Frontend Launch]], dotnet-api.md §18, fastapi.md §18, etc.
