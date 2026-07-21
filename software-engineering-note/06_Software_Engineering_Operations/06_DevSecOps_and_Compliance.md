---
tags:
  - devops
  - security
  - devsecops
  - compliance
  - software-engineering-operations
source: "DevOps Handbook Part VI"
aliases:
  - DevSecOps
  - Security in DevOps
  - Compliance as Code
created: 2026-07-21
---

# 06 — DevSecOps & Compliance

> **Source:** The DevOps Handbook — Part VI: The Technical Practices of Integrating Information Security, Change Management, and Compliance
> **Authors:** Gene Kim, Jez Humble, Patrick Debois, John Willis

---

## Overview

Part VI of the DevOps Handbook extends the Three Ways (Flow, Feedback, and Continual Learning & Experimentation) into the domain of **Information Security, Change Management, and Compliance**. Rather than treating security as a gatekeeper at the end of the software development lifecycle, DevSecOps **integrates security objectives into the daily work** of everyone in the technology value stream.

The ratio of engineers in Development, Operations, and Infosec in a typical technology organization is roughly **100:10:1**. Without automation and integration, Infosec can only perform compliance checking — which is the opposite of security engineering. DevSecOps addresses this asymmetry by weaving security into the fabric of the delivery pipeline.

> *"When Infosec is that outnumbered, without automation and integrating information security into the daily work of Dev and Ops, Infosec can only do compliance checking, which is the opposite of security engineering — and besides, it also makes everyone hate us."*
> — James Wickett, creator of Gauntlt

---

## Chapter 22: Information Security as Everyone's Job, Every Day

### 1. Integrate Security into Development Iteration Demonstrations

Invite Infosec to product demonstrations at the end of **each development interval** so they can:
- Understand team goals in the context of organizational goals
- Observe implementations as they are being built
- Provide guidance and feedback at the **earliest stages** of the project — when there is the most time and freedom to make corrections

This approach, termed **"compliance by demonstration"** by Justin Arbuckle (former chief architect at GE Capital), dramatically reduces reliance on static checklists by leveraging Infosec expertise throughout the entire SDLC.

**Key business measurements** that improved at GE Capital:
- Development velocity (speed of delivering features to market)
- Failed customer interactions (outages, errors)
- Compliance response time (lead time from audit request to delivery)

### 2. Integrate Security into Defect Tracking and Post-Mortems

Track all open security issues in the **same work tracking system** that Development and Operations use (e.g., JIRA), ensuring visibility and prioritization against all other work.

At **Etsy**, Nick Galbreath treated security issues as either **P1 (fix immediately)** or **P2 (fix by end of week)**, even for internally-facing applications. Every security issue triggered a **blameless post-mortem**, which:
- Educated engineers on prevention
- Transferred security knowledge across engineering teams
- Created institutional learning

### 3. Integrate Preventive Security Controls into Shared Repositories

Add pre-blessed security artifacts to the shared source code repository:

| Category | Examples |
|----------|----------|
| **Code libraries** | 2FA authentication, bcrypt password hashing, logging |
| **Secret management** | Vault, sneaker, Keywhiz, credstash, Trousseau, Red October |
| **OS packages and builds** | NTP for time sync, secure OpenSSL configurations, OSSEC/Tripwire for file integrity, syslog for centralized ELK |

Provide **base cookbooks or build images** of OS, databases, and infrastructure (NGINX, Apache, Tomcat) in a known, secure, and risk-reduced state. Engineers using pre-blessed libraries don't need separate security design reviews — they're using guidance already created for configuration hardening, database security, key lengths, etc.

### 4. Integrate Security into the Deployment Pipeline

Automate security tests so they run alongside all other automated tests in the deployment pipeline — **ideally on every code commit**, even in the earliest project stages. This provides fast feedback when insecure changes are committed, enabling quick detection and correction as part of daily work.

**Key testing categories:**

| Test Type | Description | Tools |
|-----------|-------------|-------|
| **Static Analysis** | Inspects program code for all possible runtime behaviors — coding flaws, back doors, malicious code ("testing from the inside-out") | Brakeman, Code Climate, banned function searches (e.g., `exec()`) |
| **Dynamic Analysis** | Tests executed while a program is in operation — monitors memory, functional behavior, response time ("testing from the outside-in") | Arachni, OWASP ZAP, Nmap, Metasploit |
| **Dependency Scanning** | Inventories all dependencies for binaries/executables and checks for known vulnerabilities | Gemnasium, bundler-audit (Ruby), Maven (Java), OWASP Dependency-Check |
| **Source Code Integrity & Code Signing** | All commits signed with PGP keys; packages created by CI are signed and their hashes recorded | gpg + git, keybase.io |

