---
title: "Human Factors in Cyber Security"
yaml_tags: [human-factors, cyber-security, cybok]
source: "CyBOK v1.1 Chapter 4 — Human Factors"
version: "1.1"
date: "2021-07"
status: "reference"
---

# Human Factors in Cyber Security

> **Knowledge Area:** Human Factors  
> **CyBOK Version:** 1.1 (July 2021)  
> **Scope:** Usable security, human behaviour in security, security UX, stakeholder engagement

---

## 4.1 Introduction

The importance of human factors in cyber security has been recognised since the earliest days of the field. Nearly 100 years before **Saltzer & Schroeder**'s design principles, **Auguste Kerckhoffs** formulated six principles for operating a secure communication system, three of which focused on human factors: *"it must be easy to use and must neither require stress of mind nor the knowledge of a long series of rules."*

Both foundational texts recognised that **security measures cannot be effective if humans are neither willing nor able to use them**. A compelling example is email encryption: tools have existed for over 20 years, yet today less than 0.1% of emails are end-to-end encrypted. This outcome was predictable since Whitten & Tygar (1999) demonstrated that even well-motivated and trained people could not use email encryption correctly **Whitten & Tygar, 1999**.

### The "Weakest Link" Fallacy

Over the past 20 years, research has shown that security failures are not primarily the fault of users — despite the common refrain that humans are the "weakest link." The core problem is that **security measures treat humans as components** whose behaviour can be specified through policies and controlled through mechanisms and sanctions. This ignores the fundamental requirement that security must be usable and acceptable to be effective.

Adams & Sasse demonstrated that password policies agreed upon by security experts did not work in practice and were routinely bypassed by employees **Adams & Sasse**. Naiakshina et al. showed that not only end-users struggle — developers also need explicit prompting to include security, and even then often implement outdated and faulty mechanisms **Naiakshina et al.**.

### Scope of This Knowledge Area

This KA provides a foundational understanding of:
- How to **design security that is usable and acceptable** to a range of human actors (end-users, administrators, developers)
- A broader **organisational and societal perspective**: the importance of trust and collaboration for effective cyber security
- Skills for **engaging stakeholders** and negotiating security solutions that meet their needs

The chapter is organised from the inside out: starting with the individual and internal factors driving behaviour, then moving to the broader context of interaction, group dynamics, and organisational factors.

---

## 4.2 Usable Security — The Basics

When users do not behave as specified by security policies, the traditional response is to blame the user. But research shows non-compliance (better termed **"rule-bending"**) is caused by people facing a stark choice between security and productivity — and most choose productivity because that is what the organisation also prioritises.

The typical response — security awareness and education ("fitting the human to the task") — is limited. **Human Factors** research established decades ago that **"fitting the task to the human" is more efficient**. As the UK's **NCSC** states:

> *"The way to make security that works is to make security that works for people."*

### ISO Definition of Usability (ISO 9241-11:2018)

> *"The effectiveness, efficiency and satisfaction with which specified users achieve specified goals in particular environments."*

Criteria:
1. **Effectiveness** — accuracy and completeness with which users achieve specified goals
2. **Efficiency** — resources expended in relation to accuracy and completeness
3. **Satisfaction** — comfort and acceptability of the work system to users

---

### 4.2.1 Fitting the Task to the Human

Making security tasks usable means establishing a fit with four key elements:

1. The **capabilities and limitations** of target users
2. The **goals and tasks** those users have
3. The **physical and social context** of use
4. The **capabilities and limitations** of the device

---

#### 4.2.1.1 General Human Capabilities and Limitations

**Attention:** Humans can only focus their attention primarily on one task at a time. Security mechanisms demanding more time and attention than users can afford will fail. Changes in passive security indicators (e.g., padlock icons on screen edges) are often not noticed. Security indicators that need attention should be **placed in front of the person and require a response**.

**Alarm Fatigue:** The brain stops paying attention to signals it has classified as irrelevant — they are filtered out before reaching conscious processing. Once alarms are classified as unreliable, people ignore them. Even a 10% false alarm rate can induce alarm fatigue. SSL certificate warnings, for instance, have a false-positive rate of 50% or more, so it is unsurprising that people ignore them.

