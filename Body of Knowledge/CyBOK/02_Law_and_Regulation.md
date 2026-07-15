---
tags: [law, regulation, compliance, cyber-security, cybok]
---

> *Source: CyBOK v1.1 by NCSC, Chapter 3 — Law and Regulation*

## Purpose

This chapter provides a comprehensive overview of the legal and regulatory landscape relevant to cyber security practitioners. It covers foundational legal concepts, the application of law to cyberspace, jurisdiction, data protection (GDPR), computer crime, contract and tort liability, intellectual property, regulatory frameworks, public international law, ethics, and legal risk management — equipping practitioners with the knowledge needed to identify and manage legal risk in cyber security activities.

---

## Data Protection and Privacy Law (GDPR)

### Foundations

Data protection law evolved from general privacy law and is embodied primarily in the EU **General Data Protection Regulation (GDPR)** (Regulation 2016/679) and Directive 2016/680 (for law enforcement). GDPR has exerted tremendous global influence through adequacy determinations, contractual requirements, and its broad territorial scope.

### Key Definitions

- **Personal Data**: Any information relating to an identified or identifiable natural person. Includes direct identifiers (name, ID numbers) and indirect identifiers (IP addresses, location data, online identifiers). Pseudonymised data remains personal data. Even server logs with IP addresses qualify as personal data.
- **Processing**: Virtually any operation on personal data — collection, storage, alteration, retrieval, disclosure, erasure, etc.
- **Controller vs. Processor**: The controller determines the purposes and means of processing; the processor processes data on behalf of the controller. GDPR has increased processor responsibility compared to prior law.
- **Data Subject**: Only living natural persons (not legal persons or deceased individuals, unless member states extend protections).

### Core Regulatory Principles (GDPR Art. 5)

- Lawfulness, fairness, and transparency
- Purpose limitation
- Data minimisation
- Accuracy
- Storage limitation
- Integrity and confidentiality (security)

**Sensitive personal data** (racial/ethnic origin, political opinions, religious beliefs, trade union membership, genetic/biometric data, health, sex life/orientation) triggers additional protections under Art. 9.

**Consent**: Must be "freely given, specific, informed and unambiguous" (Art. 4(11)). Consent is only one of several lawful bases for processing — legitimate interest, contractual necessity, legal obligation, etc., also apply.

### Appropriate Security Measures (Art. 32)

Controllers and processors must implement "appropriate technical and organisational measures to ensure a level of security appropriate to the risk." Examples include pseudonymisation, encryption, ensuring CIA (confidentiality, integrity, availability) and resilience, and incident recovery plans. Compliance is assessed by reference to the state of the art, costs, and risks. Security certifications help but are not dispositive.

### Data Protection by Design and by Default (Art. 25)

Obligations to embed data protection into system design from the planning phase, before processing commences. Data Protection Impact Assessments (DPIAs) are required for high-risk processing activities (Art. 35).

### International Data Transfers (Arts. 44–49)

A general prohibition on transferring personal data outside the EEA, with several compliance mechanisms:

- **Adequacy decisions**: EU Commission finding that a receiving territory has adequate protections (e.g., Canada, US Privacy Shield framework — though Safe Harbour was invalidated in *Schrems*).
- **Appropriate safeguards**: Binding corporate rules (multinational enterprise governance), approved contract clauses (standard contractual clauses).
- **Derogations**: Data subject consent, contract performance, important public interest, legal claims, protection of vital interests — interpreted narrowly.

### Personal Data Breach Notification (Arts. 33–34)

- Processor must notify controller "without undue delay."
- Controller must notify supervisory authority within 72 hours of becoming aware, unless breach is unlikely to result in risk to data subjects.
- If high risk to data subjects, the controller must communicate to the affected data subjects without undue delay — unless data was encrypted or harm-mitigation measures were in place.

### Enforcement and Penalties (Art. 83)