**OWASP Cheat Sheet design patterns** include: how to store passwords, handle forgotten passwords, handle logging, and prevent cross-site scripting (XSS).

### 5. Ensure Security of the Application

Focus on both the **happy path** (correctness of functionality) and the **sad/bad paths** (security-related error conditions — SQL injection, buffer overruns, fraud). Automate these as unit/functional tests running continuously in the deployment pipeline.

> **Case Study: Twitter Static Security Testing (2009)**
>
> After high-profile security breaches (including the @BarackObama account hack) led to an FTC consent order, Twitter's Infosec team:
> - Integrated **Brakeman** static code analysis into the build process during a hack week
> - Reduced the rate of vulnerabilities found by **60%** over subsequent years
> - Made security testing part of the earliest development stages
> - Focused on preventing repeated vulnerabilities through tooling
> - Made everything security-related **self-service**
>
> Key principles: preserve trust of Development (minimize false positives), maintain fast flow through Infosec via automation, take a holistic approach (source code + production + customer perspective).

### 6. Ensure Security of the Software Supply Chain

Modern developers assemble software from open source parts, inheriting their vulnerabilities. Key findings from the **2015 Sonatype State of the Software Supply Chain Report**:

- The typical organization relies on **7,601 build artifacts** and uses **18,614 different versions**
- **7.5%** of components have known vulnerabilities
- **66%** of those vulnerabilities are over two years old without resolution
- Of open source projects with known vulnerabilities, only **41%** are ever fixed (average: **390 days**)

**Mitigations:** detect known vulnerabilities in dependencies, choose components with a demonstrated history of quick fixes, identify multiple versions of the same library (especially older vulnerable versions).

The **Verizon PCI Data Breach Investigation Report (2014)** found that **10 vulnerabilities accounted for 97% of exploits** in cardholder data breaches — and **8 of those were over 10 years old**.

### 7. Ensure Security of the Environment

Generate automated tests to ensure:
- All appropriate security settings are applied (configuration hardening, database security, key lengths)
- Environments are scanned for known vulnerabilities (Nmap for open ports, Metasploit for hardening)
- Output is stored in artifact repositories and compared with previous versions to detect undesirable changes

> **Case Study: 18F — Automating Compliance with Compliance Masonry**
>
> US federal agencies spend ~$80B/year on IT. Obtaining an **Authority to Operate (ATO)** typically takes **8–14 months** after "dev complete" and requires implementing 100+ controls from 4,000+ pages of policy documents.
>
> The **18F** team (GSA) created:
> - **Cloud.gov**: A PaaS running on AWS GovCloud that handles the bulk of compliance controls at the infrastructure/platform level, leaving only application-layer controls for teams to document
> - **Compliance Masonry**: Stores System Security Plan (SSP) data in machine-readable YAML, auto-generating GitBooks and PDFs
>
> This dramatically reduces compliance burden and ATO lead time.

### 8. Integrate Information Security into Production Telemetry

> *"Year after year, in the vast majority of cardholder data breaches, the organization detected the security breach months or quarters after the breach occurred... the way the breach was detected was not an internal monitoring control, but was far more likely someone outside of the organization."*
> — Marcus Sachs, Verizon Data Breach researcher

Integrate security telemetry into the **same tools** Development, QA, and Operations use, giving everyone visibility into how applications perform in a hostile threat environment.

**Application-level telemetry to create:**
- Successful and unsuccessful user logins
- User password/email resets
- User credit card changes
- Ratio of unsuccessful to successful login attempts (brute-force detection)

**Environment-level telemetry to monitor:**
- OS changes (production and build infrastructure)
- Security group changes
- Configuration changes (OSSEC, Puppet, Chef, Tripwire)
- Cloud infrastructure changes (VPC, security groups, users/privileges)
- XSS and SQL injection attempts
- Web server errors (4XX and 5XX)

> **Case Study: Etsy Instrumenting the Environment (2010)**
>
> Nick Galbreath embedded security responsibilities throughout the DevOps value stream and created security telemetry displayed alongside Dev and Ops metrics:
> - **Abnormal production program terminations** (segfaults, core dumps triggered from single IPs → vulnerability exploitation)
> - **Database syntax errors** (zero-tolerance — leading attack vector for SQL injection)
> - **SQL injection attempt indicators** (alerting on `UNION ALL` in user-input fields)
>
> Result: developers saw their code being attacked in real-time, which fundamentally changed how they thought about security as they wrote code.

### 9. Protect the Deployment Pipeline

The CI/CD infrastructure itself is an attack surface. Risks include:
- Compromised build servers stealing source code
- Malicious code injected through unit tests (no one reviews test code)
- Unauthorized users gaining access to code or environments

