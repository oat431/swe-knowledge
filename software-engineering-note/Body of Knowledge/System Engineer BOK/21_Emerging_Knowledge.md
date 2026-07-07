---
tags: [emerging-knowledge, future, systems-engineering, sebok]
---

> *Source: SEBoK v2, Part 8 — Emerging Knowledge*

# 21 — Emerging Knowledge

## Purpose

Part 8 of the SEBoK addresses **emerging topics and research** in systems engineering (SE). While the practice of systems engineering began appearing in journals around 1950 and has since gained momentum in most engineering and non-engineering circles, the rate and nature of change in recent decades have been profound. Classically trained systems engineers of the 1970s and 1980s face a paradigm shift brought on by software-centric systems, cybersecurity, agent-based and object-oriented approaches, and model-based practices — each bringing their own methods and tools.

This part introduces **significant emerging changes** that have a high probability of impacting how systems engineering is practiced. As topics discussed here evolve and become mainstream, they are moved into the appropriate SEBoK parts. The structure follows the evolution of System of Systems Engineering (SoSE) as a model: an emerging topic generates research, ultimately producing a foundational body of knowledge that expands and is absorbed into the wider discipline.

Part 8 consists of two Knowledge Areas (KAs):

| KA | Scope |
|----|-------|
| **Emerging Topics** | Significant emerging trends in SE practice — AI, socio-technical systems, MBSE, digital engineering, set-based design |
| **Emerging Research** | Recent (3–5 year) SE research: doctoral dissertations, INCOSE/IEEE publications, NSF- and SERC-funded research |

---

## Emerging Topics in Systems Engineering

The Emerging Topics KA informs readers on significant, rapidly emerging needs and trends that are reshaping SE practice. Topics are selected by the SEBoK editorial board based on their probability of significantly impacting SE. The current emerging topics include:

### 1. Introduction to SE Transformation

SE continues to evolve from a combination of heuristics-based practices (primarily from aerospace and defense) toward a theoretically grounded, model-based discipline. This transformation is driven by increasing system complexity, global megatrends, grand engineering challenges, stakeholder expectations, and the enterprise environment. The INCOSE Systems Engineering Vision 2035 defines the attributes of the transformed SE discipline as: model-based and digitally enabled, capable of dealing with complexity through enterprise agility, leveraging data science, supported by formal theoretical foundations, impacted by AI, and sustained by a step change in SE education with lifelong learning.

### 2. Socio-technical Systems

**Lead Author: Erika Palmer**

Socio-technical systems (STS) describe the interrelationship between humans and machines. The concept originated to cope with theoretical and practical work-environment problems in industry (Ropohl, 1999). In a systems engineering context, it has been argued that *all systems are socio-technical systems* (Palmer et al., 2019).

STS theory has developed over the past 60 years across several key research areas:

- **Human factors and ergonomics** (Carayon, 2006)
- **Organizational design** (Cherns, 1976)
- **System design** (Clegg, 2000; van Eijnatten, 1998)
- **Information systems** (Mumford, 2006)

As a design approach, Socio-technical Systems Design (STSD) brings human, social, organizational, and technical elements together in the design of organizational systems (Baxter and Sommerville, 2011). The working definition in SE context: *Systems operating at the intersection of social and technical systems* (Kroes et al., 2006).

**Modeling approaches** for socio-technical systems include:

| Approach | Examples |
|----------|----------|
| **Qualitative (Soft Systems)** | Insurance systems as socio-critical systems (Yasui, 2011) — combining Holon concept with the Vee Model |
| **Agent-Based Modeling (ABM)** | Guiding behavior of sociotechnical systems (Heydari and Pennock, 2018); enterprise systems (Pennock and Rouse, 2016) |
| **Economic Modeling** | Social Systems Engineering (Wang et al., 2018) |
| **System Dynamics** | Social policy (Palmer, 2017); public health (Homer and Hirsch, 2006); participatory SD modeling (García-Díaz and Olaya, 2018) |

A key insight from Pennock and Rouse (2016): when modeling sociotechnical systems versus traditional engineering systems, focus less on "control" and more on "influence."

### 3. Artificial Intelligence (AI) in SE

**Lead Authors: Barclay Brown, Tom McDermott, Rael Kopace, Ramakrishnan Raman, Ali Raz, Kevin Robinson**

AI is defined as the ability of a system to exhibit behavior which, if exhibited by a human, would be considered intelligent — processing and learning from data, recognizing patterns, solving problems, and making decisions autonomously. Modern AI systems fall into two categories: explicitly pre-programmed decision logic, and systems that learn from data (**Machine Learning — ML**).

**ML Foundations:**

