---
tags: [risk-management, governance, cyber-security, cybok]
---

# Risk Management and Governance

> *Source: CyBOK v1.1 by NCSC, Chapter 2 — Risk Management and Governance*

## Purpose

To explain the fundamental principles of cyber risk assessment and management and their role in risk governance. The chapter grounds risk in human values, describes different perspectives on cyber risk assessment (from individual assets to whole-system goals), identifies major risk assessment methods and their uses/limitations, and links risk management to governance, security culture, and incident response.

---

## Risk Management Concepts

### What Is Risk?

Risk is grounded in **human value**. Renn's working definition: *the possibility that human actions or events lead to consequences that have an impact on what humans value*. Three abstract elements underpin risk assessment:

1. **Outcomes** that have an impact on what humans value.
2. **Possibility of occurrence** (uncertainty).
3. A **formula** to combine both elements.

**Risk assessment** is the process of collating observations and perceptions justified by logical reasoning or comparison with actual outcomes. **Risk management** is the process of developing and evaluating options to address risks in a manner agreeable to stakeholders. **Risk governance** is the overarching set of ongoing processes and principles that ensures awareness, education, responsibility, and accountability — underpinning collective decision-making and encompassing legal, social, organisational, and economic contexts.

### Why Risk Assessment and Management Matters

Risk assessment involves three core components:
- **Identification/estimation of hazard** — establishing events and outcomes.
- **Assessment of exposure and/or vulnerability** — aspects of a system open to threat actors and attributes that could be targeted.
- **Estimation of risk** — combining likelihood and severity (quantitative or qualitative).

An often overlooked part is **concern assessment** — capturing wider stakeholder perceptions of hazards, fear/dread, personal/institutional control, and trust in risk managers.

Risk management decisions lead to three outcomes:
- **Intolerable** — abandon/replace, or reduce vulnerabilities and limit exposure.
- **Tolerable** — reduce risk to ALARP/ALARA levels; options include mitigating, sharing, or transferring risk.
- **Acceptable** — no intervention needed; risk may even be embraced as opportunity ("upside risk").

### Four Types of Risk (Renn)

| Type | Characteristics | Management Approach |
|------|----------------|---------------------|
| **Routine** | Normal decision-making; statistics and data available | Define desirable outcomes, implement and enforce risk reduction |
| **Complex** | Less clear-cut; broader evidence needed | Cost-benefit analysis, comparative approaches |
| **Uncertain** | Lack of predictability | Precautionary approach; continual managed development; resilience |
| **Ambiguous** | Different stakeholders interpret risk differently | Participatory decision-making; discursive measures to reduce ambiguity |

### Risk Perception vs. Reality

People tend to **exaggerate dread-related but rare risks** (nuclear incidents, terrorist attacks) and **downplay common ones** (street crime, home accidents). Expert risk ranking follows expected/recorded undesirable outcomes, while lay people are influenced more by intuitive judgment. This mismatch makes transparent risk communication essential.

---

## Governance Frameworks

### What Is Risk Governance?

Risk governance is the overarching set of ongoing processes and principles aimed at ensuring awareness, education, responsibility, and accountability for managing risk. It underpins collective decision-making and encompasses both risk assessment and management.

### Three Governance Models (Millstone et al.)

1. **Technocratic** — Policy directly informed by science and domain expertise evidence.
2. **Decisionistic** — Risk evaluation and policy use inputs beyond science alone (e.g., social and economic drivers).
3. **Transparent (Inclusive)** — Context for risk assessment considered from the outset with input from science, politics, economics, and civil society ("pre-assessment").

The more inclusive and transparent the policy development, the more likely support and buy-in from wider stakeholder groups.

### IRGC Risk Governance Framework

The International Risk Governance Council (IRGC) framework, inspired by Renn, comprises four core areas with cross-cutting communication:

- **Pre-assessment** — Problem framing, identification of actors/stakeholders, capturing risk perspectives.
- **Appraisal** — Assessment of causes and consequences of risk; developing a knowledge base of risks and mitigation options.
- **Characterisation** — Judgment about the significance and tolerance of risks.
- **Management** — Deciding on and implementing the risk management plan (accept, avoid, mitigate, share, transfer).

Risk communication sits at the heart of the governance process. The governance process is iterative — always seeking awareness of new problems and evolving threats.

### NIST SP 800-30 Risk Assessment Process

The US NIST guidelines capture risk management in a cycle:
1. **Prepare** — Identify purpose, scope, assumptions, constraints, and risk tolerances.
2. **Conduct** — Identify threats, vulnerabilities, likelihood, and impact.
3. **Communicate** — Present results appropriately to each stakeholder group.
4. **Maintain** — Ongoing phase to continually update risk assessment as systems and environments change.

### ISO/IEC 27005 Process

