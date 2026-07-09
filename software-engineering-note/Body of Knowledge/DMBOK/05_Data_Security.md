---
title: "Data Security"
source: "DMBoK v2 (Chapter 7)"
tags:
  - data-security
  - data-management
  - dmbok
  - privacy
  - data-classification
  - information-security
  - regulatory-compliance
created: 2026-07-09
---

# Data Security

> **DMBoK Definition:** Planning, development, and execution of security policies and procedures to provide proper authentication, authorization, access, and auditing of data and information assets.

## Goals

1. Enable appropriate, and prevent inappropriate, access to enterprise data assets.
2. Understand and comply with all relevant regulations and policies for privacy, protection, and confidentiality.
3. Ensure that the privacy and confidentiality needs of all stakeholders are enforced and audited.

---

## 1. Introduction

**Data Security** includes the planning, development, and execution of security policies and procedures to provide proper authentication, authorization, access, and auditing of data and information assets. The specifics of data security differ between industries and countries. Nevertheless, the goal of data security practices is the same: **to protect information assets in alignment with privacy and confidentiality regulations, contractual agreements, and business requirements.**

These requirements come from:

- **Stakeholders:** Organizations must recognize the privacy and confidentiality needs of their stakeholders, including clients, patients, students, citizens, suppliers, or business partners.
- **Government regulations:** Regulations protect stakeholder interests. Some restrict access to information, while others ensure openness, transparency, and accountability.
- **Proprietary business concerns:** Each organization has proprietary data to protect. Data provides insight into customers and can provide competitive advantage.
- **Legitimate access needs:** Business processes require individuals in certain roles be able to access, use, and maintain data.
- **Contractual obligations:** Non-disclosure agreements and industry standards (e.g., PCI-DSS) demand that certain types of data be protected in defined ways.

### 1.1 Business Drivers

Risk reduction and business growth are the primary drivers of data security activities.

#### 1.1.1 Risk Reduction

As data regulations increase, so do compliance requirements. Security organizations manage IT compliance, policies, practices, [[data-classification|data classifications]], and access authorization rules. The overall process includes:

1. **Identify and classify sensitive data assets** — personal, medical, financial, and more.
2. **Locate sensitive data throughout the enterprise** — a significant amount of sensitive data in a single location poses high risk.
3. **Determine how each asset needs to be protected** — measures vary by data content and technology.
4. **Identify how this information interacts with business processes** — determine what access is allowed and under what conditions.

In addition to classifying data, organizations must assess **external threats** (hackers, criminals) and **internal risks** (employees, processes). Much data is lost through employee ignorance, bypassed security policies, or unenforced controls.

#### 1.1.2 Business Growth

Trusted e-commerce drives profit and growth. Robust information security enables transactions and builds customer confidence. Security itself is a valuable, strategic asset.

#### 1.1.3 Security as an Asset

Security-related [[metadata]] becomes a strategic asset. When sensitive data is correctly identified, organizations build trust with customers and partners. Standard security metadata can optimize data protection and guide business usage, lowering costs and reducing risks.

### 1.2 Goals and Principles

**Guiding Principles:**

- **Collaboration:** Data Security is a collaborative effort involving IT security administrators, [[data-stewardship|data stewards]], internal/external audit teams, and the legal department.
- **Enterprise approach:** Standards and policies must be applied consistently across the entire organization.
- **Proactive management:** Success depends on being proactive, engaging all stakeholders, managing change, and overcoming organizational bottlenecks.
- **Clear accountability:** Roles and responsibilities must be clearly defined, including the 'chain of custody' for data.
- **Metadata-driven:** Security classification for data elements is an essential part of [[data-definition|data definitions]].
- **Reduce risk by reducing exposure:** Minimize sensitive/confidential data proliferation, especially to non-production environments.

---

## 2. Essential Concepts

### 2.1 Vulnerability, Threat, and Risk

| Term | Definition |
|------|-----------|
| **Vulnerability** | A weakness or defect in a system that allows it to be successfully attacked — a hole in defenses. Non-production environments are often more vulnerable than production environments. |
| **Threat** | A potential offensive action against an organization. Can be internal or external, malicious or unintentional. Also called an *attack surface*. |
| **Risk** | The possibility of loss and the condition that poses the potential loss. Calculated based on probability, damage amount, effect on revenue, cost to fix, cost to prevent, and attacker intent. |

### 2.2 Risk Classifications

Risk classifications describe data sensitivity and likelihood of being sought for malicious purposes:

