# Penetration Testing Checklist

> **Penetration testing** is an authorized, human-driven attack simulation against your system to find what automated scanners (SAST/DAST/SCA) miss: chained exploits, business logic flaws, and the kind of creative access an actual adversary would attempt.
> The master checklist ([[security]]) §9 says "penetration test schedule." This file is *how* to scope, run, and remediate a pentest.
> Deep references: → [[04 Vulnerability Management]], [[Penetration-Test-Report]] template.
> Last updated: 2026-08-07

---

## 1. What & Why

- [ ] **Core concept** — A pentest is a time-boxed simulation of a real attack. A human tester (internal or external) attempts to exploit vulnerabilities to gain unauthorized access, escalate privileges, or exfiltrate data — all within an agreed scope and rules of engagement.
- [ ] **What pentest is good at** — Chaining multiple vulnerabilities into a working exploit path, business logic abuse (negative quantities, race conditions in payments), bypassing WAF/auth controls, social engineering (phishing pretexts), internal network pivoting.
- [ ] **What pentest is NOT** — A replacement for SAST/DAST/SCA (automated scanners are cheaper and more frequent). A compliance checkbox alone (the report should drive remediation, not sit on a shelf). A one-time activity (threats evolve; pentests are periodic).
- [ ] **Pentest vs DAST vs SAST** — SAST reads code (patterns). DAST scans the running app (known signatures). Pentest chains findings and exploits business logic that no scanner can see.

---

## 2. Scoping the Pentest

> Scoping determines cost, timeline, and value. A poorly scoped pentest wastes budget and finds nothing actionable.

- [ ] **Scope explicitly defined** — What's in scope (the checkout flow, the API, the admin panel) and what's out of scope (third-party SaaS, production databases, employee personal devices). Written down, agreed by both sides.
- [ ] **Test type chosen** —

  | Type | What's Tested | Attacker Position |
  |---|---|---|
  | **Black box** | No internal knowledge | Like an external attacker |
  | **Grey box** | Some knowledge (creds, architecture) | Like a compromised user or partner |
  | **White box** | Full access (source code, infra) | Like an insider or advanced adversary |

  Grey box is the sweet spot for most apps: enough context to go deep, realistic enough to matter.

- [ ] **Environment specified** — Staging/UAT environment preferred. If production is in scope, rules of engagement specify what can/can't be done (no DoS, no data modification, no social engineering of real customers).
- [ ] **Rules of engagement documented** — Time window, permitted techniques, excluded techniques, emergency contacts, data handling rules (what the tester does with found data).
- [ ] **Success criteria defined** — What does a "good" pentest look like? Not "zero findings" (that's a bad test). A good test finds realistic exploit paths and rates them by business risk.

---

## 3. Types of Pentest

| Type | Focus | When |
|---||---|
| **Web/App pentest** | OWASP Top 10, business logic, session management | Before launch, annually |
| **API pentest** | OWASP API Top 10 (BOLA, BFLA, mass assignment, excess data exposure) | When API is customer-facing or handles sensitive data |
| **Network/Infra pentest** | External + internal network, VPN, exposed services, Active Directory | Annually for production infrastructure |
| **Mobile pentest** | OWASP MASVS, client-side storage, certificate pinning, deep linking | Before mobile app launch |
| **Cloud pentest** | IAM, storage exposure, network config, Kubernetes | When moving to cloud or major infra changes |
| **Social engineering** | Phishing, vishing, physical entry | Annually for security awareness |
| **Red team** | Full-spectrum, goal-based (steal X, compromise Y) | Production-grade, mission-critical systems |

- [ ] **Type matches the risk** — A web app needs a web pentest. An API-first product needs an API pentest. A regulated system needs a red team exercise. Don't run a network pentest when your risk is in the API.

---

## 4. Choosing a Tester

| Option | Pros | Cons |
|---|---|---|
| **External firm** | Unbiased, experienced, brings fresh perspectives, compliance-accepted | Cost ($15K–$50K+), scheduling lead time |
| **Internal team** | Deep system knowledge, cheaper, faster turnaround | Blind spots (test what they built), bias |
| **Bug bounty** | Continuous, crowd-sourced, pay-for-results | Noise (low-quality reports), needs triage capacity, no guaranteed coverage |
| **CrowdStrike/F-Secure/etc.** | Structured, automated pentest platforms | May lack depth of a human-driven test |

- [ ] **External for launch / annual** — An unbiased external tester is the gold standard. Required for compliance (SOC 2, PCI-DSS).
- [ ] **Internal for iterative checks** — Between external pentests, the internal team runs targeted checks on new features.
- [ ] **Bug bounty as a complement** — Continuous coverage between formal pentests. HackerOne, Bugcrowd, or a private program. Requires triage SLA.

---

## 5. Preparing for the Pentest

- [ ] **Threat model shared with tester** — The tester gets the architecture diagram, data flow, and known concerns. This isn't "cheating" — it focuses the tester on real risk areas instead of discovering the architecture from scratch.
- [ ] **Test accounts provisioned** — Dedicated test accounts with representative permissions. Not real user accounts, not admin accounts (unless testing admin surface).
- [ ] **Monitoring/alerting informed** — The SOC/blue team knows the pentest is happening. Pentest traffic shouldn't trigger incident response (or if it does, that's a finding about detection gaps).
- [ ] **Backups verified** — If production is in scope, backups are verified restorable before the test begins. The tester won't destroy data — but accidents happen.
- [ ] **Point of contact assigned** — A technical contact who can answer questions during the engagement. Silence from the client side wastes billable hours.