| Type | Description |
|------|-------------|
| **Supervised Learning** | Labeled input-output datasets; learns mapping between input-output pairs |
| **Unsupervised Learning** | No labels; learns grouping/clustering of inputs and outputs |
| **Reinforcement Learning** | Agent trains itself via trial-and-error based on a reward function |

**The Dual Transformation: SE4AI and AI4SE**

The INCOSE FuSE workshop (2019) introduced two complementary labels:

- **SE4AI** — Applying SE methods to the design and operation of intelligent systems (improved safety, security, ethics, trust)
- **AI4SE** — Applying AI/ML techniques to improve human-driven engineering practices (augmented intelligence — scaling model construction, designing exploration systems, test case generation)

**SE4AI — Key Challenges:**

1. **New failure modes** not previously experienced — negative side effects, reward hacking, scalable oversight, unsafe exploration, distributional shift
2. **Unpredictability of performance** — non-deterministic and evolving behavior; ML systems learn during development and may change during operations
3. **Lack of trust and robustness** — explainable behavior is problematic; expert judgment requires understanding of results relative to context of use

**AI4SE — Applications:**

AI technologies can assist systems engineers across the lifecycle: advising architects on design decisions from collective prior experience, generating corner test cases during verification, and leveraging digital twins/synthetic environments for simulation insights.

**AI Applications Across Industries:**

| Domain | Examples |
|--------|----------|
| Sales | Price optimization, forecasting, dynamic recommendations |
| Security | Breach risk prediction, incident response, cyber threat classification |
| Healthcare | Assisted diagnostic imaging, disease risk prevention, personalized health |
| Autonomous Driving | Real-time sensor processing, dynamic path planning, route optimization |
| Manufacturing | Quality checks, failure mode prediction, predictive maintenance |
| Finance | Automated trading, risk management, credit application classification |
| Government/Legal | Tax evasion detection, policy checks, legal analytics, content generation |

### 4. V&V of Systems with AI Elements

**Lead Author: Laura Pullum**

Failure of an AI element can lead to system failure, hence the need for AI verification and validation (V&V). While the high-level definitions of V&V do not change for AI-containing systems, AI subsystems create unique challenges beyond conventional V&V.

**Characteristics of AI Leading to V&V Challenges:**

- **Lack of an oracle** — difficult/impossible to clearly define correctness criteria for each individual input
- **Imperfection** — intrinsically impossible for an AI system to be 100% accurate
- **Uncertain behavior for untested data** — high uncertainty about responses to untested input (radical changes given slight input changes)
- **High dependency on training data** — system behavior is highly dependent on training data quality and coverage
- **Erosion of determinism**
- **Unpredictability and unexplainability of individual outputs**

**V&V Challenge Areas:**

| Area | Key Challenges |
|------|----------------|
| **Requirements** | Formal specification is difficult for hard-to-formalize tasks; data requirements flow up from AI subsystem |
| **Data** | Coverage of operational space, adversarial inputs, data quality (accuracy, currency, consistency, lack of bias) |
| **Model** | High-dimensional input/parameter spaces, online adaptation, compositional reasoning, test coverage metrics, bias |
| **Properties** | Accountability, controllability, explainability, interpretability, reliability, resilience, robustness, safety, transparency |

**V&V Approaches:**

- Metamorphic testing (addressing the oracle problem)
- ML test score (tests for features/data, model development, infrastructure, monitoring)
- Corroborative verification (multiple methods at different levels of abstraction)
- Testing against strong adversarial attacks
- Formal verification (proving models consistent with specifications)
- Assurance cases combining V&V results as evidence

**Standards Development:** ISO/IEC JTC 1/SC 42 (AI standardization), IEEE P7000 series (Ethics of Autonomous Systems), ANSI/UL 4600 (Safety for Autonomous Products), SAE G-34 (AI in Aviation).

### 5. Transitioning SE to a Model-Based Discipline

MBSE is the **formalized application of modeling** to support system requirements, design, analysis, verification, and validation — with the model as a *primary artifact* of the SE process. This is a shift from traditional document-based SE, where emphasis is on producing and controlling documentation.

**Key Milestones:**

| Year | Development |
|------|-------------|
| 1993 | Wymore publishes *Model-Based Systems Engineering* — state-based formalism |
| 1993 | IDEF0 standard for function modeling |
| 2000 | Enhanced Functional Flow Block Diagram (EFFBD) |
| 2003 | OMG Model Driven Architecture (MDA®) |
| 2006 | OMG SysML™ adopted as general-purpose systems modeling language |
| 2008 | UPDM adopted for enterprise modeling (DoDAF/MODAF) |

**INCOSE Vision 2035 on MBSE:** "The Future of Systems Engineering Is Predominantly Model-Based." Systems engineers routinely compose task-specific virtual models using ontologically linked, digital twin-based model-assets — updated in real-time, cloud-based, enabled by modelling as a service, supporting massive simulation.