- **Critical Risk Data (CRD):** Personal information aggressively sought for unauthorized use due to high direct financial value. Compromise would cause significant financial harm.
- **High Risk Data (HRD):** Actively sought for unauthorized use due to potential direct financial value. Provides competitive edge; compromise leads to loss of opportunity.
- **Moderate Risk Data (MRD):** Company information with little tangible value to unauthorized parties, but unauthorized use would likely have negative effect.

### 2.3 Data Security Organization

Depending on enterprise size, the Information Security function may be a dedicated group within IT. Larger enterprises often have a **Chief Information Security Officer (CISO)**. In organizations without dedicated personnel, responsibility falls on data managers.

> Data managers need to be actively engaged with IT developers and cyber security professionals so that regulated data may be identified, sensitive systems properly protected, and user access controls designed.

### 2.4 Security Processes — The Four A's + E

1. **Access:** Enable individuals with authorization to access systems in a timely manner.
2. **Audit:** Review security actions and user activity to ensure compliance with regulations and conformance with policy.
3. **Authentication:** Validate users' access. Passwords, security tokens, biometrics, or multi-factor identification.
4. **Authorization:** Grant privileges to access specific views of data, appropriate to role.
5. **Entitlement:** The sum total of all data elements exposed to a user by a single access authorization decision.

**Monitoring** — both active (real-time detection) and passive (trend-based assessment) — is also essential.

### 2.5 Data Integrity

In security, **data integrity** is the state of being whole — protected from improper alteration, deletion, or addition. For example, [[Sarbanes-Oxley|SOX]] regulations focus on protecting financial information integrity by defining rules for how financial information can be created and edited.

### 2.6 Encryption

Encryption translates plain text into complex codes. Four main methods:

| Method | Description |
|--------|------------|
| **Hash** | Converts data into a mathematical representation (MD5, SHA). Used for verification of transmission integrity or identity. |
| **Private-key** | Uses one key to encrypt; sender and recipient must have the key (DES, 3DES, AES, IDEA). |
| **Public-key** | Sender uses freely available public key; receiver uses private key (RSA, Diffie-Hellman). Useful for many-to-few data flows. |

### 2.7 Obfuscation / Masking

Data can be made less available by obfuscation or masking — removing, shuffling, or changing data appearance without losing meaning or relationships.

**Persistent Data Masking:** Permanently and irreversibly alters data. Used between production and non-production environments.
- *In-flight masking:* Data masked while moving between environments.
- *In-place masking:* Data masked in the same location (risky if process fails).

**Dynamic Data Masking:** Changes data appearance to end users without changing underlying data (e.g., showing `***-**-6789` for SSN).

**Masking Methods:** Substitution, Shuffling, Temporal Variance, Value Variance, Nulling/Deleting, Randomization, Encryption, Expression Masking, Key Masking.

### 2.8 Network Security Terms

- **Backdoor:** Overlooked or hidden entry into a system, bypassing password requirements.
- **Bot/Zombie:** A workstation taken over by a malicious hacker. A **Bot-Net** is a network of infected machines.
- **Cookie:** Small data file installed by websites to identify returning visitors.
- **Firewall:** Software/hardware filtering network traffic to protect from unauthorized access.
- **Perimeter:** Boundary between an organization's environments and exterior systems.
- **DMZ:** De-militarized zone — area on the perimeter with firewalls on both sides, used to pass data between organizations.
- **Super User Account:** Administrator/root access account used only in emergencies, with short-lived credentials.
- **Key Logger:** Attack software recording keystrokes and sending them elsewhere.
- **Penetration Testing:** Ethical hacking to identify system vulnerabilities before release.
- **VPN:** Virtual Private Network — creates a secure, encrypted tunnel into an organization's environment.

### 2.9 Types of Data Security

- **Facility Security:** Locked data centers with restricted access. Humans are the weakest point.
- **Device Security:** Mobile devices (laptops, tablets, smartphones) are inherently insecure. Standards include access policies, storage restrictions, data wiping, anti-malware, and encryption.
- **Credential Security:** User ID + Password combinations. 
  - **Identity Management Systems:** Enable single-sign-on.
  - **Password Standards:** Strong passwords, regular changes (45-180 days), no blank passwords.
  - **Multiple Factor Identification:** Hardware tokens, biometrics for highly sensitive data.
- **Electronic Communication Security:** Training to avoid sending confidential information over email or social media.

### 2.10 Types of Data Security Restrictions

Two concepts drive security restrictions:

| Type | Origin | Description |
|------|--------|-------------|
| **Confidentiality** | Internal | Secret/private. Shared on a 'need-to-know' basis. One level per data set. |
| **Regulation** | External | Laws, treaties, industry regulations. Shared on an 'allowed-to-know' basis. Categories are additive. |

