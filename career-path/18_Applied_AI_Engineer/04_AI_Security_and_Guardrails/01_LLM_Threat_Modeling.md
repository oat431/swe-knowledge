---
title: "LLM Threat Modeling"
note_type: capability-topic
capability_area: ai-security-and-guardrails
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - llm-threat-modeling
  - threat-modeling
  - owasp
---

# LLM Threat Modeling

> Mapping the attack surface of an LLM application with the OWASP Top 10 for LLM Applications (2025) and deciding, before any code is written, where limited defense budget actually belongs.

## Why This Is a Senior Skill

Mid-level engineers can recite the OWASP LLM Top 10 and apply its mitigations one by one, treating every item as equally relevant to every system. Senior engineers use the Top 10 as a starting vocabulary, then build a threat model of *their* architecture: which of the ten risks actually apply, what a realistic attacker wants, how much a successful attack costs the business, and which risks are mitigated, transferred, or accepted — with a written rationale.

The senior difference is calibration. A support chatbot with read-only retrieval and an agent that can send emails, query customer databases, and trigger payments face radically different LLM07 (Insecure Plugin Design) and LLM08 (Excessive Agency) exposure, yet both could receive the same generic "OWASP mitigation" treatment from a checklist-driven engineer. Threat modeling is the skill that decides which differences matter.

## Core Frameworks

### OWASP Top 10 for LLM Applications (2025)

| # | Risk | What it means in practice | Where this area defends it |
|---|------|---------------------------|---------------------------|
| LLM01 | Prompt Injection | Attacker text overrides system instructions | [[02_Prompt_Injection_Defense]] |
| LLM02 | Insecure Output Handling | Model output trusted downstream (XSS, SQLi, code exec) | [[04_Input_Output_Guardrails]] |
| LLM03 | Training Data Poisoning | Malicious data backdoors the training corpus | Modeled here; pipeline defenses in the [[career-path/09_Data_and_ML_Engineer/00_overview\|Data and ML Engineer]] path |
| LLM04 | Model Denial of Service | Expensive queries exhaust compute or budget | [[04_Input_Output_Guardrails]] (rate and token limits) |
| LLM05 | Supply Chain Vulnerabilities | Compromised models, frameworks, plugins | [[05_Model_Supply_Chain_and_Tool_Security]] |
| LLM06 | Sensitive Information Disclosure | PII, secrets, or memorized training data in outputs | [[03_Data_Leakage_and_Privacy_Controls]] |
| LLM07 | Insecure Plugin Design | Tools accept unvalidated or overly permissive inputs | [[05_Model_Supply_Chain_and_Tool_Security]] |
| LLM08 | Excessive Agency | The model can act without human approval on high-stakes actions | [[02_Prompt_Injection_Defense]] (blast radius) |
| LLM09 | Overreliance | Humans trust model output without verification | Product design; surfaced in the threat model as an accepted organizational risk |
| LLM10 | Model Theft | Extraction of weights, prompts, or API access | [[05_Model_Supply_Chain_and_Tool_Security]] |

### Threat Modeling Approaches

| Approach | What it answers | Best used when |
|----------|----------------|----------------|
| Asset and data-flow driven | Which untrusted data crosses the trust boundary into the prompt, and what privileged data can leave with the output | You have one concrete feature to secure |
| Abuse-case driven | What does an attacker economically want: exfiltration, free compute, influence over decisions, content at scale | You need to prioritize controls |
| STRIDE adapted | Spoofing and tampering map poorly to LLMs; information disclosure, elevation of privilege, and denial of service dominate | Bridging to a traditional security team's vocabulary |
| Attack trees | The chain from injection entry point to final damage: inject → gain tool control → exfiltrate → cover tracks | Designing blast-radius limits for LLM01 |

### Threat Modeling Maturity

| Level | Evidence |
|-------|----------|
| Reactive | Guardrails added after each incident; no written model |
| Structured | OWASP Top 10 walkthrough per feature with applies/partially/N-A rationale, updated on architecture change |
| Proactive | Threat model gates design review; abuse cases run in CI; periodic red-team exercises feed the eval suite |
| Continuous | Attack telemetry from production feeds the model; attacker economics re-estimated as capabilities change |

## In Practice

**Model threats against your architecture, not against the generic list.** The Top 10 is a vocabulary, not a to-do list. Start from the data flow: which components can an attacker reach, which of those feed the prompt, which consume model output. Most real systems genuinely face three or four of the ten risks at a level worth engineering effort; identifying those three is the deliverable.

**Draw the trust boundary before choosing controls.** Everything that enters the prompt — user input, retrieved documents, tool outputs, images, conversation memory — is potentially attacker-controlled. Controls belong on the boundary: input guardrails before the model, output guardrails after it, permission checks inside every tool the model can invoke.

**Think in attacker economics, not just vulnerabilities.** An attacker chooses the cheapest path to a goal: free inference at scale (LLM04), a target's data (LLM06), influence over automated decisions (LLM01 + LLM09). Estimate the value of each goal to a plausible attacker, and spend defense budget where value and likelihood intersect.

**Re-run the model whenever the system gains capabilities.** Adding a tool, a memory store, a new retrieval source, or a multi-agent handoff changes the surface entirely. A model that was harmless read-only becomes an exfiltration or fraud vector the day it can send messages or query customer records. Threat-model updates must be a step in every feature's design review.

**Separate what you can eliminate, mitigate, and accept.** Prompt injection cannot be eliminated; document it as a mitigated-and-monitored risk with specific residual exposure. Some risks are cheaper to accept than to defend: a public-knowledge chatbot has almost no LLM06 exposure worth scanning for. Writing these decisions down — with likelihood, impact, and owner — is what makes the model useful to the rest of the organization.

**Red-team before production, with realistic adversaries.** Structured jailbreak and injection attempts against a staging deployment find the weak layer faster than any review. Every finding becomes a test case in the [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]] suite and a candidate control in [[02_Prompt_Injection_Defense]] — this closes the loop between modeling and defense.

## Practical Exercise

1. Pick one LLM feature you own and draw its full data flow: every input source, the model, every output consumer, every tool
2. Walk the OWASP LLM Top 10 and mark each entry applies / partially applies / not applicable, with a one-line architectural reason
3. For each applicable risk, write one abuse case: attacker goal, entry point, expected impact, estimated likelihood
4. Assign rough likelihood × impact, then pick the top three risks to mitigate; explicitly note the risks you accept and why
5. For LLM01, build an attack tree from direct injection to the worst realistic outcome given your tools and data
6. Write the result as a one-page threat model doc linked from the feature's design document, with an owner and a review date
7. Commit to re-running the exercise whenever the feature gains a tool, a data source, or a new integration

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/11_Prompt_Engineering_and_Security]]: OWASP LLM Top 10 table and the four defense layers this model feeds
- [[career-path/08_Security_Engineer/01_Threat_Modeling_and_Risk/00_overview|Threat Modeling and Risk]]: general threat-modeling method that LLM modeling extends
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]: abuse cases become eval cases
- [[02_Prompt_Injection_Defense]]: LLM01 mitigation in depth
- [[05_Model_Supply_Chain_and_Tool_Security]]: LLM05, LLM07, LLM08, LLM10 controls
