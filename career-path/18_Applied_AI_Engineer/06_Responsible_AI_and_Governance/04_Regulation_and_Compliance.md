---
title: "Regulation and Compliance"
note_type: capability-topic
capability_area: responsible-ai-and-governance
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - regulation
  - compliance
  - eu-ai-act
  - pdpa
---

# Regulation and Compliance

> Translating legal obligations — the EU AI Act, GDPR, Thailand's PDPA — into engineering requirements and verifiable controls for AI systems.

## Why This Is a Senior Skill

Mid-level engineers wait for legal to send them a list. Senior engineers are the translators between legal text and system design: they read a regulatory requirement and produce a testable engineering control, a log, and an owner. They also understand that compliance is a market-access question — a Thai vendor selling to European clients must meet the EU AI Act regardless of where its servers live.

The senior challenge is proportionality. Reading every rule as maximum-burden process kills the roadmap; reading none as irrelevant creates existential fines. The skill is matching obligation to feature risk and spending compliance effort where the law actually points.

## Core Frameworks

### EU AI Act Risk Tiers

| Tier | Obligation | Example for an Applied AI Engineer |
|------|-----------|-----------------------------------|
| Unacceptable (prohibited) | Banned outright; prohibitions applied from Feb 2025 | Social scoring, emotion recognition in workplaces |
| High-risk | Risk management system, logging, human oversight, conformity assessment, registration | AI in hiring, credit scoring, critical infrastructure |
| Limited / transparency | Disclose that users interact with AI; label synthetic content | Chatbots must disclose AI identity |
| Minimal | No specific obligations; voluntary codes apply | Spam filters, internal drafting tools |

### EU AI Act Application Timeline

| Date | What Happened |
|------|---------------|
| 1 Aug 2024 | Act entered into force |
| 2 Feb 2025 | Prohibitions and AI literacy obligations applied |
| 2 Aug 2026 | General application of most provisions, including high-risk obligations |

### Thailand PDPA: From Law to Engineering

| PDPA Requirement | Engineering Implication for AI Systems |
|------------------|----------------------------------------|
| Consent for personal-data collection | User-facing consent flows; consent metadata recorded per user and per purpose |
| Sensitive personal data (race, religion, health, political opinion, biometrics) | Exclude from prompts, training, and eval sets without explicit consent and legal basis |
| Data subject rights (access, rectify, erase) | Deletion tooling must reach vector stores, caches, and logs — not just the primary database |
| Cross-border transfer restrictions | Thai-hosted inference or contractual safeguards with model providers |
| Data breach notification (72 hours) | Incident detection and notification runbooks covering AI pipelines |
| Data Protection Officer | Named DPO reviews AI features touching personal data before launch |

### Roles in Compliance

| Role | Owns |
|------|------|
| Legal / DPO | Interpretation, DPIA sign-off, regulator liaison |
| Engineering | Implementing controls, producing logs and evidence |
| Product | Consent UX, feature scoping, market decisions |
| Leadership | Risk appetite, funding, accountability |

## In Practice

**Turn each legal clause into a testable requirement.** "Ensure transparency" is not an engineering task; "the chatbot discloses it is AI within its first two messages, verified by an automated test" is. Compliance becomes tractable when law is decomposed into requirements with tests, exactly the way product requirements are — and the tests become the audit evidence.

**Treat the EU AI Act as your default bar even without EU customers.** The Act sets the de facto global standard: clients demand it contractually, and other regulators are drafting laws on its template. A Thai company whose AI features already satisfy EU transparency, documentation, and risk-management obligations will clear most future requirements at near-zero marginal cost.

**Run the PDPA data mapping before adding AI to anything.** Most Thai companies' AI features start from customer data — where it lives, who processes it, and where it flows across borders is the prerequisite. PDPA applies to personal-data processing whether or not AI is involved; adding an LLM adds new processing purposes that each need a legal basis, and vectors of personal data are still personal data.

**Cross-border LLM calls are a data-transfer decision, not just an API choice.** Sending user prompts to a provider hosted outside Thailand can constitute a cross-border transfer under PDPA, triggering contractual-safeguard requirements. This makes data residency an input to model selection (alongside the cost and latency trade-offs in the Model and Inference Operations area, 05): document which providers offer regional endpoints and data-processing agreements.

**Compliance evidence is log-based, not narrative.** Auditors and regulators ask "show me." Risk logs, model cards, consent records, change history, and incident reports must be queryable artifacts. A well-governed system produces compliance evidence as a byproduct of normal operations (see [[06_Governance_Operating_Model]]) — if evidence collection is a separate project, the operating model is broken.

**Plan for regulation churn.** AI law is young: the EU issued general-purpose-AI guidelines in mid-2025, and Thailand is drafting its own AI framework on top of PDPA. Build compliance mechanisms that are cheap to update — parameterized policies, versioned model cards, tiered change control — not hardcoded one-off checklists that die at the first amendment.

## Practical Exercise

Run a compliance pass for one AI feature:
1. Classify the feature under the EU AI Act tiers and justify the tier in writing
2. List every personal-data element the feature processes: where it is stored, who processes it, where it crosses borders
3. Map PDPA obligations (consent, deletion, breach response, transfer) to concrete engineering controls
4. Identify gaps: obligations with no control and controls with no obligation
5. Draft a DPIA-style document with data-flow diagrams for the feature
6. Convert the top five obligations into testable requirements with owners and due dates
7. Present the package to legal/DPO for sign-off and store it where the audit trail can find it

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/08_AI_Ethics_and_Future]] — transparency rights and regulatory pressure behind the laws
- [[01_AI_Risk_Management]] — regulatory non-compliance as a scored risk class
- [[03_Transparency_and_Model_Cards]] — documentation as compliance evidence
- [[06_Governance_Operating_Model]] — the audit trail regulators will ask to see
- [[05_AI_ROI_and_Business_Case]] — compliance cost as a TCO line item

## Sources

- [EU AI Act — regulatory framework (European Commission)](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai)
- [Overview of Thailand Personal Data Protection Act B.E. 2562 (2019)](https://www.nortonrosefulbright.com/en-th/knowledge/publications/e29d223d/overview-of-thailand-personal-data-protection-act-be2562-2019)