#### Confidentiality Classification Levels

- **For General Audiences:** Available to anyone, including the public.
- **Internal Use Only:** Limited to employees or members; minimal risk if shared.
- **Confidential:** Cannot be shared outside the organization without an NDA.
- **Restricted Confidential:** Limited to individuals with 'need to know'; may require clearance.
- **Registered Confidential:** Requires signed legal agreement to access; individual assumes responsibility for secrecy.

#### Regulatory Families

- **Personal Identification Information (PII/PPI):** Name, address, government ID, account numbers, etc. Covers EU Privacy Directives, Canadian PIPEDA, PCI standards, FTC requirements, GLB.
- **Financially Sensitive Data:** All financial information including non-public reports, merger plans, sales data. Covered by Insider Trading Laws, SOX, GLBA.
- **Medically Sensitive Data / PHI:** Health/medical information. Covered by HIPAA in the US.
- **Educational Records:** Covered by FERPA in the US.
- **Industry/Contract-based:** PCI-DSS, trade secrets, contractual restrictions with vendors and partners.

### 2.11 System Security Risks

- **Abuse of Excessive Privilege:** Users granted more access than their job requires. Solution: query-level access control (row/column level).
- **Abuse of Legitimate Privilege:** Users abusing authorized access for unauthorized purposes (intentional or unintentional).
- **Unauthorized Privilege Elevation:** Attackers exploiting software vulnerabilities to gain administrator access.
- **Service Account / Shared Account Abuse:** Untraceable generic accounts increase breach risk.
- **Platform Intrusion Attacks:** Requires regular patches and Intrusion Prevention Systems (IPS).
- **SQL Injection Vulnerability:** Inserting unauthorized database statements into vulnerable SQL channels.
- **Default Passwords:** Must be eliminated after every implementation.
- **Backup Data Abuse:** Encrypt all database backups; securely manage decryption keys.

### 2.12 Hacking / Social Threats / Malware

- **White Hat Hacker:** Ethical hacker who improves system security.
- **Black Hat Hacker:** Malicious hacker seeking financial or personal information.
- **Social Engineering / Phishing:** Tricking people into providing information or access.
- **Malware Types:** Adware, Spyware, Trojan Horse, Virus, Worm.
- **Malware Sources:** Instant Messaging, Social Networking Sites, Spam.

---

## 3. Activities

### 3.1 Identify Data Security Requirements

#### 3.1.1 Business Requirements
Analyze business rules and processes to identify security touch points. Data-to-process and data-to-role relationship matrices ([[CRUD]]) map needs and guide definition of role groups, parameters, and permissions.

#### 3.1.2 Regulatory Requirements
Create a central inventory of all relevant data regulations and affected data subject areas. Key regulations include:

- **US:** Sarbanes-Oxley (SOX), HIPAA, HITECH, Gramm-Leach-Bliley (GLB), FISMA, California SB 1386
- **EU:** Data Protection Directive (EU DPD 95/46/), Basel II Accord
- **Canada:** Bill 198 (PIPEDA)
- **Industry:** PCI-DSS, FTC Standards for Safeguarding Customer Info

### 3.2 Define Data Security Policy

Policies describe behaviors in the best interests of the organization. They must be auditable and audited.

**Policy levels:**
- **Enterprise Security Policy:** Facility access, email standards, breach reporting
- **IT Security Policy:** Directory structures, password policies, identity management
- **Data Security Policy:** Application/database roles, user groups, information sensitivity

### 3.3 Define Data Security Standards

#### 3.3.1 Define Data Confidentiality Levels
Each organization should create or adopt a classification scheme meeting business requirements. Any classification should be clear and easy to apply.

#### 3.3.2 Define Data Regulatory Categories
Group similar regulations into categories/families. Most data regulations seek to do the same thing. The results should be a formally approved set of security classifications and regulatory categories, captured as [[metadata]] in a central repository.

#### 3.3.3 Define Security Roles
Role-based access control is preferred for larger organizations. Two approaches:
- **Role Assignment Grid:** Maps access roles based on data confidentiality, regulations, and user functions.
- **Role Assignment Hierarchy:** Child roles further restrict privileges of parent roles.

#### 3.3.4 Assess Current Security Risks
Identify where sensitive data is stored and what protections are required. Evaluate each system for:
- Sensitivity of data stored or in transit
- Requirements to protect that data
- Current security protections in place

#### 3.3.5 Implement Controls and Procedures
Controls should cover:
1. How users gain/lose access to systems
2. How users are assigned to/removed from roles
3. How privilege levels are monitored
4. How access change requests are handled
5. How data is classified by confidentiality and regulations
6. How data breaches are handled