- Lower tier: up to €10M or 2% of annual worldwide turnover (operational/procedural violations, including security failures).
- Upper tier: up to €20M or 4% of annual worldwide turnover (fundamental violations — data subject rights, lawful basis for processing, export violations).
- Fines must be "effective, proportionate and dissuasive."
- Proposed fines: British Airways £183M (2019), Marriott £99M.

### Jurisdictional Reach (Art. 3)

GDPR applies to:
1. Controllers/processors with an "establishment" in the EU (interpreted broadly — can include a non-EU parent via subsidiary activities).
2. Any person, anywhere, offering goods/services to data subjects in the EU (Art. 3(2)(a)).
3. Any person monitoring the behaviour of data subjects in the EU (Art. 3(2)(b)).
4. Non-EU controllers subject to GDPR must appoint an EU representative (Art. 27).

### Privacy and Electronic Interception

Privacy is a conditional (non-absolute) human right recognised in instruments including the UDHR (Art. 12), ECHR (Art. 8), and the US Fourth Amendment. Rights apply to intangible data as well as physical space. States regulate lawful interception through heterogeneous legal frameworks — some requiring prior judicial authorisation, others with broader delegated authority. Metadata has traditionally received lower protection than content data, though this distinction is increasingly questioned.

---

## Computer Misuse and Cyber Crime

### Crimes Against Information Systems

The Budapest Convention (Council of Europe, 2001) and EU Directive 2013/40 harmonise computer crime laws internationally. Core categories:

1. **Improper Access (Hacking)**: Unauthorised access to computer systems — criminalised regardless of whether access succeeds (CMA 1990 s.1; Budapest Art. 2).
2. **Improper Interference with Data**: Deleting, damaging, altering, or suppressing data — includes malware and ransomware (Budapest Art. 4; Directive Art. 5).
3. **Improper Interference with Systems**: Material degradation of system performance — addresses DoS/DDoS attacks (Budapest Art. 5; Directive Art. 4; CMA s.3).
4. **Improper Interception**: Wrongfully intercepting electronic communications (Budapest Art. 3; Directive Art. 6).
5. **Producing Hacking Tools with Improper Intentions**: Production/distribution of tools intended to facilitate other computer crimes — creates challenges for security testing tool developers (Budapest Art. 6; CMA s.3A).

### Key Legal Instruments

- **UK Computer Misuse Act 1990** — foundational statute, regularly amended.
- **US Computer Fraud and Abuse Act 1984** — federal computer crime law, plus state-level statutes.
- **Budapest Convention** — 44 Council of Europe states + 19 non-European states (Canada, Japan, US) ratified as of July 2019.
- **EU Directive 2013/40** — mandates member states to criminalise attacks against information systems.

### Penalties

Vary widely by jurisdiction. UK CMA: up to 2 years for improper access, up to 5 years for interference, up to 14 years if significant risk/serious damage, up to life imprisonment if serious harm to human welfare or national security (s.3ZA). US federal law: up to 20+ years for serious computer crime violations.

### Research and Development Risks

Security researchers and developers face potential criminal liability for activities including: uninvited remote analysis of third-party servers, Wi-Fi analysis, malware analysis, botnet research, covert intelligence gathering, and producing/distributing testing tools. The law criminalises tool production only when intent to facilitate other violations can be proven.

### Self-Help Dangers

- **Software locks**: Undisclosed or post-facto time-lock devices risk prosecution as computer crimes.
- **Hack-back**: Counter-attacks constitute crimes against information systems and risk both criminal prosecution in multiple states and triggering state countermeasures under international law. No hack-back exceptions have been enacted.

### Warranted State Activity

Activities conducted under state warrants (e.g., UK Investigatory Powers Act 2016) are exempted from computer crime liability when conforming to the warrant's scope.

---

## Intellectual Property

### Overview

Intellectual property rights are **negative rights** — the right to demand that others cease prohibited activity; they do not confer affirmative rights to any action. **Registered rights** (patents, registered trademarks) require state application and examination; **unregistered rights** (copyright) spring into existence automatically.

### Copyright

