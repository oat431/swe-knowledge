---
title: "Incident Response for AI Abuse"
note_type: capability-topic
capability_area: ai-security-and-guardrails
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - incident-response
  - ai-abuse
  - postmortem
---

# Incident Response for AI Abuse

> Prepared detection, containment, and learning for the abuse incidents that will happen: injection-driven exfiltration, jailbreak-driven harm, coordinated resource abuse, and mass misuse of generated content.

## Why This Is a Senior Skill

Mid-level engineers respond when users report problems, then delete the offending logs and move on. Senior engineers assume abuse is inevitable and build the system to answer three questions in order: how would we know, how would we stop it, and how do we make sure it never happens this way again. Detection comes from telemetry designed in advance — canary hits, guardrail-trigger spikes, token-spend anomalies, anomalous tool calls. Containment comes from kill switches that work without a code deploy. Learning comes from a postmortem loop that converts every incident into eval cases, guardrail changes, and threat-model updates.

AI incidents also have an organizational shape that classic security incidents do not. A prompt-injection exfiltration may be a personal-data breach with legal notification duties; a jailbreak that produces harmful content may be a PR event; coordinated abuse of a public endpoint may be a cost spike measured in dollars per minute. The senior coordinates with security, legal, and leadership, and pre-agrees who decides what — before the incident, not during it.

## Core Frameworks

### Incident Taxonomy

| Type | Signature | Primary detection | Primary response |
|------|-----------|-------------------|------------------|
| Injection-driven exfiltration | Canary in external output, anomalous tool calls, unusual query patterns | Canary alerts, tool-call anomaly detection | Revoke tools, rotate prompts and credentials |
| Jailbreak-driven harm | Spike in guardrail triggers on output harm checks | Guardrail telemetry | Feature flag off, tighten output policy, fallback model |
| Resource abuse / model DoS (LLM04) | Token-spend anomaly, latency degradation, budget breach | Cost and latency dashboards | Rate limits, circuit breakers, hard budget caps |
| Data leakage (LLM06) | PII or secrets in outputs, canary in logs | Output scanners, canary alerts | Contain, notify per data-protection policy, rotate exposed data |
| Content abuse at scale | Mass generation of spam or deceptive content via API | Usage pattern detection, abuse reports | Key revocation, per-account quotas, content policy enforcement |
| Model theft attempts (LLM10) | Extraction-style probing traffic, weight download attempts | Traffic pattern monitoring | Rate limiting, access controls, legal escalation |

### Response Phases

| Phase | Actions | AI-specific twist |
|-------|---------|-------------------|
| Detect | Telemetry, canaries, anomaly thresholds | Abuse often looks like legitimate usage; anomalies are statistical, not signatures |
| Contain | Kill switches, tool revocation, prompt rollback, model fallback, human-only mode | Containment is capability removal, not just access removal |
| Eradicate | Identify entry vector and fix the control: guardrail rule, tool schema, prompt | The "patch" may be a prompt or guardrail change, not code |
| Recover | Restore service behind the new control, monitor for recurrence | Verify the bypass is closed with the eval suite before reopening |
| Learn | Postmortem → eval case → guardrail change → threat-model refresh | The loop feeds [[01_LLM_Threat_Modeling]] and [[04_Input_Output_Guardrails]] |

### Severity Rubric

| Severity | Example | Response expectation |
|----------|---------|----------------------|
| S1 | Confirmed exfiltration of regulated or confidential data; fraud executed via tools | Immediate kill switch, page leadership, legal notification process |
| S2 | Guardrail bypass producing policy-violating content; sustained resource abuse | Contain within hours, escalate to on-call and product owner |
| S3 | Blocked injection attempts; single-user abuse of limits; false-positive storm | Log, tune, fold into the next postmortem batch |

### Response Maturity

| Level | Evidence |
|-------|----------|
| Reactive | Incidents discovered by users; ad hoc response; no postmortems |
| Planned | Written runbook, named on-call, kill switches exist |
| Instrumented | Detection from canaries and telemetry; rehearsed playbooks; postmortems convert to eval cases |
| Continuous | Red-team exercises feed detection; containment time measured and improved; abuse economics revisited regularly |

## In Practice

**Instrument for detection before you need it.** Canary tokens in the system prompt, guardrail-trigger dashboards, token-spend and tool-call anomaly alerts — without these, your first signal of abuse is a customer or a bill. Detection telemetry is built in advance or not at all.

**Build kill switches that do not require a code deploy.** Per-tool feature flags, prompt-version rollback, model fallback, and hard spend caps let on-call contain an incident in minutes rather than a deployment cycle. The kill switch for the most dangerous capability — the write tool, the email sender — should be the best-tested switch in the system.

**Contain first, attribute later.** Sever the capability — disable the tool, take the feature offline, flip to human-only mode — and preserve the evidence before deep investigation. The cost of a few more minutes of exposure usually outweighs the forensic convenience of leaving the system running.

**Preserve evidence, and mind the nondeterminism.** Log the full request context — prompt, parameters, retrieved context, tool calls, and the actual output — with hashes and timestamps. Model outputs will not reproduce exactly on replay, so the stored output is the only authoritative record. Redacted logs are still forensic evidence; do not delete them in cleanup.

**Run the postmortem loop as a system, not a ceremony.** Every incident yields at least one new eval case, one guardrail or blast-radius change, and a threat-model review — with owners and deadlines. The loop is the only mechanism by which the system gets stronger; a postmortem with no follow-through teaches the same lesson again at the next incident.

**Treat AI incidents as organizational incidents.** Data-breach notification duties, PR exposure, and customer trust decisions are not engineering calls. Pre-agree the escalation ladder and decision rights with legal, security, and leadership, and rehearse the communication path. The engineering response and the organizational response must not be invented simultaneously at 3 a.m.

**Drill it.** Tabletop exercises with a simulated injection-exfiltration or jailbreak scenario reveal broken assumptions — a kill switch that needs an unavailable admin, an alert that pages nobody, a runbook step that assumes a service that does not exist. Measure time-to-contain in each drill and improve it like any other SLO.

## Practical Exercise

1. Write the one-page runbook: detection signals, severity levels, who to page, kill-switch locations, and decision rights
2. Implement the kill switches it names: per-tool feature flags, spend caps, prompt rollback, model fallback
3. Add detection: canary-output alerting, guardrail-trigger dashboards, token-spend and tool-call anomalies
4. Dry-run an injection-exfiltration scenario in staging; measure time from signal to containment
5. Write the postmortem template: timeline, blast radius, entry vector, evidence list, follow-ups with owners
6. Convert one real incident or near-miss into an eval case plus a guardrail change
7. Schedule a quarterly tabletop with the security team and revisit the runbook after each one

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/11_Prompt_Engineering_and_Security]]: canary tokens and the layers the response protects
- [[career-path/08_Security_Engineer/06_Detection_Incident_Response_and_Resilience/00_overview|Detection, Incident Response and Resilience]]: the general incident-management discipline this extends
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]: the telemetry and eval suite the loop depends on
- [[01_LLM_Threat_Modeling]]: incidents invalidate or confirm the threat model
- [[02_Prompt_Injection_Defense]]: canaries and blast radius under live attack
- [[04_Input_Output_Guardrails]]: guardrail telemetry as the detection layer
