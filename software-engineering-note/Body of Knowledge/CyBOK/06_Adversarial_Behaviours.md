---
title: "CyBOK v1.1 – Chapter 7: Adversarial Behaviours"
author: "Gianluca Stringhini (Boston University)"
source: "The Cyber Security Body of Knowledge (CyBOK) v1.1, July 2021"
tags: [adversarial, cyber-security, cybok, attacker-models, motivations, tactics]
created: "2026-07-09"
---

# Chapter 7: Adversarial Behaviours

## Introduction

The technological advancements of recent decades have created opportunities for attackers to cause harm on an unprecedented scale. Before the Internet revolution, crime generally required physical contact between victim and perpetrator. Technology has removed this barrier — attackers can now reach victims anywhere in the world. This has revolutionised the characteristics of crime and warfare.

This chapter provides an overview of malicious operations happening on the Internet today, covering:
- A **taxonomy of malicious activities** based on attacker motivations and capabilities
- The **technological and human elements** adversaries need to run successful operations
- **Frameworks for modelling** malicious operations, drawing from computer science, criminology, and war studies
- How these frameworks help develop **effective mitigations**

---

## 7.1 A Characterisation of Adversaries

Adversaries are characterised based on their **motivation** (financial, political, etc.) and **capabilities**. This characterisation follows the evolution of cybercrime from ad-hoc operations by single offenders to a **commoditised ecosystem** where specialised actors operate together.

### 7.1.1 Cyber-Enabled and Cyber-Dependent Crimes

**Cyber-enabled crimes**: Traditional crimes whose reach is increased by the Internet — removing the need for physical proximity. According to Clough, criminals have five main incentives to move online:
1. **Easier to find and contact victims** (email lists, social network search)
2. **Cheaper operations** (free emails vs. postage)
3. **Faster execution** (near-instant delivery)
4. **Cross-border reach** (limited mainly by language barriers)
5. **Lower risk of getting caught** (transnational nature, underreporting)

**Cyber-dependent crimes**: Crimes that can *only* be committed using computers/technology devices. While often paralleling physical crimes (extortion, identity theft, fraud), technology enables large-scale organised endeavours reaching millions.

### 7.1.2 Interpersonal Offenders

Cyber-enabled crimes targeting individuals — harassment and violence amplified by the Internet.

- ****Cyberbullying****: Sending/posting harmful material or social aggression using digital technologies. Key differences from physical bullying:
  - Non-stop exposure (anytime, anywhere)
  - **Online anonymity** creates a **disinhibition effect** — negative people become meaner
  - **Ephemeral content** (Snapchat, 4chan) reduces perceived consequences

- ****Doxing****: Publicly releasing a victim's private information online. Often part of larger harassment campaigns — releasing home addresses, workplace info. Used by polarised communities (e.g., 4chan's /pol/) for coordinated hate attacks ("raids"). Also employed by hacktivist groups like Anonymous.

- ****Cyberstalking****: Using electronic means to stalk another person. Two types:
  - Using online information to stalk in real life
  - Pure online stalking (passive monitoring or interactive harassment)

- ****Sextortion****: Luring victims to perform sexual acts on camera, recording them, then demanding payment not to release footage.

- ****Child Predation****: Grooming victims online through chats, social networks, or gaming platforms. Offenders use deceptive tactics (pretending to be young people) or distribute child abuse material via peer-to-peer networks and anonymising technologies (Tor).

### 7.1.3 Cyber-Enabled Organised Criminals

Career criminals operating through small organisations or structured criminal groups.

- ****Advance Fee Fraud**** (419 scams): Victim promised a reward but must first pay a small fee. Archetypal examples:
  - **Nigerian prince scams** — large money transfer requiring "legal fees"
  - **Consumer fraud** on classifieds sites (Craigslist) — desirable items at suspiciously low prices
  - **Romance fraud** — criminals pose as attractive individuals on dating sites, build emotional relationships over months, then request money. Victims can lose £50–£240,000; psychological damage often exceeds financial loss.

  Key element: building an **enticing narrative**. Fraudsters target specific demographics and impersonate personas (e.g., military members stationed abroad). Herley argues absurd narratives are intentional — filtering for the most gullible victims.

- ****Online Drug Dealing****: Cryptocurrencies + anonymising technologies (Tor) have enabled online drug marketplaces. Despite frequent takedowns, the business thrives. The "last mile" delivery model has changed, but major producers remain unchanged.

### 7.1.4 Cyber-Dependent Organised Criminals