- **Subject matter**: Literary works, including source and executable software code.
- **Scope**: Protects expression, not ideas/functionality (functionality is the province of patents).
- **Term**: Life of author + 70 years — essentially perpetual by ICT standards.
- **Infringement**: Copying, transmitting, displaying, or translating a significant part; proof of copying inferred from similarity.
- **Fair use/dealing**: Defences available but defined differently across states.
- **Anti-circumvention**: Expanded in the early 2000s — prohibits interfering with technological protection measures (DRM).
- **Criminal liability**: Egregious infringement prosecutable; anti-circumvention violations: up to 5 years (first offence) or 10 years (second) under US law.

### Patents

- A registered right granted state-by-state following application and examination.
- Must be **novel** and involve an **inventive step** (non-obvious).
- Software "as such" and mathematical formulae "as such" are commonly excluded — but inventions embodying these are patentable.
- **Cryptographic patents**: DES, Diffie-Hellman, and RSA were patented; the field remains dense with active patents.
- **Cost**: Application and maintenance fees, plus **mandatory public disclosure** — the application must teach a skilled practitioner how to replicate the invention.
- **Term**: 20 years from filing; examination can take years.
- **Infringement**: Manufacturing, distributing, importing, exporting, or selling — no need to prove copying.

### Trademarks

- Registered (sometimes unregistered) right; granted state-by-state.
- Distinguishes one person's business/products from another's; granted within defined use categories.
- **Term**: 10 years, renewable indefinitely.
- **Domain name conflicts**: Domain names are globally unique; trademarks are not — infringement when domain name is identical/confusingly similar and used within the registered scope.
- **Certification marks**: Used to demonstrate conformity with a standard; registered by a standards body.

### Trade Secrets

- Information that is secret, valuable because secret, and maintained through reasonable efforts.
- Protected under general tort law and increasingly under specific statutes (US Defend Trade Secrets Act 2016, EU Trade Secrets Directive 2018).
- Protected indefinitely as long as secrecy is maintained.
- **Cyber industrial espionage** is believed to be widespread — loss of confidentiality of patentable subject matter before filing destroys patentability.

### Reverse Engineering

Historically accepted practice — scientific study of a publicly sold device to learn its secrets has been "fair game." However, anti-circumvention laws and software licence restrictions have made reverse engineering more difficult. European law grants limited rights to reverse-compile for interoperability purposes.

---

## Regulatory Frameworks

### NIS Directive (EU Directive on Network and Information Systems)

Requires "operators of essential services" to:
- Take appropriate technical and organisational measures to manage network security risks.
- Prevent and minimise incident impact to ensure service continuity.
- Notify competent authorities/CSIRTs of significant incidents without undue delay.

UK implementation delegates oversight to existing industry regulators.

### Industry-Specific Regulation

Financial services regulators, legal and medical professional bodies, and other sector-specific regulators increasingly incorporate cyber security into their frameworks. Multiple breach disclosure requirements operate alongside GDPR obligations.

### EU Cyber Security Act

Establishes a framework for certification of ICT products and services against cyber security standards. Certification marks (a type of trademark) may be used to demonstrate compliance.

### Export Controls on Security Technologies

Dual-use export restrictions apply to cryptographic technologies. Historical challenges (e.g., *Junger v. Daley*, 2000) held US crypto export regulations unconstitutional as applied to source code (protected speech). Current regulations are narrower but still restrict certain cryptographic exports. Violations are prosecutable as crimes.

### State Secrecy Laws

Practitioners employed or engaged by states are subject to laws mandating secrecy of classified information, with severe criminal penalties for violations.

### Internet Intermediary Liability Shields

Policies adopted in the 1990s shield communication service providers from liability for online content in prescribed circumstances:
- **Mere conduit**: Widest exemption.
- **Hosting**: Exempt until aware of illicit content; then must take down "expeditiously."
- **Caching**: Intermediate protection.
EU Ecommerce Directive (Arts. 12-15) and various US subject-specific shields govern these protections. Policy continues to evolve (e.g., 2018 US amendment removing protections for sex trafficking-related claims).

### Electronic Trust Services