**Key sub-activities:**
- **Assign Confidentiality Levels:** Data Stewards evaluate and determine appropriate levels.
- **Assign Regulatory Categories:** Information must be assessed and classified within the schema.
- **Manage and Maintain Data Security:** Control data availability via user entitlements, masking, and views.
- **Monitor User Authentication and Access Behavior:** Reporting on access is a basic compliance audit requirement.
- **Manage Security Policy Compliance:** Ongoing activities to ensure policies are followed.
- **Manage Regulatory Compliance:** Measure compliance, ensure auditability, protect regulated data.
- **Audit Data Security and Compliance Activities:** Regular internal audits by independent auditors.

---

## 4. Tools

| Tool | Purpose |
|------|---------|
| **Anti-Virus / Security Software** | Protects computers from viruses and malware |
| **HTTPS** | Encrypted security layer for web communications |
| **Identity Management Technology** | Stores credentials and shares them with systems (LDAP, single-sign-on) |
| **Intrusion Detection/Prevention Software (IDS/IPS)** | Detects incursions and dynamically denies access |
| **Firewalls** | Filters network traffic at enterprise gateway |
| **Metadata Tracking Tools** | Tracks movement of sensitive data; identifies data requiring protection |
| **Data Masking/Encryption Tools** | Restricts movement of sensitive data |

---

## 5. Techniques

### 5.1 CRUD Matrix Usage
Create data-to-process and data-to-role relationship (CRUD — Create, Read, Update, Delete) matrices to map data access needs and guide role group definitions. Some versions add **E** for Execute (**CRUDE**).

### 5.2 Immediate Security Patch Deployment
Install security patches as quickly as possible on all machines. A malicious hacker only needs root access to one machine to attack the network.

### 5.3 Data Security Attributes in Metadata
A [[metadata]] repository is essential for integrity and consistent use of the [[enterprise-data-model]]. Security and regulatory classifications should be documented in the Metadata repository and tagged to the data. These classifications inform development teams about risks related to sensitive data.

### 5.4 Metrics

#### 5.4.1 Security Implementation Metrics
- Percentage of computers with most recent security patches
- Percentage of computers with up-to-date anti-malware
- Percentage of new-hires with successful background checks
- Percentage of employees scoring >80% on annual security quiz
- Percentage of business units with formal risk assessment completed
- Percentage of audit findings successfully resolved

#### 5.4.2 Security Awareness Metrics
- Risk assessment findings, risk events and profiles
- Formal feedback surveys and interviews
- Incident post-mortems and lessons learned
- Patching effectiveness audits

#### 5.4.3 Data Protection Metrics
- Criticality ranking of specific data types and systems
- Annualized loss expectancy of data loss, compromise, or corruption
- Risk of specific data losses related to regulated information categories
- Risk mapping of data to specific business processes
- Threat assessments based on likelihood of attack
- Vulnerability assessments of business process touch points
- Auditable list of locations where sensitive data is propagated

#### 5.4.4 Security Incident Metrics
- Intrusion attempts detected and prevented
- Return on Investment for security costs (savings from prevented intrusions)

#### 5.4.5 Confidential Data Proliferation
Measure the number of copies of confidential data. The more places confidential data is stored, the higher the risk of a breach.

### 5.5 Security Needs in Project Requirements
Every project involving data must address system and data security. Identify detailed requirements in the analysis phase. Building compliance into architecture from the start prevents costly retrofitting.

### 5.6 Efficient Search of Encrypted Data
Reduce the amount of data needing decryption by encrypting search criteria using the same method used for the data, seeking matches on encrypted values, then decrypting only the result set.

### 5.7 Document Sanitization
The process of cleaning [[metadata]] (e.g., tracked change history, comments) from documents before sharing. Mitigates risk of sharing confidential information embedded in documents, especially important for contracts and negotiations.

---

## 6. Implementation Guidelines

### 6.1 Readiness Assessment / Risk Assessment
Organizations can increase compliance through:
- **Training:** Mandatory security training at all levels, with evaluation mechanisms.
- **Consistent policies:** Data security policies that complement and align with enterprise policies.
- **Measure benefits:** Include objective metrics for security activities in balanced scorecards.
- **Vendor requirements:** Include data security in SLAs and outsourcing contracts.
- **Build urgency:** Emphasize legal, contractual, and regulatory requirements.
- **Ongoing communications:** Continual employee security-training programs.