**Mitigation strategies:**
1. **Hardening** continuous build and integration servers with reproducible, automated builds
2. **Code review** of all changes before merging into trunk (pair programming or pull request review)
3. **Instrumenting** the repository to detect suspicious API calls in test code (e.g., unit tests accessing filesystem/network)
4. **Isolation** — every CI process runs in its own container or VM
5. **Read-only** version control credentials for CI systems

---

## Chapter 23: Protecting the Deployment Pipeline — Change Management & Compliance

### 1. Integrate Security and Compliance into Change Approval Processes

Effective change management policies recognize different risk levels. **ITIL** defines three categories:

| Change Type | Risk Level | Approval Required | Examples |
|-------------|------------|-------------------|----------|
| **Standard Changes** | Low | Pre-approved; no per-change approval | Monthly tax table updates, website content/styling changes, well-understood patches |
| **Normal Changes** | Higher | CAB/ECAB review and approval | Large code deployments, infrastructure changes |
| **Urgent Changes** | Potentially high (emergency) | Senior management approval; documentation post-facto | Urgent security patches, service restoration |

### 2. Re-Categorize Lower-Risk Changes as Standard Changes

With a reliable deployment pipeline and a track record of successful, low-drama deployments:
- Show a history of changes over **months or quarters**
- Provide complete list of production issues during the same period
- Demonstrate **high change success rates** and **low MTTR**
- Gain CAB agreement that changes are pre-approved standard changes

Even standard changes must be **visible and recorded** in change management systems (Remedy, ServiceNow). Deployments performed automatically by pipeline tools (Puppet, Chef, Jenkins) should auto-record results. Link change request records to work planning tools (JIRA, Rally) to provide traceability from production deployment → version control → planning tickets.

### 3. Normal Changes: What to Do

For changes that cannot be classified as standard:
- Submit through the **Request for Change (RFC)** process
- Provide: desired business outcomes, planned utility and warranty, business case with risks and alternatives, proposed schedule
- Leverage deployment pipeline artifacts (automated links to defects fixed, features completed)
- Goal: continually demonstrate exemplary track record to eventually gain standard change classification

> **Case Study: Salesforce.com — Automated Infrastructure as Standard Changes (2012)**
>
> After a multi-year DevOps transformation:
> - Reduced deployment lead times from **6 days to 5 minutes** (by 2013)
> - Scaled to process **over 1 billion transactions per day**
> - Integrated automated testing throughout CI/CD using the open-source tool **Rouster** for Puppet module testing
> - Performed **destructive testing** — running services under increasingly high loads until failure — to understand failure modes
> - Infosec collaborated with Quality Engineering from the earliest project stages
>
> **Key outcome:** The change management group declared that **infrastructure changes via Puppet would be treated as standard changes** (no further CAB approvals), while manual infrastructure changes still required approvals. This created further motivation to automate.

### 4. Reduce Reliance on Separation of Duty

Separation of duty is a traditional control to reduce fraud/mistake risk, but it:
- Slows down feedback engineers receive on their work
- Prevents engineers from taking full responsibility for quality
- Reduces organizational learning capability

**Prefer these controls over separation of duty:**
- **Pair programming** — continuous peer review
- **Continuous inspection** of code check-ins
- **Code review** — formal or lightweight pull request reviews

These provide equivalent reassurance and can be demonstrated to satisfy separation of duty requirements if needed.

> **Case Study: Etsy PCI DSS Compliance**
>
> Etsy's PCI-compliant cardholder data environment (CDE) had to be physically and logically separated. The ICHT team modified their CD practices:
> - Designated a single change approver for production deployments
> - Required JIRA review/approval before manual deployment
>
> **Unintended consequences:**
> - "Compartmentalization" — no one could be a full-stack engineer in the PCI environment
> - Fear and reluctance around deployment and maintenance
> - "An impenetrable wall between developers and ops" — tension not seen at Etsy since 2008
>
> **Lesson:** Compliance is achievable with DevOps, but high-trust DevOps team dynamics are fragile — low-trust control mechanisms can erode them.

### 5. Ensure Documentation and Proof for Auditors and Compliance Officers

> *"How many auditors can read code and how many developers have read NIST 800-37 or the Gramm-Leach-Bliley Act? That creates a gap of knowledge, and the DevOps community needs to help bridge that gap."*
> — Bill Shinn, Principal Security Solutions Architect, AWS

**Challenges auditors face with DevOps:**
- Traditional sampling (e.g., "give me screenshots of 1,000 out of 10,000 servers") doesn't work when infrastructure is code and auto-scaling makes servers ephemeral
- Deployment pipelines differ fundamentally from traditional SDLC where one group writes code and another deploys

