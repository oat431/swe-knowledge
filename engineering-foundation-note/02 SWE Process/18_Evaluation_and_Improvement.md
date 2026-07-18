---
tags: [evaluation, metrics, process-improvement, software-engineering]
source: "SWE Theory & Practice — Roger Pressman & Bruce Maxim, Ch 12–13"
---

# Evaluation and Improvement

> **Source:** *Software Engineering: A Practitioner's Approach* by Roger S. Pressman & Bruce R. Maxim — Ch 12 (Evaluating Products, Processes, and Resources) and Ch 13 (Improving Predictions, Products, Processes, and Resources)

---

## Approaches to Evaluation

### Four Categories of Evaluation Techniques

| Technique | Description | Control Level | Generalizability |
|---|---|---|---|
| **Feature Analysis** | Rate and rank attributes of products/tools (e.g., scoring design tools on criteria) | None — subjective ratings | Narrow — tool-specific |
| **Survey** | Retrospective study documenting relationships and outcomes; no variable manipulation | Low — observe existing data | Moderate — organizational |
| **Case Study** | Planned investigation comparing situations (sister project, baseline, or random selection) | Medium — some variable control | Limited — context-specific |
| **Formal Experiment** | Rigorous, controlled investigation manipulating independent variables | High — randomization, replication | Broad — generalizable |

### Preparing for an Evaluation

1. **Set the Hypothesis** — State what you want to investigate in quantifiable terms (e.g., "Cleanroom code has fewer faults per KLOC than SSADM code")
2. **Control Variables** — Decide which variables affect the outcome and how much control you have; experiments require direct manipulation, case studies do not
3. **Make It Meaningful** — Ensure results apply beyond the immediate study; experiments yield more generalizable results than case studies

### Common Evaluation Pitfalls

| Pitfall | Description |
|---|---|
| **Confounding** | Another factor is causing the observed effect |
| **Cause or Effect?** | The factor could be a result, not a cause |
| **Chance** | Small possibility the result happened by coincidence |
| **Homogeneity** | No link found because all subjects had the same factor level |
| **Misclassification** | No link found because factor levels could not be accurately classified |
| **Bias** | Selection or administration inadvertently skews the result |
| **Too Short** | Short-term effects differ from long-term ones |
| **Wrong Amount** | Factor would have had an effect, but not at the amount used |
| **Wrong Situation** | Factor has the desired effect, but not in the situation studied |

> **Rule of thumb:** The more control in the study, the stronger the evidence. Case studies provide "preponderance of evidence"; experiments provide "evidence beyond reasonable doubt."

---

## Assessment vs. Prediction

### Two Kinds of Measurement Systems

- **Measurement systems** — Assess an *existing* entity by numerically characterizing attributes
- **Prediction systems** — Predict some attribute of a *future* entity using a mathematical model

### Validating Prediction Systems

- Compare model performance with known data in the given environment
- State acceptance ranges in advance (e.g., reliability prediction accurate within 20%)
- Use **fit data** (to build/tune the model) and **test data** (to validate) — typically 2/3 and 1/3 split
- Beware that predictions can become self-fulfilling targets (use double-blind designs where appropriate)

### Validating Measures

- **Representation condition** — Relationships among numerical values must correspond to real-world attribute relationships
- **Internal validity** (narrow sense) — Measure accurately captures the attribute it claims to measure
- **Wide-sense validity** — Internally valid *and* a component of an accurate prediction system
- **Correlation ≠ causation** — Statistical correlation alone does not prove cause and effect

---

## Evaluating Products

### Quality Models

#### Boehm's Quality Model

Hierarchical model centered on **General Utility**:

```
General Utility
├── As-is Utility
│   ├── Reliability (accuracy, robustness, integrity, consistency)
│   ├── Efficiency (device efficiency, accessibility)
│   └── Human Engineering (communicativeness, accessibility)
├── Maintainability
│   ├── Testability (structuredness, self-descriptiveness, conciseness)
│   ├── Understandability (legibility, self-descriptiveness, structuredness)
│   └── Modifiability (structuredness, conciseness, augmentability)
└── Portability
    ├── Device independence
    └── Self-containedness
```

#### ISO 9126 Quality Model

| Attribute | Sub-characteristics |
|---|---|
| **Functionality** | Suitability, accuracy, interoperability, security, maturity |
| **Reliability** | Maturity, fault tolerance, recoverability |
| **Usability** | Understandability, learnability, operability |
| **Efficiency** | Time behavior, resource behavior |
| **Maintainability** | Analyzability, changeability, stability, testability |
| **Portability** | Adaptability, installability, conformance, replaceability |