Analogous to NIST with phases: Establish Context → Risk Assessment (Identification, Estimation, Evaluation) → Risk Treatment/Acceptance → Risk Communication → Risk Monitoring and Review. Less prescriptive than NIST; supports a range of assessment approaches.

---

## Security Policy

### Enacting Security Policy

Effective cyber risk governance is underpinned by a clear and **enactable security policy**. Key principles:

- From the initial risk assessment phase, there should be a clear focus on the **purpose and scope** of the exercise.
- For complex systems, identify **objectives and goals** with clear links to underpinning processes.
- Risks should be articulated as clear statements capturing interdependencies between vulnerabilities, threats, likelihoods, and outcomes.
- Risk management decisions must be linked to the security policy, which clearly articulates required actions, who performs them, and expected timelines.
- Security policy must also specify what happens as a consequence of risk becoming reality.

### Presentation and Visualisation

Heat maps and risk matrices are often used, but research has identified limitations in combining multiple risk measurements into a single matrix. Attention must be paid to the purpose of the visualisation and the accuracy of the evidence it represents.

### Policy and Human Factors

People fail to follow required security behaviour for two reasons:
1. **Unable** to behave as required (not technically possible; policies too large/difficult/unclear).
2. **Do not want** to behave as required (easier to work around; disagree with policy).

A set of rules dictating security risk management will almost certainly fail unless necessary actions are seen as linked to broader organisational governance — just like HR and finance policy. People must be enabled to operate securely and not be subject to a blame culture.

Security education should be a formal part of **continual professional development**, with reinforced messaging around why cybersecurity is important and each employee's role and duties.

### Accepted Risks and Accountability

The final risk assessment outcomes should include a **list of accepted risks** with associated owners who have oversight for organisational goals and assets underpinning the processes at risk. These individuals should be tightly coupled with review activity and clearly identifiable as responsible and accountable for risk management.

---

## Security Culture and Awareness

### The Human Factor in Security Governance (Sasse & Flechais)

Human factors impacting security governance include people:
- Having problems using security tools correctly.
- Not understanding the importance of data, software, and systems.
- Not believing assets are at risk (that they would be attacked).
- Not understanding that their behaviour puts the system at risk.

Risk cannot be mitigated with technology alone; concern assessment is essential.

### Risk Communication Principles

Risk communication plays a key role in governance, including:
- **Education** — Risk awareness and day-to-day handling of risks.
- **Training and behaviour change** — Changing internal practices and processes to adhere to security policy.
- **Creation of confidence** — Building trust in organisational risk management through strong performance.
- **Involvement** — Giving stakeholders opportunity to participate in risk/concern assessment and conflict resolution.

**Leading by example** is paramount — people resent it if senior management does not abide by the same rules.

### Just Culture (Dekker)

Dekker's principles on Just Culture aim to balance **accountability with learning**. Key ideas:
- Change how we think about accountability so it becomes compatible with learning and improving security posture.
- People must feel able to report issues and concerns, particularly if they think they may be at fault, without fear of stigmatisation.
- Accountability should be intrinsically linked to helping the organisation.
- An independent team to handle security breach reports can reduce anxiety and lead to a more open security culture.

### Security Metrics and Awareness (Jaquith)

Useful questions and metrics for improving security culture:
- Are employees acknowledging security responsibilities? (Metric: % new employees completing security awareness training)
- Are employees receiving training at required intervals? (Metric: % completing refresher training)
- Do security staff possess sufficient skills and certifications? (Metric: % with professional certifications)
- Are security awareness efforts leading to measurable results? (Metric: correlation of password strength with training latency, correlation of tailgating rate with training latency)

---

## Risk Assessment and Management Principles

### Component vs. Systems Perspectives

| Perspective | Focus | Good For |
|-------------|-------|----------|
| **Component-driven** (bottom-up) | Individual technical components; threats and vulnerabilities they face | Less complex systems with well-understood connections; working at levels where physical function is agreed |
| **System-driven** (top-down) | Goals of the entire system; sub-system interactions and dependencies | Complex interactions; establishing requirements before physical design; bringing together multiple stakeholder views |

Rasmussen's hierarchy of abstraction shows how these perspectives are complementary — higher levels consider goals and dependencies, lower levels consider capabilities and functionality of real-world artefacts.

### Elements of Risk

Four core concepts underpinning risk assessment in most models:

1. **Vulnerability** — Something open to attack or misuse that could lead to an undesirable outcome (socio-technical: technology, people, legal, etc.).
2. **Threat** — An individual, event, or action that has the capability to exploit a vulnerability (hackers, poorly trained employees, poorly designed software, etc.).
3. **Likelihood** — A measure capturing the degree of possibility that a threat will exploit a vulnerability (qualitative: low/medium/high; quantitative: 1-10 scale or percentage).
4. **Impact** — The result of a threat exploiting a vulnerability, having a negative effect on the objectives being assessed.