Laws governing electronic signatures and identity trust services address:
- Legal equivalence of electronic signatures to wet-ink signatures.
- Rights and responsibilities in PKI-based certificate systems.
- Liability of certificate issuers to relying third parties.
- Duty of care of those who select root trust certificates (e.g., browser developers).

UN encouragement (1996) and state laws have broadly enabled online business, though some areas (wills, immovable property) retain significant requirements of form.

---

## Legal Liability in Cyber Security

### Negligence

Most legal systems recognise a **duty of care** owed to others. Three conditions (English law):
1. Proximity in time and space.
2. Reasonably foreseeable harm to persons in a similar position.
3. Fair and reasonable to impose responsibility.

**Breach** is measured against the "objectively reasonable person" standard — not perfection, but fault-based. Judge Learned Hand's formula (1947): when the burden (cost) of a precaution < probability of loss × magnitude of loss, the reasonable action is to adopt that precaution. "Common practice" ≠ "reasonable practice."

**Potential duties of care** extend to: retail merchants (payment card security), email service providers, business enterprises, software developers (web server software, crypto protocols), trust service providers, and web browser developers.

**Doctrines aiding victims**: *Negligence per se* (violation of a statute or standard as evidence of negligence) and *res ipsa loquitur* (the thing speaks for itself — inference of negligence from circumstances).

### Product Liability (Strict Liability)

Strict liability for defective products causing personal injury, death, or property damage — liability without fault, focusing on the product rather than the tortfeasor's conduct. Software as such may not be a "product," but a software defect can make a physical product defective. Autonomous vehicles, industrial control systems, pacemakers, and IoT devices present growing cyber security-related strict liability risks. The EU Commission (2018) is re-examining whether digital products should be redefined as "products" under product liability law.

### Contract and Security

**Encouraging security through contract**: Supply chain contracts mandate compliance with standards (ISO 27001, PCI DSS). Membership contracts in closed trading/payment systems enforce security through economic incentives (e.g., losing payment rights for non-compliance with authentication standards). PCI DSS is a prominent example — while not law itself, some states have adopted it into legislation.

**Warranties**: Implied quality warranties for goods and services are routinely excluded by ICT vendors and replaced with narrow express warranties — raising questions about whether this encourages or discourages security-conscious development.

**Limitations/exclusions of liability**: Ubiquitous in ICT contracts; more enforceable in B2B than consumer contexts; civil law jurisdictions are generally more restrictive than common law.

### Vicarious Liability

Employers are **strictly liable** for torts committed by employees within the scope of employment. Pleas about reasonable precautions, training, or due diligence are ineffective. The *Morrisons* case (2018) affirmed vicarious liability in a data protection tort action — a disgruntled internal auditor publishing 100,000 employees' salary data led to the employer being held vicariously liable.

### Joint and Several Liability

Any jointly responsible tortfeasor can be required to pay 100% of damages — significant risk when supply chain partners or joint venturers lack financial resources or are resident in foreign states.

### Affirmative Defences

- **Contributory/comparative negligence**: Victim's own negligence contributed to harm — can reduce or eliminate liability.
- **Assumption of risk/consent**: Victim knowingly consented to the risks — useful for penetration testing engagements.
- **State of the art defence** (product liability): Defect was not discoverable given technological state of the art at time of production.

### Conflict of Law — Torts

Within the EU (Rome II Regulation), applicable law is generally the law of the place where damage was suffered (Art. 4(1)). For product liability, the law may be the injured party's habitual residence, place of acquisition, or place of damage (Art. 5).

---

## Related Chapters

- [[00_Introduction_to_CyBOK]] — CyBOK scope, structure, and knowledge area relationships
- [[02_Law_and_Regulation]] — Human factors, usable security, organisational governance
- [[01_Risk_Management_and_Governance]] — Risk assessment, governance frameworks, security management
- [[09_Software_Security]] — Secure development lifecycle, software vulnerabilities
- [[10_Operating_Systems_and_Virtualisation]] — Network security, cryptographic controls, system hardening
