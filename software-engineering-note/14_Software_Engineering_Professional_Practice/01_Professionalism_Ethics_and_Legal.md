---
tags:
  - professionalism
  - ethics
  - accreditation
  - certification
  - legal
  - software-engineering
source: "SWEBOK v4 Chapter 14 — Professional Practice, ACM/IEEE Code of Ethics"
created: 2026-07-21
---

# Professionalism — Ethics, Accreditation, and Legal Practice

> A professional software engineer's obligations extend beyond technical competence. They encompass ethical responsibility, formal qualification, and legal accountability.

## 1. Codes of Ethics and Professional Conduct

### ACM/IEEE Software Engineering Code of Ethics

The joint ACM/IEEE-CS Code of Ethics defines eight fundamental principles:

| # | Principle | Summary |
|---|---|---|
| **1. PUBLIC** | Act consistently with the public interest | Safety, disclosure of dangers, fair access |
| **2. CLIENT & EMPLOYER** | Act in their best interests consistent with public interest | Avoid conflicts of interest, protect confidential data |
| **3. PRODUCT** | Ensure products meet highest professional standards | Appropriate quality, due diligence, fitness for purpose |
| **4. JUDGMENT** | Maintain integrity and independence in professional judgment | Avoid bribery, disclose conflicts, reject external pressures |
| **5. MANAGEMENT** | Promote ethical approach to software development management | Fair project management, realistic expectations |
| **6. PROFESSION** | Advance integrity and reputation of the profession | Share knowledge, mentor, report violations |
| **7. COLLEAGUES** | Be fair and supportive to colleagues | Credit others' work, fair reviews, assist professional development |
| **8. SELF** | Participate in lifelong learning and promote ethical practice | Maintain competence, understand relevant law |

> [!important] The public interest is paramount. If a client asks you to deploy insecure software that could harm users, your duty to the public (Principle 1) overrides your duty to the client (Principle 2).

### Other Professional Codes

| Body | Code | Focus |
|---|---|---|
| **IFIP** | International Professional Practice Partnership (IP3) | Global standards for ICT professionalism |
| **BCS** | Code of Conduct (4 pillars) | UK chartered institute — public interest, professional competence, duty to profession, duty to relevant authority |
| **ACS** | Code of Professional Conduct | Australian Computer Society — priorities (public interest first), competence, honesty, social implications, professional development |
| **NSPE** | Code of Ethics for Engineers | US professional engineers — fundamental canons covering safety, competence, truthful statements |

### Case Studies in Ethical Reasoning

**VW Emissions Scandal (2015):** Engineers developed software to detect EPA testing and temporarily reduce emissions — normal road performance was 40× legal limits. This violated Principles 1 (public interest), 3 (product quality), and 4 (judgment). Engineers should have refused or reported the work.

**Therac-25 Radiation Overdoses (1985-87):** A race condition in the safety interlock allowed radiation therapy machines to deliver lethal doses. The manufacturer reused code without adequate testing, ignored incident reports, and blamed operators. Violated Principles 1, 3, 5, and 8.

**Boeing 737 MAX MCAS (2018-19):** A single angle-of-attack sensor fed a stall-prevention system that could repeatedly push the nose down. Pilots were not trained on MCAS and the system was classified as non-safety-critical. Design decisions prioritized market competition over safety. Violated Principles 1, 3, and 5.

## 2. Accreditation, Certification, and Licensing

### Distinction

| Type | What It Validates | Who Issues |
|---|---|---|
| **Accreditation** | University programs meet curriculum standards | ABET (US), Engineering Council (UK), signatories to Washington Accord |
| **Certification** | Individual's current competence in a domain | IEEE CSDP, ISC2 (CISSP), ISTQB, PMI (PMP), AWS/Azure/GCP certs |
| **Licensing** | Governmental authorization to practice | Professional Engineering (PE) license — rare in software, required for safety-critical public works |

### Professional Certification Pathways