Complex technical infrastructures (e.g., botnets) required. This complexity has prompted **compartmentalisation** in the criminal ecosystem — each actor specialises in one part of the operation.

- ****Email Spam****: Unsolicited bulk email. Evolved from small operations (1990s) selling goods to today's massive botnet-powered campaigns. Modern spam operations are supported by thriving ecosystems:
  - Renting botnets from infection specialists
  - Purchasing email address lists
  - Joining affiliate programmes (handling advertising, shipments, payments)

  Arms race: anti-spam techniques block most malicious emails. Criminals send tens of billions to stay profitable. Study of Storm botnet: 469M emails → 0.01% reached targets → 0.005% clicked links → only 28 purchases. Yet affiliate programmes made up to $85M over three years — key is **returning customers**.

- ****Phishing****: Emails pretending to be from genuine services, luring victims to fake login pages. Enabled by:
  - **Phishing kits** — software producing realistic-looking pages for popular services
  - **Compromised servers** for hosting (cheaper than purchasing servers)
  - Stolen credentials sold on black markets or exploited directly

- ****Financial Malware****: Stealing financial credentials via malware (Zeus, Torpig). Torpig used **botnet-as-a-service** — specialised criminals hosted infrastructure, others ran infection campaigns. Stolen credentials sold on underground forums; prices vary by record type (dumpz vs. fullz for credit cards).

- ****Click Fraud****: Fraudulent clicks on web advertisements using botnets. ZeroAccess botnet caused ~$100,000/day in advertising losses.

- ****Unauthorised Cryptocurrency Mining****: Using infected computers (or website visitors via cryptojacking scripts) to mine cryptocurrency. Monero mining operations could make up to $18M over two years.

- ****Ransomware****: Encrypts victim's files and demands ransom (typically cryptocurrency) for decryption. The "gold standard" for cybercriminals — solves monetisation problems:
  - No need to convince victims to purchase goods (spam)
  - No need for victims to fall for fraud (phishing)
  - Victims are highly incentivised to pay
  - Traced $16M in Bitcoin payments to ransomware campaigns

  Less sophisticated variants use password-protected bootloaders (easier to mitigate).

- ****DDoS for Hire****: Leveraging botnet bandwidth or amplification attacks. "Stress tester" services advertised to hide illicit nature; used to knock competitors or gaming opponents offline.

### 7.1.5 Hacktivists

Computer crime motivated by **political goals**. Debate exists whether these actions constitute civil disobedience or cyberterrorism.

- **Denial of Service**: Evolved from 1990s "netstrikes." Anonymous popularised using LOIC (Low Orbit Ion Cannon) — volunteers install the program to join collective DDoS attacks.

- ****Data Leaks****: Releasing stolen documents — WikiLeaks, Anonymous exposing KKK member identities.

- ****Web Defacement****: Exploiting vulnerabilities to replace website homepages with politically-charged content. Also used by early-career criminals to "prove their worth."

### 7.1.6 State Actors

Nation-state attacks differ from commodity cybercrime in two ways:
1. **Targeted, not mass-scale** — no need to gather many victims; attacks can be tailored to specific targets, using unique exploits (zero-days) that existing protections won't catch
2. **Patience allowed** — no pressure for fast monetisation; can wait long periods before finalising attack

Three categories:

- ****Sabotage****: Disrupting critical infrastructure electronically. Even air-gapped networks can be attacked. Stuxnet (2010) sabotaged Iranian nuclear centrifuges — textbook example of state-sponsored attack sophistication. Also caused by insider threats (Maroochy Water Services).

- ****Espionage****: Spear-phishing targeting activists and companies, infecting sensitive systems. Security industry terms these long-standing sophisticated attacks ****Advanced Persistent Threats** (APTs)**.

- ****Disinformation****: State-sponsored troll accounts polarising online discussion on social media. Evidence on backend operations (human vs. bot control) remains incomplete.

---

## 7.2 The Elements of a Malicious Operation

Criminal operations require complex infrastructures, driven by:
- **Cost-effectiveness** — maximum profit
- **Resilience** — withstand takedown attempts by law enforcement, security companies, and users

The cybercriminal ecosystem has specialised — different actors trade services on the black market.

### 7.2.1 Affiliate Programmes

Schemes where a main organisation provides brand, order processing, shipments, and payments. Affiliates direct traffic and get a cut of sales. Popular for:
- Counterfeit pharmaceuticals (early email spam)
- Ransomware operations (CryptoWall)

Affiliate programmes also act as **facilitators** — forums where criminals trade services, typically requiring vetting for access.