**Transition Challenges:** Advancing modeling languages (expressiveness, precision, usability), methods (rigorous across full lifecycle), tools (integration with multi-disciplinary engineering models), workforce development (training, infrastructure, organizational commitment).

### 6. MBSE Adoption Trends (2009–2018)

**Lead Author: Rob Cloutier**

Surveys conducted in 2009, 2012, 2014, 2018, and 2019 tracked MBSE adoption. Key findings from the 2018 survey (661 respondents from 30+ countries):

- **MBSE is expanding beyond defense/space** into energy, infrastructure, transportation, and civil engineering
- **Most useful in early project phases** — conceptualization (39%), requirements analysis (42%), system/subsystem architecting (76%)
- **Cultural resistance and skills shortage** are the top inhibitors to adoption
- **Customer value perception** of MBSE is rising; software engineers' perceived value is declining
- Only ~50% of practitioners believe management values their MBSE experience
- Organizations are providing **less training** despite growing skills shortages

### 7. Digital Engineering

**Lead Author: Ron Giachetti**

The US DoD Digital Engineering Strategy (2018) promotes creation of computer-readable models representing all aspects of the system throughout its lifecycle, integrated via a **digital thread** — shared data schemata connecting all stakeholders. MBSE is a subset of digital engineering; the key remaining challenge is integrating MBSE with physics-based models used by mechanical and electrical engineering.

A **digital twin** is a related concept — a high-fidelity model of the system that can emulate the actual system, enabling analysis of design changes prior to physical incorporation. Digital engineering represents an organizational transformation from document-intensive to model-based SE, where the digital thread serves as the authoritative source of truth.

### 8. Set-Based Design (SBD)

**Lead Authors: Eric Specking, Gregory S. Parnell, Ed Pohl**

SBD is a complex design method that enables robust system design by:
1. Considering a **large number of alternatives** (sets, not single points)
2. **Establishing feasibility** before making decisions
3. Using experts who design from their own perspectives and use the **intersection** between individual sets to optimize

SBD is particularly valuable when projects involve many design variables, tightly coupled variables, conflicting requirements, flexibility for trades, or poorly understood technologies. In early-stage design, SBD helps inform requirements analysis and assess design decisions. Quantitative SBD requires an integrated MBE environment to assess effects of constraining/relaxing requirements on the feasible tradespace.

**SBD Tradespace Exploration Process (8 steps):**
1. Analyze business/mission needs and requirements
2. Develop integrated models/simulations
3. Treat design decisions as random variables and explore tradespace
4. Evaluate alternatives (e.g., Monte Carlo simulation)
5. Classify tradespace by sets (set drivers vs. set modifiers)
6. Apply dominance analysis and optimization
7. Explore remaining sets for additional insights
8. Select one or more sets for next design stage

---

## Introduction to SE Transformation

The SEBoK's concern extends beyond current practice to the **future evolution of the discipline**. The knowledge in this KA reflects the transformation and continued evolution of SE, formed by current and future challenges.

**INCOSE Systems Engineering Vision 2025 (INCOSE 2014)** describes the global context and defines attributes of a transformed SE discipline:

- **Relevant to a broad range of domains**, well beyond aerospace and defense — addressing society's quest for sustainable system solutions
- **Applied to socio-physical systems** for policy decisions and remediation
- **Integrating market, social, and environmental stakeholder demands** against end-to-end life-cycle considerations and long-term risks
- **A key integrating role** supporting collaboration across diverse organizational and regional boundaries and disciplines
- **Supported by a more encompassing foundation of theory** and sophisticated model-based methods and tools for understanding increasingly complex systems under uncertainty
- **Enhanced by an educational infrastructure** stressing systems thinking and systems analysis at all learning phases
- **Practiced by a growing cadre of professionals** with technical acumen and mastery of next-generation tools and methods

**INCOSE Vision 2035 (INCOSE 2021)** further defines the transformed SE:

- **The future of SE is model-based**, enabled by enterprise digital transformation
- **SE practices will make significant advancements** to deal with complexity and enable enterprise agility
- **SE will leverage practices from other disciplines** such as data science to manage data growth
- **Formal SE theoretical foundations will be codified**, leading to next-generation methods and tools
- **AI will impact both SE practice and the types of systems designed**
- **A step change in SE education** starting with early education and a heavy focus on lifelong learning

SE has evolved from a combination of practices in aerospace and defense toward a standardized life-cycle approach. Early SE was heuristics-based; efforts are underway to evolve a theoretical foundation. The discipline is gaining recognition across industries, academia, and governments, though cross-fertilization across industries has been slow and global needs outpace progress.

---

## Future Directions