**The NEAT Principle** (Reeder, Microsoft): Warnings should be **N**ecessary, **E**xplained, **A**ctionable, and **T**ested — plus have a false alarm rate of 10% or less.

**Memory (STM vs LTM):**
- **Short Term Memory (STM):** Used for one-time passwords and codes. Works for most people for strings of up to 6 characters. Longer codes overload the STM loop, increasing entry time and error rates.
- **Long Term Memory (LTM):** Divided into **Semantic Memory** (general knowledge) and **Episodic Memory** (personal history). Items in episodic memory are retained better because they are connected to images and emotions.

**LTM and Passwords:** The interference effect makes managing multiple credentials nearly impossible — old passwords compete with current ones during recall. Most people struggle with more than 2–3 passwords or PINs. The **NCSC Password Guidance** recommends: switching to **2FA** solutions, using **password managers**, and not expiring strong passwords regularly.

**Biases in Credential Selection:**
1. Passwords tend toward the familiar — memorable names, dates
2. Graphical credentials favour strong colours and shapes
3. Pictures of humans: users pick "more attractive" people and those from their own ethnic background
4. Location-based credentials: users prefer features that stand out
5. Grid-based PINs: users choose connected locations anchored on edges/corners
6. Cultural preference for left-to-right ordering
7. Android swipe patterns: users pick from a very limited set of shapes

These biases reduce diversity and increase guessability. **Password strength meters** have shown limited accuracy improvement over 5 years **Golla & Dürmuth**.

**Specific User Groups:** Children, older citizens, people with motor skill limitations, colour blindness (~8% of males), autism, epilepsy — all need consideration in security mechanism selection and configuration.

**CAPTCHAs:** While some work addresses sensory impairments, CAPTCHAs add effort for legitimate users and contribute to **security fatigue** **CAPTCHA**.

---

#### 4.2.1.2 Goals and Tasks

Human behaviour is fundamentally **goal-driven**. People perform tasks to achieve goals. Tasks are classified as:

- **Production tasks** — what people consider "their job" (the work they were trained for)
- **Enabling tasks** — activities that protect the organisation's long-term viability (safety, security)

Security is often experienced as an unwelcome interruption. Most workarounds (writing passwords down, sharing them, keeping local document copies, mouse-jiggling software to prevent screen locks) happen because people try to **protect business productivity**.

**Design Principles for Security Tasks:**
- Automate security where possible (e.g., **implicit authentication**)
- Minimise workload and disruption when explicit human action is necessary
- Trigger security mechanisms only when necessary
- Design systems that are **secure by default**

**Physical vs Mental Workload:** Given a choice, most people prefer extra physical over extra mental workload, especially if the physical task is routine.

**Workload Audit** — before selecting a security measure, security specialists must ask:
1. What is the workload of primary and secondary (security) tasks?
2. Are there performance constraints on the primary task?
3. Are there resource constraints (mental, physical, or external)?
4. What is the impact of failing to complete the security task?

**Compliance Budget:** People have a built-in awareness of how much time they spend on non-productive tasks. As enabling tasks accumulate throughout the day, the likelihood of bypass increases. **Furnell & Thompson** coined the term **security fatigue**. Other enabling tasks (safety, sustainability, diversity training, regulatory compliance) compound this into **Compliance Fatigue** **Beautement et al.**.

**Recommendation:** Have honest discussions with line managers about the time budget for enabling activities, calculate security task workload, and identify which security behaviours really matter for the key risks.

---

#### 4.2.1.3 Interaction Context

**Contextual Inquiry** — the core premise: *"Go to the user, watch them do the activities you care about, and talk with them about what they're doing right then."* **Contextual Inquiry**

**Physical Context Factors:**
| Factor | Impact |
|--------|--------|
| **Light** | Bright light makes displays hard to see; glare affects camera-based biometrics |
| **Noise** | Interferes with voice recognition; high noise increases stress and error likelihood |
| **Temperature** | Cold affects fingerprint sensors and pointing speed; heat causes sweat that interferes with sensors |
| **Pollution** | Lipids + particles clog fingerprint sensors and leave visible patterns on touchscreens |

