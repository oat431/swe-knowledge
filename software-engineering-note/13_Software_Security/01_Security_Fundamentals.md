---
tags:
  - security
  - threat-modeling
  - psychology
  - economics
  - software-security
source: "Anderson, Ross. Security Engineering (3rd ed.), Chapters 1–3, 8"
created: 2026-07-21
---

# 01 Security Fundamentals

> *"Security engineering is about building systems to remain dependable in the face of malice, error, or mischance."* — Ross Anderson

---

## 1. The Security Engineering Framework

Anderson defines four interacting elements needed to build dependable systems:

| Element | Description |
|---------|-------------|
| **Policy** | What you're supposed to achieve — the protection goals |
| **Mechanism** | Ciphers, access controls, hardware tamper-resistance, and other machinery |
| **Assurance** | The amount of reliance you can place on each mechanism, and how well they work together |
| **Incentive** | The motives of both the defenders (to do their job) and the attackers (to defeat your policy) |

> **Key insight:** The 9/11 hijackers getting knives through airport security was a *policy* failure (knives under 3" were permitted), not a mechanism failure. Yet governments responded by spending billions on passenger screening (mechanism) — a classic case of **security theatre**: visible but ineffective measures driven by political incentives.

**Critical questions for any system:**
- What are we seeking to prevent?
- Will the proposed mechanisms actually work?

---

## 2. Defining the System

The word "system" is a fertile source of errors. Anderson defines six levels:

1. **Product/component** — crypto protocol, smartcard, hardware
2. **Above + OS + infrastructure** — comms, network
3. **Above + applications** — client and cloud components
4. **Above + IT staff**
5. **Above + internal users and management**
6. **Above + customers and external users**

> **Pitfall:** Vendors and evaluators focus on levels 1–2; businesses must think at level 6. Ignoring the human components and usability is one of the largest causes of security failure.

---

## 3. Key Definitions

| Term | Definition |
|------|------------|
| **Subject** | A physical person (operator, principal, or victim) |
| **Principal** | Entity participating in a security system — can be a person, role, device, or compound (group, conjunction, delegation) |
| **Trusted** (NSA def.) | A system whose failure can break the security policy |
| **Trustworthy** | A system that won't fail |
| **Secrecy** | Engineering mechanism limiting who can access information |
| **Confidentiality** | Obligation to protect another's secrets |
| **Privacy** | Ability/right to protect personal information and personal space |
| **Hack** | Something the system's rules permit, but which was unanticipated and unwanted by its designers |
| **Vulnerability** | Property that, with a threat, can lead to a security failure |
| **Security Policy** | Succinct statement of protection strategy |
| **Security Target** | Detailed spec of how policy is implemented in a product |

---

## 4. Who Is the Opponent? (Chapter 2)

Security engineers must first identify likely opponents. Anderson classifies threats by motive:

### 4.1 Spies (Nation-State Actors)

**Five Eyes (USA, UK, Canada, Australia, New Zealand):**
- **Prism** — NSA access channel via FBI for warranted wiretaps; used at scale for foreign intelligence
- **Tempora** — Bulk fibre-optic cable tapping (200+ transatlantic fibres, 21 Pb/day)
- **Muscular** — Collecting data flowing between data centres of Google, Yahoo (unencrypted backhaul)
- **Xkeyscore** — Distributed database for searching collected data; federated across 700 servers at 150 sites
- **Bullrun/Edgehill** — $100M/year program of supply-chain tampering, standards manipulation, crypto weakening
- **Quantum** — Dynamic exploitation of endpoints (Quantum Insert redirects browsers to attack servers)
- **CNE (Computer Network Exploitation)** — Hacking infrastructure (e.g. GCHQ hacking Belgacom to access roaming traffic)
- **Offensive operations** — Stuxnet (US/Israel worm destroying Iranian centrifuges), scalable cyber-weapons

**China:**
- Total domestic control + aggressive overseas collection
- Operation Aurora (2009): Hacked Google's wiretap infrastructure
- Supply-chain attacks (e.g. Supermicro motherboard implants)
- Censored versions of Western services with keyword scanning

**Russia:**
- NotPetya: Cyber-weapon deployed against Ukraine caused hundreds of millions in collateral damage
- Shadow Brokers leaks of NSA tools; sophisticated malware development
- Election interference operations

### 4.2 Crooks (Cyber-Crime)

- Online crime now makes up ~half of all crime by volume and value
- Criminal infrastructure based on malware and botnets
- **Underground markets** allow specialization: malware writers, credential harvesters, cash-out specialists
- **Sectoral ecosystems:** banking fraud, travel fraud, ransomware, business email compromise
- **Internal attacks:** ~1% of bank staff fired annually for dishonesty; double-entry bookkeeping as defence
- **CEO crimes:** Printer vendors using crypto for accessory control; VW emissions cheating

### 4.3 Geeks (Researchers and Hackers)

- Vulnerability researchers: curiosity + economic incentives
- Bug bounty programs (Apple up to $1M for zero-click iOS exploits)
- Volunteers contributing vulnerable code to open-source projects for six-figure payoffs
- Tension between responsible disclosure and government stockpiling

### 4.4 Personal Threats

- Cyber-bullying, stalking by former partners
- Harassment of public figures
- Online disinhibition, filter bubbles, radicalisation

> **Key takeaway:** You can't protect a complex system against all possible threats at reasonable cost. Identify your specific opponents and their capabilities.

---

## 5. Psychology and Usability (Chapter 3)

> *"Deception is now the principal mechanism used to defeat online security."*

### 5.1 Cognitive Psychology

- **Human memory** is not a video recorder — it's reconstructed, changes over time, can be manipulated
- **Miller's 7±2** is often misapplied — spatio-structural memory differs from echoic memory
- **Three categories of human error:**
  - **Slips/lapses** (skill level) — pressing wrong button, capture errors. Exploited by typosquatters.
  - **Mistakes** (rules level) — following the wrong rule. Exploited by phishermen (e.g. URLs like `www.citibank.secureauthentication.com`)
  - **Misconceptions** (cognitive level) — not understanding the problem. "Why Johnny Can't Encrypt" (PGP was too hard for most users)
- **Affordances** (Gibson) — the environment induces certain behaviours; can be weaponised as traps
- **Optical flows** — how we interpret motion; disrupting flows creates exploitable errors

### 5.2 Social Psychology

| Study | Finding | Security Implication |
|-------|---------|---------------------|
| **Asch (1951)** | People conform to groups even against their own eyes | Herd principle exploited in scams |
| **Milgram (1961)** | 60%+ obey authority even to harm others | "Officer Scott" hoax — 68 store managers strip-searched employees on phone orders |
| **Stanford Prison (1971)** | Normal people become sadistic when given authority | Abuse of authority must be designed against |
| **Bystander Effect** | Diffusion of responsibility in groups | Security is assumed to be "someone else's problem" |

- **Social-brain theory of deception:** We evolved large brains to deceive and detect deception (not to make tools). The brain is a machine for *arguing*, not for *truth*.
- **Theory of mind:** Ability to discern others' beliefs develops around age 5; delayed in autism spectrum.
- **Self-deception** (Trivers): We deceive ourselves to better deceive others.
- **Intent detection:** We over-prioritise threats involving hostile intent (terrorism) vs. impersonal threats (pandemic disease).

### 5.3 Behavioural Economics — Heuristics and Biases

| Bias/Heuristic | Description | Security Relevance |
|----------------|-------------|-------------------|
| **Prospect theory** | Loss aversion: losing $100 hurts more than winning $100 pleases | Phishers: "Your account has been frozen — click here to unlock" |
| **Anchoring** | Initial guess influences final judgment | Manipulated by framing |
| **Availability heuristic** | Overestimate risks that are easy to recall | Terrorism feared more than car accidents |
| **Present bias / hyperbolic discounting** | Immediate costs outweigh future benefits | People decline security updates even when critical |
| **Defaults (nudges)** | People take the path of least resistance | Facebook's open defaults; hazardous defaults set by adversaries |
| **Affect heuristic** | Emotions override probability calculation | Porn/emotional websites used to distribute malware |
| **Cognitive dissonance** | Rejecting information that conflicts with self-image | Victims reluctant to admit they were duped |
| **Risk thermostat** | People adjust behaviour to maintain constant perceived risk | Seat-belt laws don't save lives — they transfer risk to pedestrians |
| **Fundamental attribution error** | Explaining things by intent rather than context | Teaching users to parse URLs is limited — safe defaults are better |

### 5.4 Deception in Practice

**Cialdini's Six Principles of Influence:**
1. **Reciprocity** — people return favours
2. **Commitment and consistency** — people avoid being inconsistent
3. **Social proof** — people follow the group
4. **Liking** — people comply with likeable people
5. **Authority** — deference to authority figures
6. **Scarcity** — fear of missing out

**Stajano & Wilson's Seven Scam Principles:**
1. **Distraction** — mark focuses on the wrong thing
2. **Social compliance** — society trains us not to question authority
3. **Herd principle** — letting guard down when others share the risk
4. **Dishonesty** — marks doing something dodgy won't complain
5. **Kindness** — exploiting helpfulness
6. **Need and greed** — helping the mark dream a dream
7. **Time pressure** — preventing rational thought

> **Blurry line:** Fraud is essentially a subdivision of marketing. "Dark patterns" (hidden subscriptions, sneak-into-basket, forced account opening) are found on ~25–33% of popular websites.

### 5.5 Passwords

> *"It's hard to think of a worse authentication mechanism than passwords, given what we know about human memory."* — Angela Sasse

- Most passwords you set are for somebody else's benefit (maximising registered user bases)
- Humans can't remember infrequently-used items, can't forget on demand, recall is harder than recognition
- Password recovery is often the weakest link — security questions use near-public information (mother's maiden name, first school)
- Sarah Palin's Yahoo account was hacked via her date of birth and first school — both public
- **Key defence:** Big firms detect anomalous logins (wrong geography) and alert users; small sites can't

### 5.6 Social Engineering & Phishing

- **Social engineering:** Hacking systems through the people who operate them
- Mitnick's "Art of Deception" — almost all his exploits involved pretending to be a colleague
- IRS audit (2007): 62 of 102 employees gave their passwords to a caller
- **Phishing** emerged 1996 (AOL passwords), hit banks in 2003, stabilised around 2015 with 2FA and fraud engines
- **Spear-phishing:** Personalised attacks using context from social networks
- **Compliance budget:** Staff have limited hours for tasks not directly helping their goals — use it wisely

### 5.7 Operational Security (Opsec)

- Training must be **socially embedded**, not just documented
- Callback policies (never discuss records unless you initiated the call to a known number)
- Social rules: tailgating stopped by cultural norm of showing badges when someone holds a door
- Senior people are often the hardest to train
- Sensitive information should be made *impossible* to disclose, not just forbidden

---

## 6. Security Economics (Chapter 8)

> *"Many systems fail because the incentives are wrong, rather than because of some technical design mistake."*

### 6.1 Why Information Markets Produce Monopolies

1. **Low marginal costs** — duplicating information costs nearly zero → prices race to zero → free services funded by ads
2. **Network externalities** — value grows more than linearly with users (Metcalfe's law). Facebook won because "everyone was there."
3. **Supply-side scale economies** — access to unmatchable user data, A/B testing at scale
4. **Technical lock-in** — switching costs in retraining, file conversion, and data migration

> **Shapiro-Varian:** The value of a software company is the total lock-in of all its customers.

### 6.2 Market Failures Relevant to Security

| Failure Type | Description | Security Example |
|-------------|-------------|-----------------|
| **Monopoly** | Single vendor controls market; price discrimination | Microsoft's platform dominance → "ship Tuesday, get it right by version 3" |
| **Asymmetric information** (Akerlof's "Market for Lemons") | Buyers can't tell good from bad → market for poor security products | Why cheap AV outsells good AV; why people still buy AV at all |
| **Adverse selection** (hidden information) | Bad actors self-select into markets | Firms that cut corners buy more cyber-insurance |
| **Moral hazard** (hidden action) | People take more risks when protected | Volvo drivers have more accidents; seat-belt laws shift risk |
| **Externalities** | Costs dumped on others | Insecure machines on the Internet enable botnets |
| **Public goods** | Non-rivalrous, non-excludable | Air defence; some aspects of Internet security |

### 6.3 Game Theory

**Prisoners' Dilemma:** Both players have a dominant strategy to defect, even though mutual cooperation is Pareto-efficient. Explains why firms don't cooperate on security.

**Repeated games:** Tit-for-tat (cooperate first, then mirror opponent's last move) is among the best strategies. Forgiveness mechanisms needed to escape (defect, defect) lock-in from noise.

**Hawk-Dove game:** Proportion of aggressive individuals = f(cost of fighting). Explains why cartel-like behaviour emerges without explicit collusion.

**Structural models of attack and defence:**
- **Weakest link** — system security depends on the laziest defender (program correctness, patching)
- **Best shot** — depends on the single best defender (security architect)
- **Sum-of-efforts** — everyone's contributions add up (vulnerability testing)

### 6.4 The Software Patching Cycle

- **Early debate (2002–2006):** Should vulnerabilities be published (forcing vendors to patch) or reported privately?
- **Rescorla:** Removing one bug makes little difference — frequent patching unnecessary
- **Arora et al.:** Public disclosure makes vendors fix bugs faster; reported vulnerabilities decline over time
- **Ozment & Schechter:** OpenBSD vulnerabilities decreased over 6 years → software can be "like wine, not milk"
- **Settlement: Coordinated disclosure** — report to vendor, keep confidential until patch, then get credit
- **Bug bounties** (Apple up to $1M) compete with black markets and government stockpiling
- **Zero-day economics:** Exploits for iPhone sell for $1M+; volunteers contribute vulnerable code to open-source for payoff

### 6.5 Economics of Privacy

**The Privacy Paradox:** People say they value privacy, yet give away sensitive data for trivial benefits.

**Three compounding factors:**
1. Many different types of privacy harm (employment discrimination → stalking → payment fraud)
2. Behavioural factors — context matters enormously; making privacy salient *reduces* disclosure
3. Industry actively makes privacy risks less salient (anodyne policies, illusion of control via granular settings)

> **Learned helplessness (Pew 2015):** 93% say controlling their information is important; only 6% are "very confident" that agencies keep records private. Yet few take any action beyond clearing browser history.

### 6.6 Economics of Cybercrime

- **Becker model (1968):** Crime as a function of rewards vs. punishments — valuable but incomplete
- **Pathways to crime:** Gamers → cheat on games → deal in cheats → code cheats → malware devs
- **Minimisation strategies:** Russian cyber-crooks feel USA screwed Russia after 1989, so they're "getting their own back"
- **Gang structure:** Street-level dealers earn below minimum wage; targeting the boss doesn't work — target the choke point
- **NCA Google ads intervention:** £3,000 on ads warning that DDoS is illegal suppressed UK DDoS growth vs. USA
- **Stability:** Cybercrime ecosystem remarkably stable through 2010s despite technology shift from laptops to phones to cloud
- **Breach disclosure effects:** Single breach → small stock dip, dissipates in ~1 week; double breach → long-term investor confidence damage

---

## 7. Key Principles for the Security Engineer

1. **Identify your opponents** — capabilities, motivation, and how this may change over the system's lifetime
2. **Align incentives** — if the people guarding a system are not the people who suffer when it fails, expect trouble
3. **Design for human fallibility** — safe defaults beat user training; make the easiest path the safest path
4. **Understand the economics** — markets fail; monopolies and externalities shape security outcomes
5. **Think at the right system level** — include users, IT staff, and external parties; not just the technical components
6. **Security is often a power relationship** — principals who control what "security" means use it to advance their own interests
7. **The patching arms race is structural** — coordinated disclosure, bug bounties, and understanding the lifecycle of vulnerabilities are essential
8. **Deception is the #1 attack vector** — understand Cialdini's principles, dark patterns, and the psychology of compliance

---

## Sources

- Anderson, Ross. *Security Engineering: A Guide to Building Dependable Distributed Systems* (3rd ed., 2020), Chapters 1–3, 8
- Chapter 1: What Is Security Engineering? (framework, definitions, examples)
- Chapter 2: Who Is the Opponent? (spies, crooks, geeks, personal threats)
- Chapter 3: Psychology and Usability (cognitive/social psychology, behavioural economics, deception, passwords, phishing, opsec)
- Chapter 8: Economics (monopoly, information economics, game theory, patching cycle, privacy, cybercrime)
