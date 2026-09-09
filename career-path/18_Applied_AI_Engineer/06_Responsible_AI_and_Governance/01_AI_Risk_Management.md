---
title: "AI Risk Management"
note_type: capability-topic
capability_area: responsible-ai-and-governance
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - ai-risk-management
  - nist-ai-rmf
  - risk-register
---

# AI Risk Management

> Identifying, classifying, and controlling AI-specific risks through a structured framework so that risk decisions are explicit, owned, and defensible.

## Why This Is a Senior Skill

Mid-level engineers respond to incidents: something bad happens, a fix is rushed, and the same failure repeats elsewhere. Senior engineers run a risk program: risks are identified before launch, scored in likelihood and impact, assigned owners, and reviewed on a cadence. The difference shows in language — mid engineers say "this is probably fine"; seniors say "this residual risk is accepted by the product owner and re-reviewed in 90 days."

The senior challenge is calibration. Flagging everything produces a register nobody reads; flagging nothing produces surprises. A risk register with two hundred unowned rows is worse than no register at all.

## Core Frameworks

### NIST AI RMF Core Functions

| Function | What To Do | Why It Matters |
|----------|-----------|----------------|
| Govern | Assign owners, set policies, define risk appetite across the AI lifecycle | Without governance, risk decisions are made ad hoc by whoever ships fastest |
| Map | Inventory AI systems, contexts, and stakeholders; identify potential harms | You cannot manage risks you have not named |
| Measure | Quantify likelihood and impact; run tests and metrics against risk scenarios | Converts adjectives ("serious") into comparable numbers |
| Manage | Prioritize, apply controls, monitor, and reassess on a schedule | Closes the loop; risk treatment is continuous, not one-time |

The four functions form a cycle, not a checklist: an LLM feature mapped at launch needs re-measuring when the model version changes or usage shifts to a new user segment.

### Risk Classes for LLM Applications

| Risk Class | Example Scenario | Typical Control |
|------------|-----------------|-----------------|
| Misinformation / hallucination | Chatbot states a wrong policy or price to a customer | Grounding, refusal patterns, human escalation |
| Bias and discrimination | Screening tool systematically filters one demographic | Disaggregated evaluation, [[02_Bias_and_Fairness_Assessment]] |
| Privacy and data leakage | PII or customer data exposed in outputs | Data minimization, PII guardrails, access control |
| Security and abuse | Prompt injection, jailbreaks, model abuse | Defenses from [[career-path/18_Applied_AI_Engineer/04_AI_Security_and_Guardrails/00_overview\|AI Security and Guardrails]] |
| Regulatory non-compliance | Feature violates EU AI Act or PDPA obligations | [[04_Regulation_and_Compliance]] mapping, DPO review |
| Operational and vendor | Provider outage, model deprecation, cost spike | Fallbacks, multi-model config, cost budgets |
| Reputational | Product generates content that damages brand trust | Output moderation, rollout gates, kill switch |

### Risk Scoring and Treatment

| Severity | Likelihood x Impact | Required Response |
|----------|--------------------|-------------------|
| Critical | High likelihood, high impact | Must mitigate before launch; escalation to leadership |
| High | High on one axis, medium on the other | Mitigation planned and dated; owner named |
| Medium | Both medium | Accept with monitoring; re-review on cadence |
| Low | Low on either axis | Accept and document; annual re-check |

| Treatment | What It Means | When To Use |
|-----------|---------------|-------------|
| Avoid | Do not ship the capability | Unacceptable risk (e.g. prohibited uses under the EU AI Act) |
| Mitigate | Add technical or process controls | Most risks: guardrails, human-in-the-loop, training, limits |
| Transfer | Shift risk to another party | Insurance, vendor contracts, model-provider SLAs |
| Accept | Document and proceed with residual risk | Low severity; cost of control exceeds expected loss |

### Reporting Residual Risk to Leadership

| Report Element | What Leadership Needs |
|----------------|-----------------------|
| Top 5 residual risks | Current severity, trend, and next re-review date |
| Risks accepted in the last quarter | Who accepted each and under what condition |
| Incidents vs controls | Which controls fired, which risks materialized anyway |
| Register health | Rows opened vs retired; stale rows without owners |

## In Practice

**Treat the risk register as a living artifact, not a launch gate.** A register opened only before launch is theater — risks emerge from production incidents, model swaps, and usage changes. Review the register on a fixed cadence (monthly for active risks) and retire rows when controls are verified. A register nobody reads erodes trust in governance itself.

**Score risks in probability and money, not adjectives.** "High risk" means different things to different people; "30% chance of a ฿2M compliance fine and 2% chance of a ฿50M breach" is a decision input. Even rough quantification forces stakeholders to disagree about numbers instead of vibes, which is a far better conversation.

**Map every control to a risk — and every risk to a control.** Controls with no mapped risk are ceremony that should be deleted; risks with no control are open exposures. This one-to-one discipline is what turns a NIST-style framework from a document into a system.

**Start with Map, not Manage.** The most common failure is jumping to mitigations before inventorying what is actually deployed. An organization with forty shadow AI features cannot "manage" risk; it must first know what exists, who owns each feature, and which models and data each one touches.

**Assign a named owner to every open risk.** Risks owned by "the team" are owned by no one. The owner decides treatment, drives mitigation to completion, and signs off residual risk. Ownership records are also the audit trail regulators ask for.

**Re-measure when the system changes, not just on schedule.** A prompt change, model swap, new data source, or expansion to a new market changes the risk profile. Tie re-assessment triggers to change events (see [[06_Governance_Operating_Model]]), not only to calendar dates.

## Practical Exercise

Run a risk assessment for one real or hypothetical LLM feature:
1. Choose a feature (e.g. a customer-support chatbot) and write 10–15 concrete failure scenarios — think like users, attackers, regulators, and outages
2. Classify each scenario into the seven risk classes above
3. Score each on a 3x3 likelihood/impact grid and derive a severity
4. Select a treatment for each — avoid, mitigate, transfer, or accept — and name one owner per row
5. Write the register as a table: scenario, class, score, treatment, control, owner, review date
6. For the two highest-severity risks, define the control concretely enough that another engineer could implement it from your description alone
7. Schedule the first re-review date and present the register to a stakeholder as a decision document, not an FYI

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/08_AI_Ethics_and_Future]] — bias, safety, and alignment concerns that inform risk scenarios
- [[02_Bias_and_Fairness_Assessment]] — the measurement discipline for one major risk class
- [[04_Regulation_and_Compliance]] — legal obligations as risk inputs
- [[06_Governance_Operating_Model]] — the machinery that keeps the register alive
- [[career-path/18_Applied_AI_Engineer/04_AI_Security_and_Guardrails/00_overview|AI Security and Guardrails]] — technical enforcement of security-class controls
