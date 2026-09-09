---
title: "Data Leakage and Privacy Controls"
note_type: capability-topic
capability_area: ai-security-and-guardrails
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - data-leakage
  - privacy
  - sensitive-information-disclosure
---

# Data Leakage and Privacy Controls

> Keeping sensitive data from reaching the model and from leaving it: classification and minimization on the way in, scanning and containment on the way out, governance on everything stored in between.

## Why This Is a Senior Skill

Mid-level engineers run PII detection on model outputs and consider leakage handled. Senior engineers design the whole data boundary: what may enter a prompt at all (classification and minimization), where the model runs and who logs what (provider and residency decisions), what may leave in outputs (scanning, canaries), and what the system itself stores afterward (conversation logs, memory, embeddings — all now copies of the sensitive data).

The senior understands that leakage is not one failure mode but four, each with its own controls: user data entering prompts unnecessarily, model outputs disclosing data (LLM06 — including memorized training data and other users' data in shared tenancy), infrastructure-side leakage through provider telemetry and stored logs, and system-prompt leakage. Each has a different detection signal and a different response. Privacy is also a legal event — prompt contents become processor-held personal data the moment they leave your infrastructure — so the senior treats the vendor relationship and the retention policy as part of the control set.

## Core Frameworks

### Leakage Directions

| Direction | Mechanism | Controls |
|-----------|-----------|----------|
| Input-side | Users paste secrets; retrieval pulls documents beyond the caller's clearance; memory carries over prior sessions | Data classification, minimization, retrieval ACLs, redaction before prompt assembly |
| Output-side | Model echoes memorized training data, another tenant's context, or the system prompt | Output scanning, canary detection, per-tenant context isolation |
| Infrastructure-side | Provider logs and telemetry; self-hosted inference caches; plaintext conversation logs | Data residency, vendor DPA and zero-retention terms, log redaction and retention limits |
| Model-side | Memorization of training data (LLM06) | Largely model-level; mitigate by avoiding fine-tuning on sensitive corpora and accepting a documented residual risk |

### Privacy Technique Trade-Offs

| Technique | What it does | Trade-off |
|-----------|--------------|-----------|
| Data minimization | Never put a datum in the prompt unless the task needs it | The cheapest control; requires prompt-construction discipline |
| Redaction and masking | Replace PII with placeholders before model input | Loses fidelity for names, IDs; reversible mapping needed for useful answers |
| Tokenization | Substitute reversible tokens for sensitive fields | Preserves more utility than masking; token store becomes a secret |
| Retrieval with authorization | Embeddings only ever index documents the requester may see | Prevents cross-tenant leakage; requires ACL-aware indexing |
| Self-hosted open-weight inference | Keep prompts inside your own infrastructure | Full ops and security burden; see [[05_Model_Supply_Chain_and_Tool_Security]] |
| Differential privacy and federated learning | Statistical privacy for training data | Training-side technique; belongs to the [[career-path/09_Data_and_ML_Engineer/00_overview\|Data and ML Engineer]] path |

### Classification Maturity

| Level | Evidence |
|-------|----------|
| None | Any user input and any document can reach the model |
| Coarse | Regex PII scanning on inputs and outputs |
| Policy-driven | Data classes (public, internal, confidential, regulated) each with an allow/redact/block/approve rule enforced in prompt assembly |
| Architectural | No data enters the prompt without a clearance check at the retrieval boundary; classes audited per tenant and per tool |

## In Practice

**Classify data before it can reach the prompt.** Define data classes and a per-class policy: public data passes freely, internal data passes within authenticated sessions, regulated data requires redaction or never enters the model. Enforce this in the retrieval and prompt-assembly layer — a classifier that runs after the model has already seen the data is not a control.

**Minimize: the model cannot leak what it never saw.** Drop fields irrelevant to the task, summarize long documents instead of injecting them raw, and default to sending the smallest context that answers the question. Every token of sensitive data in the prompt is one more token that can end up in a log, a vendor record, or an output.

**Treat the model provider as a data processor.** For API models, review data-retention terms and DPAs, enable zero-data-retention where offered, and route regulated workloads to self-hosted open-weight models when the contract demands it. If prompts leave your infrastructure, the provider's retention policy is your retention policy.

**Scan outputs, not just inputs.** Detection on the output side catches what input controls cannot: memorized training data, the system prompt, another user's context, and sensitive data the model assembled from innocuous fragments. Pair PII and secret scanning with canary tokens from [[02_Prompt_Injection_Defense]] — a canary hit proves prompt leakage deterministically.

**Isolate tenants at the context layer.** In multi-tenant systems, never assemble prompts from a shared context store without per-tenant authorization at retrieval time. A retrieval index that a tenant's query can reach beyond their own data turns one leaked response into a mass disclosure.

**Govern logs and memory like a database of secrets.** Prompt and response logs contain the most concentrated sensitive data your feature ever handles. Apply the same controls as any high-value data store: redaction at write time, access control, encryption, and retention limits. Long-term agent memory is persistent personal data — give it a deletion path and disclose it.

**Write the privacy posture down.** A one-page data-flow diagram showing every hop a prompt takes — client, retrieval, model, provider, logs, memory — with the control at each hop is the artifact that lets security, legal, and compliance review the system without reading the code.

## Practical Exercise

1. Map every datum entering prompts for one feature — user input, retrieval results, tool outputs, memory — and label each with a data class
2. Define a policy per class: allow, redact, block, or require approval; implement it in the single prompt-assembly code path
3. Review what is stored outside your control: the API provider's retention terms, or your self-hosted stack's inference and access logs
4. Add output scanning for PII patterns, secrets, and the canary token; define the action per hit: mask, block, or alert
5. Set log retention, access control, and redaction for prompt and response logs
6. Write the one-page data-flow and controls document; review it with your security contact and, for regulated data, legal
7. Note what you accepted (for example, memorization risk from a third-party model) and who owns that decision

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/11_Prompt_Engineering_and_Security]]: LLM06 and output filtering
- [[computing-foundation-note/Artificial_Intelligence/08_AI_Ethics_and_Future]]: privacy concepts behind the controls
- [[career-path/08_Security_Engineer/05_Identity_Access_and_Data_Protection/00_overview|Identity, Access and Data Protection]]: encryption, access control, and data protection general practice
- [[career-path/09_Data_and_ML_Engineer/00_overview|Data and ML Engineer]]: differential privacy and training-data governance
- [[04_Input_Output_Guardrails]]: the scanning machinery
- [[06_Incident_Response_for_AI_Abuse]]: leakage incidents and notification obligations
- Bias, fairness, and ethical use of data belong to area 06 (Responsible AI and Governance) of this path
