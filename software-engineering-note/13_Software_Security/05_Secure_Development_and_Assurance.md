---
title: Secure Development & Assurance
tags: [security, secure-development, assurance, common-criteria, ssdlc, software-security]
source: Anderson Security Engineering Ch 27-28
created: 2026-07-21
---

# Secure Development & Assurance

> "My own experience is that developers with a clean, expressive set of specific security requirements can build a very tight machine. They don't have to be security gurus, but they have to understand what they're trying to build and how it should work." — RICK SMITH

> "The fox knows many things; the hedgehog one big thing." — ARCHILOCHUS

## Overview

Chapters 27-28 cover the systematic development of secure systems and the assurance that they will remain secure over their lifetime. Anderson argues there is **no silver bullet** — managing secure development requires *fox knowledge* (knowing many things) rather than *hedgehog knowledge* (knowing one big thing). Security and safety are **emergent properties** that must be baked in from the beginning, not retrofitted.

---

## Part 1: Secure Systems Development (Ch 27)

### 27.1 The Two Fundamental Questions

1. **"Are we building the right system?"** (Validation)
2. **"Are we building it right?"** (Verification)

Security engineering has converged with safety engineering: both require systematic thinking about what can go wrong — by accident (safety) or by malice (security). Accidents expose systems to attacks, and attacks degrade systems so they become dangerous.

### 27.2 Risk Management

#### Risk Registers
- Rate possible bad events on **severity (1-5)** and **probability (1-5)**, multiply for a score (1-25)
- Write down mitigating measures; assign a senior officer as risk owner
- **Key failure mode:** political/organizational capture — e.g., UK prioritized terrorism over pandemic preparedness despite pandemics being higher on the risk register

#### Annualized Loss Expectancy (ALE)
- Standardized by NIST for US government procurements
- `ALE = Expected Loss × Incidents per Year`
- **Realistic for common losses** (e.g., teller theft) where data exists
- **Guesswork for low-probability, high-impact threats** (e.g., large SWIFT fraud)
- **Cynical reality:** Consultants often tweak ALE totals to justify whatever budget the CISO has said is politically possible

#### Product Risks & Insurance
- Different industries have sector-specific safety rules (cars, aircraft, medical devices, railway signals)
- EU increasingly the world's safety regulator (largest market, Brussels cares more about safety than Washington)
- Insurance provides some signal but is cyclical; cyber-insurance market has become less rigorous since ~2017
- **Key motivator:** Executives buy insurance to protect *themselves* (legal/regulatory/PR risk), not just shareholders

### 27.3 Lessons from Safety-Critical Systems

#### Safety Engineering Methodology (ISO 61508)
1. Identify hazards and assess risks
2. Decide strategy (avoidance, constraint, redundancy)
3. Trace hazards to critical hardware/software components
4. Identify critical operator procedures
5. Set **safety functional requirements** (what safety mechanisms must do)
6. Set **safety integrity requirements** (likelihood of satisfactory performance)
7. Develop test plan → integrated into **safety case**

#### Hazard Analysis & Elimination
- **Design hazards out completely** when possible (e.g., motor reversing circuit)
- Security parallel: minimizing the **Trusted Computing Base (TCB)** is hazard elimination
- Privacy parallel: contact tracing — central DB vs. Bluetooth contact history on device

#### Fault Trees / Threat Trees
- **Top-down:** Start from undesired outcome, work backwards to possible causes
- Used in US DoD; each leaf = a combination of technical, operational, physical attacks
- Attack branches can have countermeasure sub-branches
- **Threat tree = essentially an attack manual**

#### Failure Modes and Effects Analysis (FMEA)
- **Bottom-up:** Trace consequences of each component's failure up to mission effect
- Natural for systems with well-understood critical components (aircraft)
- *Challenger disaster* (1986): O-ring failure mode known but concerns didn't reach top management