#### Dromey's Product-Based Model

Five steps to construct a quality model:
1. Identify high-level quality attributes
2. Identify product components
3. Classify tangible quality-carrying properties (correctness, internal, contextual, descriptive)
4. Propose axioms linking properties to attributes
5. Evaluate, identify weaknesses, refine

### Establishing Baselines and Targets

- **Baseline** — The typical/average result in an organization (e.g., 3 faults per KLOC in testing)
- **Target** — Minimally acceptable behavior (e.g., ≥95% fault removal efficiency)
- U.S. DoD uses quantitative targets with "malpractice levels" (e.g., fault removal <70% = malpractice)

### Software Reuse

#### Types of Reuse

| Dimension | Options |
|---|---|
| **Perspective** | Producer reuse (create) vs. Consumer reuse (use) |
| **Mode** | Black-box (use as-is) vs. Clear-box (modify) |
| **Approach** | Compositional (building blocks, bottom-up) vs. Generative (domain-specific, application generators) |
| **Scope** | Vertical (same domain) vs. Horizontal (cross-domain) |
| **Substance** | Ideas, artifacts, procedures, patterns |

#### Faceted Classification (Prieto-Díaz)

Components described by ordered facets (e.g., `<switching system, sort, telephone number, Ada, UNIX>`). Supported by a retrieval system with:
- Thesaurus of synonyms
- Conceptual closeness matching
- Usage tracking to identify gaps

#### Reuse Success Factors

- **Management commitment** — Unconditional, sustained support
- **Measurable goals** — Know when you've succeeded
- **Incentives** — Cash rewards, royalties, recognition (GTE: $100/component, royalties on reuse)
- **Domain analysis** — Systematic identification of reusable areas
- **Integrated process** — Reuse must be part of the development workflow, not an afterthought

> **HP Results:** 51% fault reduction, 57% productivity increase, 42% time-to-market reduction on reuse projects.

---

## Evaluating Processes

### Postmortem Analysis

A post-implementation assessment of all project aspects to identify improvement areas. Five-part process (Collier, DeMarco, Fearey):

1. **Survey** — Collect data without compromising confidentiality; guided by: don't ask for more than needed, don't ask leading questions, preserve anonymity
2. **Objective Information** — Collect cost (person-months, LOC), schedule (predictions vs. actuals), and quality (faults found per activity) data
3. **Debriefing Meeting** — Team members report what went well and what didn't; identify root causes
4. **Project History Day** — Small subset reviews events, schedule-predictability charts, and identifies ~20 root causes
5. **Publish Results** — Open letter with: project description, positive findings, three worst factors, improvement activities

### Process Maturity Models

#### Capability Maturity Model (CMM)

| Level | Name | Focus | Key Process Areas |
|---|---|---|---|
| **1** | Initial | Ad hoc, chaotic | None |
| **2** | Repeatable | Basic project management | Requirements management, project planning/tracking, subcontract management, SQA, SCM |
| **3** | Defined | Standard process for org | Process focus/definition, training, integrated management, product engineering, intergroup coordination, peer reviews |
| **4** | Managed | Quantitative quality mgmt | Quantitative process management, software quality management |
| **5** | Optimizing | Continuous improvement | Fault prevention, technology/process change management |

**Key insight:** Process visibility increases at each level. At Level 2, only inputs/outputs are visible. At Level 3, intermediate activities are defined. At Level 4, detailed measures enable quantitative feedback. At Level 5, the process itself can change dynamically in response to measurement.

#### SPICE (ISO 15504)

- Assesses **processes** (not organizations like CMM)
- Six capability levels: Not performed → Informally performed → Planned and tracked → Well-defined → Quantitatively controlled → Continuously improving
- Produces a **profile** per process area rather than a single rating

#### ISO 9000

- International quality standards applicable to any system (not just software)
- **ISO 9001** most applicable to software — covers design, development, production, installation, maintenance
- **ISO 9000-3** provides software-specific guidelines
- Requires documented quality system, design control, corrective/preventive action
- Less emphasis on statistical process control than CMM or SPICE

---

## Evaluating Resources

### People Capability Maturity Model

| Level | Name | Focus | Key Practices |
|---|---|---|---|
| **1** | Initial | No active people development | Ad hoc management |
| **2** | Repeatable | Basic work practices | Compensation, training, performance management, staffing, communication, work environment |
| **3** | Defined | Competency-based practices | Participatory culture, career/competency development, workforce planning |
| **4** | Managed | High-performance teams | Organizational performance alignment, team building, mentoring |
| **5** | Optimizing | Continuous skills innovation | Continuous workforce innovation, coaching, personal competency development |