### 6.2 Organization and Cultural Change
Organizations must develop policies enabling goals while protecting sensitive information. Data Stewards are responsible for data categorization; Information Security teams assist with compliance enforcement. Well-planned security measures should make secure access *easier* for stakeholders, not harder.

### 6.3 Visibility into User Data Entitlement
Each user data entitlement must be reviewed during system implementation to determine if it contains regulated information. Classification of regulatory sensitivity should be a standard part of the [[data-definition|data definition]] process.

### 6.4 Data Security in an Outsourced World

> **Anything can be outsourced except liability.**

Outsourcing increases the number of people sharing accountability across organizational and geographic boundaries. Risks include loss of control over the technical environment and people working with data.

**Control mechanisms:**
- Service Level Agreements (SLAs)
- Limited liability provisions
- Right-to-audit clauses
- Defined consequences for breaching contractual obligations
- Frequent data security reports and auditing
- Independent monitoring of vendor system activity
- Constant communication
- Awareness of legal differences across jurisdictions

**Key tools:** [[CRUD]] matrices mapping data responsibilities across business processes, applications, roles, and organizations. **RACI matrices** (Responsible, Accountable, Consulted, Informed) clarify roles and responsibilities, including data security obligations.

### 6.5 Data Security in Cloud Environments

Cloud computing extends data boundaries beyond the organization. Data security policies must account for data distribution across different service models ([[SaaS]], [[PaaS]], [[IaaS]], [[DaaS]]).

**Key considerations:**
- Shared responsibility: defining chain of custody, ownership, and custodianship rights
- Infrastructure considerations: Who manages firewalls, access rights on servers?
- Business partner cloud usage puts your data in the cloud too
- Internal cloud data-center architecture should follow the same security policy as the rest of the enterprise

---

## 7. Data Security Governance

### 7.1 Data Security and Enterprise Architecture
[[Enterprise Architecture]] defines information assets, their interrelationships, and business rules. **Data Security Architecture** describes how security is implemented to satisfy business rules and external regulations. Architecture influences:

- Tools used to manage data security
- Data encryption standards and mechanisms
- Access guidelines for external vendors and contractors
- Data transmission protocols over the internet
- Documentation requirements
- Remote access standards
- Security breach incident-reporting procedures

Security architecture is particularly important for integration between internal systems, external business partners, and regulatory agencies.

---

## 8. References

- Andress, Jason. *The Basics of Information Security.* Syngress, 2011.
- Calder, Alan, and Steve Watkins. *IT Governance: An International Guide to Data Security and ISO27001/ISO27002.* 5th ed. Kogan Page, 2012.
- Fuster, Gloria González. *The Emergence of Personal Data Protection as a Fundamental Right of the EU.* Springer, 2014.
- Harkins, Malcolm. *Managing Risk and Information Security: Protect to Enable.* Apress, 2012.
- Hayden, Lance. *IT Security Metrics: A Practical Framework for Measuring Security and Protecting Data.* McGraw-Hill, 2010.
- Kennedy, Gwen, and Leighton Peter Prabhu. *Data Privacy: A Practical Guide.* Interstice Consulting LLP, 2014.
- Murdoch, Don. *Blue Team Handbook: Incident Response Edition.* 2nd ed. CreateSpace, 2014.
- NIST (National Institute for Standards and Technology). <http://bit.ly/1eQYolG>
- Rao, Umesh Hodeghatta and Umesha Nayak. *The InfoSec Handbook.* Apress, 2014.
- Ray, Dewey E. *The IT Professional's Merger and Acquisition Handbook.* Cognitive Diligence, 2012.
- Schlesinger, David. *The Hidden Corporation: A Data Management Security Novel.* Technics Publications, 2011.
- Singer, P.W. and Allan Friedman. *Cybersecurity and Cyberwar: What Everyone Needs to Know.* Oxford University Press, 2014.
- Watts, John. *Certified Information Privacy Professional Study Guide.* CreateSpace, 2014.
- Williams, Branden R., and Anton Chuvakin. *PCI Compliance.* 4th ed. Syngress, 2014.

---

## See Also

- [[DMBOK/04_Data_Modeling_and_Design|Data Modeling and Design]]
- [[DMBOK/03_Data_Architecture|Data Architecture]]
- [[DMBOK/02_Data_Governance|Data Governance]]
- [[DMBOK/06_Data_Storage_and_Operations|Data Storage and Operations]]
- [[DMBOK/08_Data_Integration_and_Interoperability|Data Integration and Interoperability]]
- [[DMBOK/11_Metadata_Management|Metadata Management]]
- [[data-classification]]
- [[privacy]]
- [[regulatory-compliance]]
- [[risk-management]]