#### Threat Modeling (Microsoft approach)
- **Meet-in-the-middle:** List both assets to protect AND assets available to attacker
- Trace attack paths through the system from module to module
- Categorize threats: **STRIDE** — Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, Elevation of privilege
- Used for architecture reviews, code review targeting, penetration test targeting

#### "Programming Satan's Computer"
> Security is harder than safety because we assume a **hostile opponent** who can cause components to fail at the least convenient time and in the most damaging way.

- Safety engineer: certifies MTBF of 10⁹ hours
- Security engineer: must worry whether adversary can force the preconditions for that one-in-a-billion failure
- This is **"programming Satan's computer"** vs. "programming Murphy's"

### 27.4 Prioritising Protection Goals

- **Risk vs. Reward:** Security people often focus too much on loss reduction. If you can double turnover, shareholders prefer that even if losses triple.
- **Fraud management:** Best results when fraud team reports to **Sales** (opportunity), not Finance (cost center)
- **Error budgets:** Don't make systems too reliable. If local Internet is 99%, a 99.9% service is fine. Deliberate 0.1% error budget for resilience drills (Netflix "Chaos Monkey")
- **CVE question:** "Should we aim for zero open CVEs?" — tick-box says yes, but compliance cost may be prohibitive for most firms

### 27.5 Methodology

#### Waterfall Model (Royce, 1960s)
```
Requirements → Specification → Implementation & Unit Testing → Integration & System Testing → Operations
```
- **Strengths:** Early clarification of goals, definite milestones, cost transparency
- **Weaknesses:** Requires known requirements in advance; no system-level feedback from later stages
- **V-model** variant used in safety-critical systems (ISO 26262 for cars)

#### Iterative/Agile Development
- **Spiral model** (Boehm): proceed through iterations, evaluate risk at each stage
- **Agile:** "Solve your worst problem. Repeat."
- **Regression testing** is the core technology — automated daily builds + test suites
- Microsoft's key insight: abolish analyst/programmer/tester distinction → developers own their bugs

#### Security Development Lifecycle (SDL — Microsoft, 2010)
A **waterfall process for security** with five phases:

| Phase | Activities |
|-------|-----------|
| 1. **Requirements** | Risk assessment, quality gates / bug bars |
| 2. **Design** | Threat modeling, attack surface analysis |
| 3. **Implementation** | Approved tools, deprecate unsafe functions, static analysis |
| 4. **Verification** | Dynamic analysis, fuzz testing, attack surface review |
| 5. **Release** | Incident response plan, final security review |

**Organizational:** Security SME from outside team + security/privacy champion within team. Maturity model adapted from CMM (Capability Maturity Model).

#### Gated Development
- Security patches are gated: pre-release passes through extra tests/reviews
- **Microsoft's "Patch Tuesday"** (2003): bundled monthly updates
- Since 2015: Windows home users receive continuous updates
- Safety-critical systems: patch cycles must re-validate the safety case — expensive (lab cars, real test cars)

#### SaaS, DevOps, and DevSecOps

**Continuous Integration / Continuous Deployment:**
- Vendor manages software centrally, can migrate customers incrementally
- **Canary deployments:** 1% traffic to new version → monitor → roll forward or roll back
- **Staging environments:** test against snapshots of real customer data
- Changes the economics of testing — bugs can be fixed extremely quickly

**DevOps → DevSecOps (two ecosystems):**

| Aspect | Azure/Microsoft World | Google World |
|--------|----------------------|--------------|
| Primary concern | Integrating tools, automating admin tasks | Reliability at scale |
| Key reference | SDL extensions, compliance | SRE book, "Building Secure & Reliable Systems" |
| Approach | "Shift left" — security into codebase, fail fast | Design for recoverability, stop humans touching production |
| Key principle | Pre-commit static analysis, automated security testing | Error budgets, compartmentalized microservices, blast-radius limiting |

**Google's BeyondProd:** Crypto API (Tink) forces correct use; key management service required; code provenance, integrity verification, workload isolation.

#### The Vulnerability Cycle