### Return on Investment (ROI)

**Net Present Value (NPV)** is the recommended approach for evaluating software-related investments:

```
NPV = Σ (Cash flow at time t / (1 + discount rate)^t) − Initial investment
```

- **Accept rule:** Invest if NPV > 0
- Sensitive to timing — later returns are penalized more
- NPV is additive — can evaluate collections of projects by summing
- Compare against alternative: "What if we just put the money in the bank?"

---

## Improving Predictions

### The Problem with Reliability Models

Different reliability models (Jelinski-Moranda, Goel-Okumoto, Littlewood, Duane, Littlewood-Verrall) applied to the same dataset produce dramatically different predictions — some optimistic, some noisy, none consistently best.

### Techniques for Improving Predictions

- **Ensemble methods** — Combine predictions from multiple models rather than relying on one
- **Local calibration** — Tune models to organization-specific data; "a predictive model is worthwhile only if used with a local process to select metrics that are valid as predictors"
- **Track prediction accuracy** — Compare predicted vs. actual over time to identify which models work best in your context
- **Simple baselines** — Sometimes simple heuristics (e.g., "tomorrow's weather = today's weather") are surprisingly competitive

---

## Improving Products

### Inspections

- Structured peer reviews (Fagan inspections) find faults early when they're cheapest to fix
- Effectiveness depends on **preparation time**, **reviewer expertise**, and **component size**
- Classification trees can identify which product measures predict where inspections will be most effective

### Reuse as Improvement

- Reuse improves not just coding but also testing, documentation, and design
- Reusing requirements/design has the largest ripple effect
- Benefits compound over time as the reuse library grows

---

## Improving Processes

### Cleanroom Software Development

- Combines formal specification, incremental development, and statistical testing
- Developers are not allowed to compile or unit-test their own code
- Independent team performs statistical testing based on usage profiles
- Results show significant quality improvements over traditional approaches

### Process Improvement Through Maturity

- Improvement is **incremental** — organizations move up one level at a time
- Each level provides the foundation for the next
- **Measurement is the enabler** — you cannot improve what you cannot see
- Process improvement must be **institutionalized**, not just implemented

---

## Key Relationships

```
Evaluation → Baselines → Targets → Improvement
     ↑                                    ↓
  Measurement ←── Feedback ←── Results
```

- **Evaluation** identifies what needs to change
- **Measurement** quantifies the current state and the effect of changes
- **Baselines** establish "normal" for comparison
- **Targets** set goals for improvement
- **Feedback loops** ensure continuous learning

---

## Practical Takeaways

1. **Match evaluation technique to control level** — Use experiments when you can control variables, case studies when you cannot
2. **Validate before trusting** — Ensure measures actually capture what they claim; correlation alone is not enough
3. **Postmortems are essential** — Learn more from failures than successes; make them blame-free and action-oriented
4. **Process maturity is a journey** — Each CMM level builds on the previous; skip none
5. **People matter most** — Differences between high- and low-performance teams have the largest influence on productivity
6. **NPV for investment decisions** — Compare technology investments against capital market alternatives
7. **Reuse requires investment** — Upfront costs are 1.1–4.8× normal, but long-term savings are substantial (10–63% of normal cost to reuse)
8. **Combine predictions** — No single model is universally best; ensemble approaches and local calibration improve accuracy

---

## Key References

- **Fenton & Pfleeger (1997)** — Software Metrics: A Rigorous and Practical Approach
- **Paulk et al. (1993)** — Capability Maturity Model (CMM)
- **Curtis, Hefley & Miller (1995)** — People Capability Maturity Model
- **Collier, DeMarco & Fearey (1996)** — Postmortem process at Wildfire Communications
- **Lim (1994)** — Reuse at Hewlett-Packard
- **Favaro & Pfleeger (1998)** — NPV for software investment decisions
- **ISO 9126, ISO 9000, ISO 15504 (SPICE)** — International standards


## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[02 SWE Process/17_Delivery_and_Maintenance|17_Delivery_and_Maintenance]] — Maintenance measurement
- [[02 SWE Process/16_Testing_Strategies|16_Testing_Strategies]] — Testing metrics and evaluation
- [[02 SWE Process/11_Project_Planning_and_Management|11_Project_Planning_and_Management]] — Project tracking and estimation
- [[01 Physics and Math/09_Math_Stats_and_Economics|09_Math_Stats_and_Economics]] — Statistical methods for evaluation