### 7.2.2 Infection Vectors

Methods to deliver malware to victim computers:

- **Malicious Attachments**: Oldest method — disguising malware as useful email attachments. Requires **social engineering** to convince users to click.

- ****Blackhat Search Engine Optimisation****: Pushing malicious websites high in search rankings, especially around popular events. Achieved by compromising vulnerable websites and adding invisible links.

- ****Drive-by Download** Attacks**: Automated exploitation — victim visits a malicious page containing JavaScript that exploits browser/plugin vulnerabilities, then downloads malware without user interaction. Often hosted on compromised legitimate websites or via **malvertising** (malicious advertisements).

- **Compromising Internet-Connected Devices**: Scanning for vulnerable IoT devices to build large botnets (e.g., Mirai botnet).

### 7.2.3 Infrastructure

- ****Bulletproof Hosting** Providers**: ISPs known for not complying with takedown requests — located in countries with lax legislation or operators bribing local law enforcement. Charge premium prices. Not invincible — can be disconnected by peer ISPs.

- **Command and Control (C&C) Infrastructure**:
  - Originally single servers (single point of failure, easy blacklisting)
  - **Multi-tier botnets**: intermediary relay servers between bots and central C&C
  - **Peer-to-peer botnets**: infected computers with good connectivity elected as relays — reduces costs but vulnerable to infiltration
  - **Fast Flux**: rotating multiple C&C servers rapidly
  - **Domain Flux**: rotating domain names rapidly

### 7.2.4 Specialised Services

- ****Exploit Kits****: Web applications collecting large numbers of exploits, sold on the black market. Fingerprint victim's system, deliver appropriate exploit, instruct download of customer's malware. Solves the problem of exploit obsolescence (vendors patch vulnerabilities).

- ****Pay-Per-Install** (PPI) Services**: Specialist operators maintain stable botnets; other criminals pay to have their malware installed. Offer geographic granularity (bots in developed countries cost more). Provide resilience — if one malware is taken down, updated version can be installed on same machines. Symbiosis common (Pushdo/Cutwail, Mebroot/Torpig).

### 7.2.5 Human Services

Non-technical yet essential elements:

- ****CAPTCHA** Solving Services**: Crowdsourced workers solve CAPTCHAs for automated account creation. Similar services for phone verification codes.

- **Fake Accounts**: Sold on black markets; prices vary by platform. "Reputation boosting" services build contact networks (fake likes, coerced follows).

- **Content Generation**: Workers recruited on underground forums to create spam emails, fake websites, social media content.

- ****Money Mules****: People recruited for money laundering — receive traceable payments, transfer via untraceable means (Western Union), keep percentage. **Reshipping mules** receive goods purchased with stolen credit cards and forward them abroad.

### 7.2.6 Payment Methods

- **Credit Card Processors**: Most popular (95% of spam affiliate programmes). Require banks willing to process payments at higher fees (10-20%). Offer "customer support" to minimise chargebacks.

- **PayPal**: User-friendly but centralised — can track fraud and terminate accounts.

- **Western Union / MoneyGram / Pre-paid Vouchers**: More anonymous, less regulated.

- ****Cryptocurrencies****: Safest for criminals currently. Used for ransomware, drug markets. Still traceable — conversion to real money removes anonymity. Exchanges vulnerable to breaches and "exit scams."

---

## 7.3 Models to Understand Malicious Operations

Understanding complex multi-actor operations is fundamental to developing effective countermeasures. Models drawn from computer security, criminology, and war studies.

### 7.3.1 Attack Trees

Formalised way to visualise system security during an attack. Root = attack goal; children = ways to achieve it. Each node is a sub-goal with further children.

Two node types:
- **OR nodes**: different ways to achieve a goal
- **AND nodes**: steps that must all be completed

Once created, analysts annotate with feasibility, likelihood, or cost estimates, then propagate scores up the tree. Related: **attack graphs** (multiple targets, actors, vectors) and **attack nets**.

### 7.3.2 Kill Chains

Military concept adapted by Hutchins et al. as the ****Cyber Kill Chain**** — seven phases of a malicious operation against computer systems:

| Phase | Description |
|-------|-------------|
| 1. **Reconnaissance** | Identifying targets (network scanning, purchasing email lists) |
| 2. **Weaponisation** | Preparing attack payload (developing exploits, crafting fraud emails) |
| 3. **Delivery** | Transmitting payload (malicious webserver, malvertising, malicious attachments) |
| 4. **Exploitation** | Exploiting target vulnerability (drive-by download, social engineering) |
| 5. **Installation** | Downloading malware (Remote Access Trojan for persistent access) |
| 6. **Command & Control** | Establishing C&C infrastructure and communication protocol |
| 7. **Actions on Objectives** | Monetisation (data theft, ransomware, cryptocurrency mining) |