**Key players and their incentives:**
1. **Vendor:** Prefers bugs not found (saves patching costs)
2. **Average customer:** Prefers no bugs found (avoids patching hassle)
3. **Security researcher:** Wants reward (fame, cash, or fix)
4. **Intelligence agencies:** Want early knowledge for zero-day exploitation
5. **Security software firms:** Benefit from unpatched vulnerabilities (sell detection)
6. **Large enterprises:** Hate patches (expensive testing/rollout)

**Evolution:** Bugtraq (1990s) → Responsible Disclosure → Coordinated Disclosure → Bug Bounties

**CVE System** (since 1999):
- Common Vulnerabilities and Exposures — assigns numbers to reported vulnerabilities
- CVSS: numerical severity score (local access, complexity, effort, effects, exploit availability)
- NIST National Vulnerability Database (NVD) integrates all publicly available US government resources

**Zero-day:** Exploit used in the wild before vendor issues a patch. Now commands millions of dollars on vulnerability markets.

**Coordinated Disclosure:** Multi-stakeholder (supply chains mean multiple vendors). Hard with platforms like Linux — thousands of products affected.

#### Security Incident and Event Management

Four components of an incident response plan:
1. **Monitoring** — threat intelligence team, honeypots, customer bug reports, CERT engagement
2. **Repair** — orchestrated response, identify dev teams, notify suppliers and customers
3. **Distribution** — rapid deployment; "in an emergency, run your normal patch process as fast as possible"
4. **Reassurance** — brief CEO immediately, have press releases ready, know key phone numbers

#### Organizational Mismanagement of Risk

- **Bezos' Law:** Can't run a dev project with more people than can be fed from two pizzas (~8)
- **Communication complexity:** N people → log N (military hierarchy), N² (everyone consults everyone), 2ᴺ (any subset forms a committee)
- **Most project failures:** Failure to understand requirements (Curtis, Krasner, Iscoe study)
- **Y2K lesson:** Well-defined requirements ("keep working as before") → success
- **Checklist culture:** Middle managers prefer box-ticking; checklists displace critical thought but don't handle uncertainty
- **ISO 27001 irony:** Almost every firm hit by a big data breach had ISO 27001 certification
- **CISO tenure averages ~2 years** — high stress, burnout risk

### 27.6 Managing the Team

- **Chief programmer team** (Brooks/Mills): lead + toolsmith + tester + language lawyer
- **Modern approach:** Hire only ultra-productive engineers; rotate slowly to acquire range of skills
- **Diversity:** More diverse teams are more effective. "Real change comes when you have enough [diverse members] to change the team culture — three or more."
- **Standard style:** Sit everyone down for an afternoon to hammer out house style — better team-building than paintballing
- **Bug culture:** Bugs should be declared openly, not quietly fixed (correlated bugs). Air traffic control analogy: controllers declare errors by open outcry.
- **Security responsibility:** Both — every developer gets security "boot camp" + subject matter experts at multiple levels

---

## Part 2: Assurance and Sustainability (Ch 28)

> "There are two ways of constructing a software design. One way is to make it so simple that there are obviously no deficiencies. And the other way is to make it so complicated that there are no obvious deficiencies." — TONY HOARE

### 28.1 The Shift from Static to Dynamic Assurance

**2008 paradigm:** Pre-market testing + evaluation schemes (Common Criteria)
**2020 reality:** Products are online, continuously patched, assurance is a **lifetime process**

Key evolution:
- Old: Two types — phones/laptops (patched monthly) vs. cars/medical devices (test-to-death, no patching)
- New: Everything online → must be patched online → continuous assurance

### 28.2 Evaluation

#### Historical Evaluation Schemes

| Scheme | Era | Approach | Fate |
|--------|-----|----------|------|
| **Orange Book (TCSEC)** | 1985-2000 | NSA-evaluated, government-paid, C1→A1 levels | Obsolete products, small market |
| **ITSEC (European)** | 1990s | Vendor-paid, competing labs | Race to bottom |
| **FIPS 140** | 1994-present | Tamper-resistance for crypto processors | Still used (US), but gap between Level 3 and 4 |
| **Common Criteria (CC)** | ~2000-present | Vendor-paid CLEFs, protection profiles, EAL1-7 | Captured by vendor interests |