### Common Risk Assessment Methods

#### Component-Driven Methods

| Method | Core Strength | Approach |
|--------|---------------|----------|
| **ISO/IEC 27005:2018** | Socio-technical | Non-prescriptive; covers people, process, technology; externally led in large orgs |
| **NIST SP 800-30/39** | Technology-driven | Prescriptive IT system risk management; US-focused; includes control monitoring and compliance |
| **ISF IRAM2** | Business impact-driven | Practitioner-led; requires information risk management expertise; member-only |
| **FAIR / OpenFAIR** | Economic impact-driven | Taxonomy-based; very well-defined measures on economic impact; scenario-driven |
| **Octave Allegro** | Qualitative goal-oriented | Self-directed; collaborative workshops; links organisational goals to assets and threats; free |
| **STRIDE** | Threat-driven | Six areas: Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege |
| **Attack Trees** | Attack-driven | Formulates attacker objectives as root node; develops sub-nodes for compromise actions; iterative |

#### System-Driven Methods

| Method | Core Strength | Approach |
|--------|---------------|----------|
| **STAMP / STPA-Sec** | Causality | Systems thinking; feedback loop model; identifies risks from subsystem interactions |
| **TOGAF** | Enterprise architecture-linked | Qualitative; effect × frequency; risk assessment and mitigation worksheets as governance artefacts |
| **Dependency Modelling (O-DM)** | Capturing interdependencies | Goal-oriented; iteratively asks "what does this goal depend on?"; supports Bayesian analysis of cascading failure |
| **SABSA** | Matrix-structured layered approach | Top-down through architectural layers (context → concept → logical → physical → component); linked to business model |

### Vulnerability Management

A key outcome of risk assessment is identification of vulnerabilities. Ongoing management is critical because:
- New software vulnerabilities are discovered continuously.
- ~1 in 3 breaches occur due to vulnerabilities for which a patch was available but not applied.
- Automated scanning tools exist but should be treated with caution (false positives, insider knowledge risks).
- Risk governance groups should meet regularly to triage reports, prioritise fixes, and document justifications for non-implementation.
- Prioritisation factors: impact of exploitation, visibility (Internet-facing?), ease of automated exploit deployment, cost/availability of skilled resources.

For **operational technology (OT)** / cyber-physical systems: active scanning of legacy devices can disrupt operations. Legacy system considerations are essential.

### Cyber-Physical Systems and OT

OT/ICS systems underpinning Critical National Infrastructure focus more on **safety** than traditional IT (confidentiality/integrity/availability). System-driven methods are preferred as they abstract to high-level objectives (e.g., avoiding death, complying with regulation) and bridge security and safety perspectives. The NIS Directive mandates 14 goal-oriented principles for operators of essential services in Europe.

### Security Metrics

Ongoing debate: what to measure, how to measure, and why measure at all.

**Good metrics** (Jaquith):
- Consistently measured, without subjective criteria.
- Cheap to gather, preferably automated.
- Expressed as cardinal numbers or percentages (not "high"/"medium"/"low").
- Expressed with at least one unit of measure ("defects", "hours", "dollars").
- Contextually specific and relevant enough for decision-makers to take action.

**Bad metrics**: Inconsistently measured, labour-intensive, rely on qualitative labels (traffic lights, letter grades).

Herrmann provides a pragmatic view based on regulatory compliance, resilience, and return on investment — measuring whether controls are fit for purpose and whether they add more value than their cost.

### Business Continuity: Incident Response and Recovery

Despite best efforts, breaches will occur. Essential parts of risk governance include incident response planning:

**ISO/IEC 27035-1:2016** phases:
1. Plan and Prepare — Define policy, establish team.
2. Detection and Reporting — Observe, monitor, detect, report.
3. Assessment and Decision — Determine severity, take decisive action.
4. Response — Forensic analysis, patching, containment, remediation.
5. Learning — Make improvements to reduce likelihood of future breaches.

**NCSC Ten Steps**: Establish capability → Training → Assign roles → Recovery (tested backups) → Test recovery plans → Report → Gather evidence → Develop/refine → Maintain awareness → Report to law enforcement.

Information sharing about breaches is still nascent — organisations often prefer anonymity to prevent reputational damage, despite competitors potentially benefiting from shared intelligence.

---

## Related Chapters

- [[03_Law_and_Regulation]] — Legal context for risk management; NIS Directive; regulatory compliance obligations.
- [[04_Human_Factors]] — Human factors in security; why people fail to follow security behaviour; usability of security tools.
- [[08_Security_Operations_and_Incident_Management]] — Detailed incident management processes; forensics; security operations.
- [[21_Cyber_Physical_Systems_Security]] — OT/ICS security; safety-critical systems; vulnerability management in CPS environments.