**Social Context:** Values (shared beliefs) and norms (rules and expectations about behaviour) strongly influence behaviour. If security behaviour conflicts with day-to-day behavioural norms — e.g., "be friendly to customers at all times" vs. "treat every customer enquiry as a potential information extraction attempt" — compliance problems follow.

Communicating **distrust** to employees encourages bad behaviour rather than preventing it. Users also get knowledge and support from their wider social networks **social learning**.

---

#### 4.2.1.4 Capabilities and Limitations of the Device

- Entering long, complex passwords on mobile soft keyboards is far slower and more error-prone than on regular keyboards
- Without collusion, user populations converge on a small number of passwords easiest to enter with minimal toggles — making guessing easier for attackers
- **2FA** solutions are not universally usable by default — many find tokens "too fiddly"
- Multiple implementations of 2FA and Chip & PIN create task variations that catch users out, leading to **human error**
- With increasing device diversity (smart watches, home devices, implicit interactions), the ergonomics of security interactions are ever more important

---

## 4.3 Human Error

> Over 30 years of research by **James Reason** established that virtually all mistakes people make are predictable.

### Reason's Swiss Cheese Model

A security incident occurs because a threat finds its way through a series of vulnerabilities (holes) in the organisation's defences (slices of cheese). A person may be the one who clicked the link — but **several other failures preceded this**, putting that person in a position where what appeared to be the right choice turned out to be the wrong one.

**Two types of failure:**
- **Latent failures** — organisation and local workplace conditions
- **Active failures** — errors and violations by humans

### Thinking Fast and Slow (Kahneman)

Most human activity is carried out in **System 1** (automatic, fast) mode. If people carried out most activities in **System 2** (conscious, slow) mode, they would not get much done. Exhortations to "Take Five" before clicking every link are unrealistic when people receive dozens of work emails with embedded links daily.

| Mode | Error Type | Cause | Security Example |
|------|-----------|-------|-----------------|
| **Automatic** (fast) | Slips & lapses | Recognition, memory, attention failure | "I forgot to check for the padlock before I entered my credit card details." |
| **Mixed mode** | Mistake I | Human chooses incorrect response | "I did not check for the padlock because websites on my iPhone are safe." |
| **Conscious** (slow) | Mistake II | Human does not know correct response | "I did not know to check for the padlock before entering my credit card details." |

Even in conscious mode, people try to be efficient — they choose behaviours they use frequently or that seem most similar to the situation. **Attackers exploit this** by creating similar-looking websites or incorporating security messages into phishing emails.

### Four Types of Latent Failure

1. **Individual factors** — fatigue, inexperience, risk-taking attitude
2. **Human factors** — limitations of memory, common habits, widely shared assumptions
3. **Task factors** — time pressure, high workload, monotony, boredom, uncertainty about roles and rules
4. **Work environment factors** — interruptions, poor equipment and information, changing rules and procedures

### Shadow Security

> *"Never issue a security policy that can't be followed."* — General MacArthur principle applied to security

When employees encounter impossible security policies, it undermines the credibility of all policies. **Kirlappos et al.** identified **shadow security**: employees do not show blatant disregard for security, but rather try to manage risk in the best way they know — their "amateur" security solutions may not be fully effective, but since they are workable, asking "how could we make that secure?" is a good starting point for finding effective solutions.

---

## 4.4 Cyber Security Awareness and Education

Security hygiene must come **first**. If people are told risk is serious and they must follow policy, but cannot do so in practice, they develop resentment and negative attitudes toward security and the organisation.

### Three Distinct Elements

| Element | Purpose |
|---------|---------|
| **Security Awareness** | Capture attention. Convince people security is relevant to them and that there are steps they can take. Work with communications specialists to craft effective messages. |
| **Security Education** | Provide information about risks and protections. Transform incomplete/incorrect mental models into more accurate ones. |
| **Security Training** | Help people acquire skills. Support with practice in settings where they can experiment and reflect. Learning is more successful in a social community context. |