#### Common Criteria — How It Works

- **Target of Evaluation (TOE):** product under test
- **Evaluation Assurance Level (EAL):** EAL1 (functional testing) to EAL7 (formally verified design)
- **Protection Profile (PP):** Implementation-independent security requirements for a product class
- **Security Target (ST):** Refinement of PP for a specific product
- **EAL4+:** EAL4 plus selected higher-level requirements (common for smartcards)

#### What's Wrong with the Common Criteria

| Problem | Detail |
|---------|--------|
| **Cost & bureaucracy** | Several million € and years to navigate — a moat defending cartels |
| **Ignores usability** | Administrative security measures excluded; "user interfaces are somebody else's problem" |
| **Protection profiles rig markets** | Sponsoring firms design PPs to favor their strengths |
| **Assumes waterfall** | Cannot cope with normal SDL/monthly patches |
| **Uneven rigor across countries** | Germany (hardest) vs. Hungary/Spain (easiest); cannot be discussed publicly |
| **No liability** | "Procedures for use of evaluation results in accreditation are outside scope" |
| **Brand not defended** | Vendors claim "CC evaluated" without actual certification |

**Collaborative Protection Profiles (cPPs, 2015):** Attempt to fix by moving from EAL levels to single industry-collaborative profiles. Result: captured by vendor interests — "secure fax machines" and printers now get cPP certification.

#### The Principle of Maximum Complacency (Lerner-Tirole model)

> Owners seek endorsement from a single certifier and resist attempts to improve the product.

- Competition between certifiers → **race to the bottom** (unless certifier cares about users more than sponsors)
- Exceptions: elite universities, NGO sustainability certification
- **CC evaluation = liability shield** for the vendor, not a technical authority

#### Boeing 737Max Case Study

- Root cause: MCAS software relied on **single** angle-of-attack sensor (design error)
- No proper FMEA; software behavior not in pilot manual
- Boeing had captured the FAA; engineers sidelined by finance executives
- **Cost:** 346 lives, $18.7bn in lost sales, $60bn market cap
- **Lesson:** Safety failure = technical + psychological + institutional + power dynamics

#### Emerging Alternatives

| Scheme | Approach |
|--------|----------|
| **ENISA (EU Cybersecurity Act 2019)** | EU-wide certification at basic/substantial/high levels |
| **Cyber Essentials (UK)** | £300 validated self-certification for basic security |
| **Bitsight** | Private-sector: crawls internet, rates firms by visible vulnerabilities — dominates insurance assessments |
| **ETSI EN 303 645** | Draft IoT security standard: ban default passwords, require update mechanism |

### 28.3 Metrics and Dynamics of Dependability

#### Reliability Growth Models

- **Single bug / simple case:** Exponential growth — `p = e^(-Et)`
- **Real large systems:** Growth follows `k/t` (not exponential) — reliability grows **linearly** with testing time
- **"If you want MTBF of a million hours, test for a million hours"** (safety-critical systems rule of thumb)
- **Evolutionary link:** Software testing and biological evolution both preserve diversity while removing minimum bugs/genes consistent with selection pressure
- **Attack vs. Defence asymmetry:** Attacker has thermodynamics on their side — a small team can find bugs that the defender's vast testing effort missed

#### Hostile Review

> Friendly reviews (by people who want the system to pass) are essentially useless compared with contributions by people seriously trying to break it.

**Effective hostile review models:**
- **NASA IV&V:** Contractors paid bonus per bug found
- **NSA vs. Sandia:** Competing to find each other's bugs
- **IBM:** Two crypto teams (NY + NC) trying to break each other's work
- **Google Project Zero:** 90-day aggressive disclosure; gets >97% of bugs fixed
- **Bug bounty programs:** Apple offers $1M for iOS kernel hack (zero-click)