---

## 6. The Pentest Report

> A good pentest report is actionable. It tells you what's wrong, how bad it is, and how to fix it. A bad report is a list of theoretical risks with no exploit proof.

- [ ] **Executive summary** — Non-technical summary of overall security posture and top risks. For leadership.
- [ ] **Vulnerability details** — For each finding:
  - Title, severity (CVSS or Critical/High/Medium/Low)
  - Description (what's wrong)
  - Affected component (URL, endpoint, parameter)
  - **Proof of concept** (step-by-step reproduction — the report is worthless without this)
  - Impact (what an attacker could do)
  - **Remediation recommendation** (specific fix, not "be more secure")
  - References (OWASP, CWE, CVE)
- [ ] **Exploit chains documented** — When the tester chained multiple findings (e.g., IDOR → token leak → admin takeover), the chain is documented, not just individual findings.
- [ ] **Positive findings noted** — What the tester *couldn't* break. Validates existing controls.
- [ ] **Report format** — PDF + machine-readable (SARIF or JSON) for importing into vulnerability management.

---

## 7. Remediation & Follow-Up

- [ ] **Triage within 48 hours** — Each finding classified: confirmed, false positive, or needs investigation. Severity re-assessed in your context (a Critical in the report may be Medium for you if the endpoint is internal-only).
- [ ] **Fix per remediation SLA** — Critical ≤ 7 days, High ≤ 30 days (see [[security]] §5). Pentest findings follow the same SLA as scanner findings.
- [ ] **Retest before closing** — The original tester or internal team verifies the fix. A fix without verification is a hope.
- [ ] **Findings entered into vuln management** — Not just in the PDF report. Entered into the tracker (DefectDojo, GitHub Security Advisories, Jira) with owner, severity, due date.
- [ ] **Lessons fed back to design** — Recurring finding categories (missing authZ, IDOR) feed into security requirements and SAST custom rules. Fix the system, not just the symptom.

---

## 8. Anti-Patterns to Avoid

- [ ] **Pentest as a compliance checkbox** — The report is filed, findings are ignored, nothing changes. The pentest served compliance, not security. Next year's pentest finds the same things.
- [ ] **Black box when you could be grey/white box** — The tester spends 3 of 5 days discovering the architecture. Share context. Focus billable time on finding exploits, not drawing diagrams.
- [ ] **No rules of engagement** — The tester takes down the production database. Nobody knows if it's the pentest or a real attack. The SOC triggers incident response. Chaos.
- [ ] **Scope too broad** — "Test everything" means nothing gets depth. Focus on the highest-risk flows: auth, payments, data access, admin.
- [ ] **No remediation tracking** — The report has 30 findings. Three get fixed. Nobody tracks the other 27. They accumulate as silent debt.
- [ ] **Trusting the report blindly** — The tester rated it Critical, but it's on an internal-only endpoint behind VPN. Re-assess severity in your context. Don't over- or under-react.
- [ ] **One pentest, never again** — Code changes weekly. A pentest from 2 years ago says nothing about today's attack surface. Pentests are periodic.

---

## Quick Sanity Check

- [ ] Scope, type (black/grey/white), environment, and rules of engagement documented
- [ ] Tester selected (external for launch/annual; internal for iterative)
- [ ] Threat model, architecture diagram, and test accounts shared with tester
- [ ] SOC/blue team informed; monitoring won't trigger IR on pentest traffic
- [ ] Report received with PoC for each finding + remediation recommendations
- [ ] Findings triaged within 48 hours; severity re-assessed in context
- [ ] Fixes tracked per remediation SLA; retested before closing
- [ ] Recurring categories fed back into security requirements and SAST rules

---

## Sources

- Master checklist: [[security]] §9.
- Deep references: [[04 Vulnerability Management]], [[Penetration-Test-Report]] template.
- Standards: OWASP Top 10, OWASP API Security Top 10 (2023), OWASP MASVS (Mobile), OWASP WSTG (Web Security Testing Guide), PTES (Penetration Testing Execution Standard).