**Solutions:**
- Involve auditors in the **control design process** using an iterative, sprint-based approach
- Send all data into **telemetry systems** (Splunk, Kibana) so auditors self-serve evidence for any time range
- Derive engineering requirements from the actual regulations (e.g., HIPAA → 45 CFR Part 160 → Subparts A and C of Part 164 → "technical safeguards and audit controls")
- Use the **DevOps Audit Defense Toolkit** — an end-to-end narrative showing how controls are designed, attested, and proven effective

> **Case Study: Relying on Production Telemetry for ATM Systems**
>
> A major US financial services organization detected ATM fraud (a developer planted a backdoor putting ATMs into maintenance mode to steal cash) **not through code review** but through **production telemetry** — someone noticed ATMs in a city entering maintenance mode at unscheduled times during a regular operations review meeting. The fraud was detected before the scheduled cash audit process.
>
> **Lesson:** Production monitoring controls, combined with automated testing, code reviews, and approvals, are more effective at detecting fraud than code reviews alone — especially when perpetrators have sufficient means, motive, and opportunity.

---

## Part VI Conclusion

The DevOps approach to security transforms Infosec from a **gatekeeping silo** into an **integrated capability** embedded throughout the value stream. Key principles:

1. **Security is everyone's job, every day** — not delegated to a separate department
2. **Automate security testing** in the deployment pipeline — static, dynamic, dependency, and environment scanning on every commit
3. **Integrate security telemetry** into the same monitoring tools Dev and Ops already use
4. **Shift compliance left** — involve auditors early, automate evidence collection, make controls self-documenting
5. **Replace low-trust controls** (separation of duty) with high-trust controls (pair programming, code review, automated testing)
6. **Protect the pipeline itself** — the CI/CD infrastructure is a high-value attack surface

Better security ensures we are **defensible and sensible** with our data, that we can **recover from security problems before they become catastrophic**, and — most importantly — that we can make the security of our systems and data **better than ever**.

---

## Key Terminology

| Term | Definition |
|------|------------|
| **Rugged DevOps / DevOpsSec** | Practices incorporating information security objectives into DevOps; Infosec integrated into all stages of the SDLC |
| **Standard Change** | Low-risk, pre-approved change following an established process; no per-change CAB approval needed |
| **Normal Change** | Higher-risk change requiring CAB review and RFC submission |
| **Urgent Change** | Emergency change requiring senior management approval; documentation post-facto |
| **CAB (Change Advisory Board)** | Body that reviews and approves normal changes |
| **ECAB (Emergency CAB)** | Subset of CAB for urgent change approvals |
| **RFC (Request for Change)** | Formal submission documenting business case, risks, schedule for a normal change |
| **ATO (Authority to Operate)** | US federal government authorization to take a system live in production |
| **SSP (System Security Plan)** | Comprehensive description of system architecture, controls, and security posture |
| **GRC (Governance, Risk, and Compliance)** | Traditional Infosec tool category for tracking vulnerabilities (being replaced by Dev/Ops shared tools) |
| **CDE (Cardholder Data Environment)** | PCI DSS scope — people, processes, and technology that store, process, or transmit cardholder data |

---

## Tools Referenced

| Tool | Category | Purpose |
|------|----------|---------|
| Gauntlt | Security Testing | Automated security tests in Gherkin syntax, integrated into deployment pipelines |
| Brakeman | Static Analysis | Ruby on Rails vulnerability scanning |
| OWASP ZAP | Dynamic Analysis | Web application security scanner (Zed Attack Proxy) |
| Nmap | Environment Scanning | Network port scanning to verify only expected ports are open |
| Metasploit | Penetration Testing | Automated penetration testing framework |
| Vault / Keywhiz / credstash | Secret Management | Secure storage for connection settings, encryption keys |
| OSSEC / Tripwire | File Integrity | Monitor file changes in production environments |
| Gemnasium / bundler-audit | Dependency Scanning | Detect known vulnerabilities in dependencies |
| OWASP Dependency-Check | Dependency Scanning | Identify publicly disclosed vulnerabilities in project dependencies |
| Rouster | Functional Testing | Puppet module functional testing (Salesforce open source) |
| Compliance Masonry | Compliance Automation | Machine-readable YAML → auto-generated SSPs (18F open source) |


## Related

- [[Software Engineering Operations Overview]] — All operations topics
- [[01_The_Three_Ways]] — The Three Ways framework
- [[03_Accelerating_Flow]] — Deployment pipeline security
- [[05_Continual_Learning]] — Learning from incidents