| Certification | Body | Domain |
|---|---|---|
| **CSDP** (Certified Software Development Professional) | IEEE Computer Society | Broad software engineering competence |
| **CISSP** (Certified Information Systems Security Professional) | ISC² | Information security |
| **ISTQB** (International Software Testing Qualifications Board) | ISTQB | Software testing |
| **PMP** (Project Management Professional) | PMI | Project management |
| **CSSLP** (Certified Secure Software Lifecycle Professional) | ISC² | Secure SDLC |

> [!note] Certification differs from licensing. A certificate says "you demonstrated competence on this date." A license says "you are legally authorized to practice." Most software engineers need the former, not the latter — except when working on safety-critical systems (medical devices, avionics, nuclear, etc.).

### Washington Accord

The Washington Accord (1989) is an international agreement among bodies that accredit engineering programs. Signatories recognize each other's accredited degrees as substantially equivalent. Current signatories include ABET (US), Engineering Council (UK), Engineers Canada, Engineers Australia, and 20+ other nations.

## 3. Legal and Regulatory Issues

### Intellectual Property

| Protection | What It Covers | Duration | Registration |
|---|---|---|---|
| **Patent** | Functional invention (algorithm, method, process) | 20 years | Required — examination process |
| **Copyright** | Expression (source code, documentation) | Life + 70 years | Automatic upon creation |
| **Trade Secret** | Confidential business information | Indefinite (while secret) | No registration — reasonable protection measures |
| **Trademark** | Brand identity (name, logo) | Renewable (10 years) | Registration recommended |

> [!warning] **Key Fact for Software Engineers:** Copyright protects *expression* (your specific code), not *ideas* (the algorithm). Patents protect functional ideas but are costly and take years. Your employer typically owns IP created during employment — read your contract. Open-source licenses (GPL, MIT, Apache) are copyright-based legal instruments.

### Professional Liability and Negligence

Software engineers can be held liable for:
- **Negligence:** Failure to exercise reasonable professional care — e.g., not testing safety-critical code
- **Breach of contract:** Failure to deliver as specified in a contract or SOW
- **Product liability:** Harm caused by defective software (especially in embedded systems)

Defenses include: following industry best practices, maintaining thorough documentation, obtaining liability insurance, and working through a limited-liability entity (LLC/corporation).

### Data Privacy Regulations

| Regulation | Jurisdiction | Key Requirements |
|---|---|---|
| **GDPR** | EU/EEA | Consent, right to erasure, data portability, breach notification within 72 hours, DPO appointment, fines up to 4% global turnover |
| **CCPA/CPRA** | California, USA | Right to know/delete/opt-out of sale, data minimization, fines per violation |
| **HIPAA** | USA (healthcare) | Protected Health Information (PHI) safeguards, audit trails, minimum necessary use |
| **PIPEDA** | Canada | Consent, limited collection, accuracy, safeguards, openness |

### Software Contracts and Employment

- **Work-for-Hire Doctrine:** Code written within scope of employment is owned by the employer (US law)
- **Non-Compete Clauses:** Restrictions on working for competitors after leaving — enforceability varies by jurisdiction
- **Confidentiality/NDA:** Obligation to not disclose proprietary information — survives employment termination
- **Moonlighting Policies:** Restrictions on outside work — must not use employer resources or conflict with duties

## Key Takeaways

1. **Public safety is the paramount ethical obligation** — above employer, client, and self-interest
2. **Know and apply your professional code of ethics** — ACM/IEEE is the minimum standard
3. **Certification demonstrates competence; licensing authorizes practice** — most software engineers need the former
4. **Copyright protects code; patent protects inventions** — understand the distinction
5. **GDPR, CCPA, HIPAA are legal obligations, not optional** — ignorance is not a defense
6. **When asked to do something ethically questionable, say no and document why**

## Related

- [[Professionalism of Software Engineering Overview]] — All professional practice topics
- [[clean-coder/clean-coder Overview]] — Clean Coder: practical professionalism
- [[clean-craftsmanship/clean-craftsmanship Overview]] — Disciplines, standards, and ethics
- [[02_Group_Dynamics_and_Psychology]] — Team psychology and dynamics
- [[03_Communication_Skills]] — Technical communication