#### Free and Open-Source Software (FOSS)

- **Kerckhoffs' Principle (1883):** Security must depend only on the key, not on secrecy of the technique
- **Raymond's criteria for FOSS benefit:** common engineering knowledge, failure-sensitive, needs peer review, business-critical, strong network effects — **security passes all five**
- **Standard model:** Openness helps attack and defence **equally**
- **Empirical data:** OpenBSD security bugs significantly correlated → openness was beneficial

### 28.4 The Entanglement of Safety and Security

#### Why They Converge

- Software in everything → everything connected → safety-critical devices need security patches
- EU is the world's lead safety regulator; must now incorporate security
- **"When hackers showed they could change the dose on an infusion pump to a fatal level, the FDA issued a safety advisory — but ignored 300+ models with only safety issues that kill thousands annually"**
- Public is much more sensitive to adversarial harm than accidental harm (even at far lower probability)

#### Autonomous Vehicle Safety & Security

- Regulation evolved from crash testing → recalls → complex ecosystem (driver training, road design, social norms)
- Now adding: adversarial ML attacks, remote exploitation, supply-chain security
- **ISO 21434** (draft): cybersecurity standard for road vehicles
- **UNECE regulations:** New cybersecurity and software update requirements for connected vehicles
- **Dieselgate lesson:** Vendors themselves are in the threat model — regulators can't blindly trust industry

#### Policy Recommendations

- Extend product liability to services
- Mandatory breach/vulnerability reporting to multiple stakeholders
- Self-certification that products can be patched (CE mark requirement)
- Shift from testing products to assuring whole systems
- Create security engineering competence within regulatory agencies

### 28.5 Sustainability

**The core problem:** Products that last decades must be patchable for decades.

- Most 2-year-old phones don't get patched (OEM + carrier coordination failure)
- How to patch a 25-year-old Land Rover exported across countries?
- Car industry wanted max 6-year software liability; EU pushed for longer
- **Environmental stakes:** Embedded carbon cost of new car ≈ lifetime fuel burn. Reducing average car life from 15 to 6 years would wipe out CO₂ savings from EVs
- **Right-to-repair:** Tech firms use "security" mechanisms to prevent repair; consumer-rights organizations fighting back

---

## Key Takeaways

1. **No silver bullet** — secure development requires broad knowledge across engineering, psychology, economics, and management
2. **Security must be baked in** — it's an emergent property, not a retrofit
3. **Risk management is political** — ALE analysis often justifies predetermined budgets; checklists displace critical thought
4. **Waterfall for security within agile products** — Microsoft's SDL is essentially waterfall, gated within an iterative overall process
5. **The attacker has thermodynamics on their side** — a small team can find bugs the defender's vast effort missed
6. **Hostile review is essential** — friendly evaluations (Common Criteria, ISO 27001) are systematically broken
7. **Assurance is now dynamic** — pre-market testing is insufficient; lifetime patching and monitoring is the new reality
8. **Safety and security are converging** — regulators must hire security engineers; standards must mandate development lifecycles
9. **Sustainability is the hardest problem** — devices lasting decades need security updates for decades, with profound environmental implications
10. **Culture matters more than process** — good teams with clear requirements beat bureaucratic compliance schemes every time

---

## Further Reading

- Brooks, *The Mythical Man-Month* [329]
- Leveson, *Safeware* [1151]
- Google, *Site Reliability Engineering* [237]
- Google, *Building Secure and Reliable Systems* [23]
- Swiderski & Snyder, *Threat Modeling* [1855]
- Howard & LeBlanc, *Writing Secure Code* [929]
- Raymond, *The Cathedral and the Bazaar* [1587]
- Petroski, *To Engineer Is Human* (bridge failures and learning from collapse) [1520]
- ISO 61508 (safety), ISO 26262 (car safety), ISO 21434 (car security — draft)
- Anderson, Leverett, Clayton — EU report on safety/security regulation [158]