For each phase, strategies aim to **Detect, Deny, Disrupt, Degrade, Deceive** — patching vulnerabilities, IDS, honeypots.

Gu et al.'s botnet infection model (five phases) became obsolete as botmasters changed methods — illustrating the difficulty of creating resilient behavioural models.

### 7.3.3 Environmental Criminology

Applying established criminology theories to cybercrime. Challenge: the concept of "place" is less defined online.

- ****Routine Activity Theory****: Crime occurs when three elements converge: **motivated offender + suitable target + absence of capable guardian**. Example: botnet activity peaks during daytime when victims are online.

- ****Rational Choice Theory****: Offenders make rational choices. Useful for understanding how criminals react to mitigations and displacement effects.

- ****Pattern Theory of Crime****: Identifies crime-related places:
  - **Crime attractors** — appealing targets (corporations with sensitive data)
  - **Crime generators** — poorly configured easy-to-compromise systems
  - **Crime enablers** — services with poor hygiene

- ****Situational Crime Prevention****: Reducing crime by reducing opportunities. Five mitigation categories:
  1. **Increase effort** — firewalls, automated updates
  2. **Increase risk** — reducing payment anonymity
  3. **Reduce rewards** — blocking suspicious payments
  4. **Reduce provocations** — peer pressure on rogue ISPs/banks
  5. **Remove excuses** — education campaigns, automated redirects

  Two key implementation issues:
  - **Adaptation**: criminals actively circumvent mitigations (arms race) → effective mitigations are those criminals cannot easily react to or where adaptation has financial cost
  - **Displacement**: criminals move operations elsewhere → a mitigation that merely displaces without reducing effectiveness is not worth pursuing

- ****Crime Scripting****: Extrapolating the sequence of steps an adversary performs to commit an offence. Related to kill chains but developed independently in criminology.

### 7.3.4 Modelling the Underground Economy as a Flow of Capital

Following money flow identifies bottlenecks for developing mitigations. Thomas et al.'s model introduces:
- **Profit centres** — where victims transfer new capital into the criminal ecosystem
- **Support centres** — services facilitating the operation for a fee

Money enters through profit centres, is consumed by various actors providing tools/services to each other. Cash-out points (traditional payment methods) are the weakest link — easier for law enforcement to trace.

### 7.3.5 Attack Attribution

Important for law enforcement and governments, but controversial in cyberspace. Attackers can route through proxies/compromised machines in third countries.

- **Code artefacts and exploits** can identify state-sponsored groups — but commoditisation of exploit kits dilutes this signal
- **Zero-day exploits** are unique to an actor, aiding attribution — but once used, they can be intercepted and reused by others, actively misleading attribution
- CIA has collected other nation-states' exploits to make attacks look like another country's work

Rid et al.'s **attribution framework** identifies three layers:
| Layer | Focus | Question |
|-------|-------|----------|
| **Tactical** | Technical aspects of the attack | *How?* |
| **Operational** | High-level architecture, attacker type | *What?* |
| **Strategic** | Motivation behind the attack | *Why?* |

Originally for state-sponsored attacks but applicable to other malicious activity (e.g., online hate attacks).

---

## Conclusion

This chapter surveyed the adversarial behaviours existing on the Internet:
- Various types of malicious operations based on attacker **motivations** and **capabilities**
- Components required to set up **successful malicious operations**
- Modelling techniques from computer science, criminology, and war studies

Having good models is of **fundamental importance** to developing effective mitigations that are difficult for adversaries to circumvent. The commoditisation and specialisation of the cybercriminal ecosystem demand equally sophisticated, multi-disciplinary defence strategies.

---

## Cross-Reference: Topics vs. Reference Material

| Section | Key References |
|---------|---------------|
| 7.1 Characterisation of Adversaries | [694–702, 709, 719, 722, 724, 725, 766, 767, 774, 781, 784] |
| 7.2 Elements of a Malicious Operation | [689, 705, 706, 746, 747, 758, 787–793, 801, 805, 806, 810] |
| 7.3 Models to Understand Malicious Operations | [62, 705, 815–818] |

---

*Source: CyBOK v1.1, Chapter 7 — Adversarial Behaviours, authored by Gianluca Stringhini, Boston University.*