Based on SEBoK Part 8 and the INCOSE Vision documents, the following future directions are shaping systems engineering:

### Model-Based Everything
- SE transitions to a **predominantly model-based discipline** with models as primary artifacts
- Ontologically linked, digital-twin-based model-assets composed for task-specific purposes
- Cloud-based modeling-as-a-service with massive simulation capabilities
- Integration of SysML 2.0 and improved ontology-driven representations

### AI-Infused Systems Engineering
- **SE4AI** — New methods for engineering safe, trustworthy, ethical AI systems; new V&V approaches for non-deterministic, learning systems
- **AI4SE** — AI-augmented SE processes (architecture advice, automated test generation, digital twin simulation)
- Addressing challenges of explainability, bias, adversarial robustness, and trust

### Digital Engineering Transformation
- Digital thread connecting all lifecycle stakeholders through shared data schemata
- Integration of MBSE with physics-based engineering models
- Digital twins enabling analysis and emulation prior to physical changes
- Organizational transformation from document-based to model-based environments

### Socio-Technical Integration
- Recognition that all systems are fundamentally socio-technical
- Integration of social, organizational, and technical factors in system design
- Modeling approaches that emphasize "influence" over "control" in human-machine systems

### Research Frontiers
- Doctoral research in system reliability prediction, digital ecosystems, cybersecurity decision patterns
- SERC-funded research on enterprises, system of systems, trusted systems, and SE management transformation
- NSF EDSE program funding on how processes, organizational structure, social interactions, and strategic decision-making impact SE success

### Standards Evolution
- ISO/IEC JTC 1/SC 42 driving AI standardization
- IEEE P7000 series on ethics of autonomous and intelligent systems
- ANSI/UL 4600 for autonomous product safety evaluation
- SAE G-34 for AI in aviation certification

---

## Related Chapters

| Chapter | Relationship |
|---------|-------------|
| [[00_Introduction_to_SEBoK]] | Overall structure and scope of the SEBoK |
| [[01_Systems_Engineering_Fundamentals]] | Core concepts, principles, and heuristics that are being transformed |
| [[02_Nature_of_Systems]] | Engineered and natural systems — socio-technical context |
| [[03_Systems_Science_and_Thinking]] | Foundations of emergence, complexity, and systems thinking |
| [[04_Systems_Models_and_Approach]] | Modeling concepts, types, and standards that underpin MBSE and digital engineering |
| [[05_Life_Cycles_and_Processes]] | Development approaches being transformed (agile, lean, incremental) |
| [[06_Technical_Management_Processes]] | Technical management in the context of model-based and AI-infused SE |
| [[07_System_Definition_and_Architecture]] | Architecture definition using MBSE and set-based design |
| [[08_System_Realization_and_Maintenance]] | V&V challenges for AI systems and model-based integration |
| [[09_Systems_Engineering_Standards]] | Emerging AI and autonomous systems standards |
| [[11_Enterprise_Systems_Engineering]] | Enterprise transformation and digital engineering at enterprise scale |
| [[12_Systems_of_Systems]] | SoS engineering as a model for how emerging topics mature into the SEBoK |
| [[17_SE_and_Software_Engineering]] | AI/ML development intersects heavily with software engineering practices |
| [[18_SE_and_Quality_Attributes]] | Safety, security, resilience — critical properties for AI systems |
| [[20_Implementation_Examples]] | Case studies illustrating emerging practice applications |

---

## Key References

- INCOSE. 2014. *Systems Engineering Vision 2025*. Available at: https://www.incose.org/docs/default-source/aboutse/se-vision-2025.pdf
- INCOSE. 2021. *Systems Engineering Vision 2035*. Available at: https://www.incose.org/about-systems-engineering/se-vision-2035
- Wymore, W. 1993. *Model-Based Systems Engineering*. Boca Raton, FL: CRC Press.
- Siebel, Thomas M. 2019. *Digital Transformation: Survive and Thrive in an Era of Mass Extinction*. Rosetta Books.
- DoD. 2018. *DoD Digital Engineering Strategy*. Washington, D.C.: OUSD R&E.
- Baxter, G. and Sommerville, I. 2011. "Socio-technical systems: From design methods to systems engineering." *Interacting with Computers*, 23(1), pp.4-17.
- Seshia, Sanjit A., Dorsa Sadigh, and S. Shankar Sastry. 2020. "Towards Verified Artificial Intelligence." arXiv:1606.08514v4.
- Cloutier, R. 2019. "2018 MBSE Survey Results." In *Proceedings of the 2019 INCOSE MBSE Workshop*.
- Palmer, E., et al. 2019. "Social Systems — Where Are We and Where Do We Dare to Go?" Panel Discussion, 29th Annual INCOSE Symposium, Orlando, Florida.