**Key insight:** Knowing what to do is not enough. Human activity is ~90% automatic, driven by routines stored in long-term memory. New security behaviour must displace existing habits — and **old habits die hard**. Only 1–2 behaviours can be targeted at a time; the next set should only be addressed once the first are genuinely embedded.

### The Behaviour Change Model (RISCS)

The **RISCS White Paper** presents a model requiring **seven steps** for security behavioural change — awareness, education, and training are only the first three. Further organisational investment in strategy, time, planning, and resources is required.

---

### 4.4.1 New Approaches to Support Awareness and Behaviour Change

**Anti-Phishing Simulations** are the most widely used approach today. They can decrease click rates in the short term by creating a "teachable moment." However, the **Fogg Behaviour Model** is clear: trigger moments only lead to behaviour change if the person has sufficient **motivation** and **ability**.

**Human factors concerns with anti-phishing simulations:**
1. Employees may perceive being "attacked by their own organisation," reducing trust
2. They may become overly reluctant to click links, ignoring genuine important emails

The use of **DMARC** can reduce the number of suspicious emails, enabling training to focus on social engineering and manipulation techniques.

**Security Games:**
- **Capture The Flag (CTF)** — trains defenders by showing how vulnerabilities can be exploited
- **Tabletop card games** (e.g., Ctrl-Alt-Hack, d0x3d!) — security awareness for wider user bases
- **Board games** (e.g., Decisions and Disruptions) — raise awareness of threats and complexity of cyber risk decision-making in workgroups

Games offer social learning experiences if played in groups, but one-off exercises are unlikely to have lasting effects. They must be part of a **planned behaviour transformation programme**.

---

### 4.4.2 Mental Models of Cyber Risks and Defences

Much knowledge in long-term memory is organised as **mental models** — mental analogues of devices. They range from **structural models** (like blueprints, held by experts) to **task-action models** (enabling non-experts to operate devices competently).

Wash argues that inadequate mental models make users vulnerable: *"These users believe that their current behaviour doesn't really make them vulnerable, so they don't need to go to any extra effort."* **Wash**

Example mental model categories that may help communicate complex security issues:
- Physical security models
- Medical models
- Criminal models
- Warfare models
- Market models

Understanding users' mental models provides insights into how they perceive security information (e.g., alerts) and specific tasks (e.g., deletion).

---

## 4.5 Positive Security

The traditional approach to selling security relies on **FUD** (Fear, Uncertainty, and Doubt):

> *"FUD provides a steady stream of factoids... the effect of which is to persuade us that things are bad and constantly getting worse."* — **Florencio et al.**

The problem: when FUD-driven investment proves ineffective, decision-makers become skeptical, leading to a spiral of fear and grudging investment.

**Positive security** offers more than "freedom from" negative consequences — it enables "freedom to" engage in valued activities and cherished experiences **Roe**. A positive conception:
- Opens ideas for new policy options and interventions
- Encourages individuals and groups to become more involved in security decision-making and delivery
- Requires stopping the practice of demonising people as "The Weakest Link"

---

## 4.6 Stakeholder Engagement

### 4.6.1 Employees

A clear theme from the past decade of research: **the importance of engaging employees in finding ways of making security work for them**.

**Lizzie Coles-Kemp** and colleagues developed an approach using **projective techniques** (drawings, collages) to build representations of daily activity and ground security discussions in these. Case studies show this helps identify root causes of insecure behaviour — often badly designed security or more fundamental organisational failings **Coles-Kemp**.

**Creative Security Engagements** (**Dunphy et al.**) encourage participants to reflect on:
- Their environment
- The emotions they feel
- The constraints and pressures they experience
- The actions and tasks they perform when generating and sharing information

The EU **Trespass Project** used **Lego for physical modelling** of information security threats — bridging the gap between security practitioners' typical diagrams (flowcharts, UML) and the everyday practices of consumers. Heath, Hall & Coles-Kemp reported a successful case study modelling security for a home banking application, identifying areas where human intervention was needed.

**Productive Security** **Productive Security** represents a growing trend: moving away from finding traits conducive to desired behaviour, and instead **changing the design of security to align with user and organisational tasks** — reducing workload and increasing productivity, with a more positive perception of security as a valuable side-effect.

---

### 4.6.2 Software Developers and Usable Security

**Zurko & Simon** pointed out that unusable security affects not only general employees but also those with significant technical skills — developers and system administrators. They face increasing workloads and complexity, and make mistakes because the libraries and APIs they draw on are not usable. Errors by technical users generally have **more significant impact** (e.g., Heartbleed).

**Developers and Password Security:**
- Naiakshina et al. conducted randomised control trials with CS students and freelance developers
- None of the student participants, and only a small number of freelancers, implemented any security unless explicitly prompted
- Those who did implement security measures — students outperformed freelancers, who used more outdated and incorrect cryptographic mechanisms

**App Development Ecosystem Vulnerabilities:**
- Enck et al. and Fahl et al. highlighted vulnerability prevalence in app development ecosystems
- Developers had little to no security training and were under extreme pressure to complete apps quickly
- Of 96 developers contacted, only 13 were interviewed because their companies refused permission

**StackOverflow and Security:**
- **Acar et al.** studied the impact of online social networks on code security
- 2/3 of developers using StackOverflow or textbooks produced functionally correct solutions within allocated time, vs. 40% using official documentation
- In terms of security, results were reversed: official documentation users produced the most secure code, StackOverflow users the least
- Traditional response ("forbid StackOverflow") ignores the productivity cost — developers use forums for mutual support and community of practice
- The solution is not banning but providing **relevant support** that addresses why developers turn to these resources

---

## Key Takeaways

1. **Security must be usable to be effective.** This principle was recognised by Kerckhoffs and Saltzer & Schroeder, yet remains under-implemented.
2. **Humans are not the weakest link** — they are forced into "rule-bending" by security designs that ignore their goals, tasks, and limitations.
3. **Fitting the task to the human** is more efficient than trying to fit the human to the task through awareness training alone.
4. **Human error is predictable** — Reason's Swiss Cheese Model and Kahneman's System 1/2 framework explain why mistakes happen and how to design around them.
5. **Compliance Budget** — people can only absorb so many enabling tasks before security fatigue sets in. Prioritise and streamline.
6. **Positive security** — move from FUD-based fear selling to enabling "freedom to" through credible, usable security.
7. **Stakeholder engagement** — creative, participatory approaches produce better outcomes than top-down policy enforcement.
8. **Developers are users too** — security APIs, libraries, and documentation must be usable, and developer communities need support, not prohibition.

---

## Related Knowledge Areas

- **Risk Management & Governance** (Chapter 2) — security culture, risk perception, responsibility
- **Privacy & Online Rights** (Chapter 5) — usable privacy controls
- **Adversarial Behaviours** (Chapter 7) — social engineering, attack models
- **Authentication, Authorisation & Accountability** (Chapter 14) — authentication usability
- **Secure Software Lifecycle** (Chapter 15) — developer security training

---

## References

- Adams, A. & Sasse, M.A. "Users are not the enemy" (1999)
- Beautement, A., Sasse, M.A., & Wonham, M. "The Compliance Budget" (2008)
- Coles-Kemp, L. et al. — Creative security engagements and projective techniques
- Fogg, B.J. — Fogg Behaviour Model (motivation, ability, triggers)
- Furnell, S. & Thompson, K. — Security fatigue
- Herley, C. "More Is Not The Answer" — on security communications
- Kahneman, D. "Thinking, Fast and Slow" (2011)
- Kerckhoffs, A. — Six principles for secure communication systems
- Kirlappos, I., Parkin, S., & Sasse, M.A. — Shadow security
- Naiakshina, A. et al. — Developer password security studies
- NCSC — Password Guidance, "People: the strongest link"
- Reason, J. — Swiss Cheese Model, human error classification
- Reeder, R. — NEAT principle for security warnings
- Saltzer, J.H. & Schroeder, M.D. — Design principles for secure systems
- Wash, R. — Mental models of security
- Whitten, A. & Tygar, J.D. "Why Johnny Can't Encrypt" (1999)
- Zurko, M.E. & Simon, R.T. — User-centered security
