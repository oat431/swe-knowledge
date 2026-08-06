# Threat Modeling Checklist

> **Threat modeling** is the deliberate process of identifying, rating, and treating security threats *before* writing code. It is a session that happens, not a document that exists — though the document captures decisions for future teams.
> The master checklist ([[security]]) §1 says "do threat modeling." This file is *how* to actually do it.
> Deep references: STRIDE, PASTA, attack trees → [[01 Threat Modeling and Risk]].
> Last updated: 2026-08-07

---

## 1. Frame the System (Before You Threat-Model)

- [ ] **Scope defined** — What system / feature / change are we modeling? State the boundary explicitly: "the checkout flow," "the new MCP server," "the auth refresh." Out-of-scope items are written down, not implicit.
- [ ] **Assets inventoried** — What are we protecting? Data (PII, secrets, financial), services (availability-critical), credentials, brand/trust. Rate each asset by value and sensitivity → [[Data-Classification-Schema]].
- [ ] **Actors identified** — Who interacts with the system? Users, admins, service accounts, third-party integrations, unauthenticated visitors, and *adversaries*. Distinguish trusted from untrusted actors.
- [ ] **Data flows mapped** — Draw how data moves: entry point → processing → storage → egress. A diagram (DFD, sequence, or whiteboard photo) makes boundaries visible. Every arrow is a trust transition.
- [ ] **Trust boundaries drawn** — Where does data cross from one trust zone to another? Internet → edge → app → DB → third party. Each boundary gets explicit controls. This is the single most important artifact of threat modeling.
- [ ] **Assumptions stated** — "We assume the internal network is trusted." "We assume the IdP is secure." Write them down — assumptions that go unstated are the ones that bite.

---

## 2. Identify Threats (STRIDE)

> STRIDE is the default methodology — one category per letter. Walk every data flow and trust boundary against each category. For each, ask: "What could go wrong here?"

### S — Spoofing (Stealing identity)

- [ ] **Could an attacker impersonate a user?** — Stolen password, session hijack, JWT replay, credential stuffing.
- [ ] **Could an attacker impersonate a service?** — Stolen API key, spoofed internal service, fake OAuth callback.
- [ ] **Is identity proof appropriate to the action?** — Login for low-value, MFA for admin, hardware key for prod deploy. Match assurance to consequence.
- [ ] **Is mutual authentication needed?** — Service-to-service calls should prove both identities (mTLS, signed JWTs), not just one side.

### T — Tampering (Modifying data or code)

- [ ] **Could data be modified in transit?** — Unencrypted channels, downgraded TLS, man-in-the-middle.
- [ ] **Could data be modified at rest?** — Direct DB access, compromised backup, shared storage with weak permissions.
- [ ] **Could code/config be modified?** — CI/CD pipeline compromise, dependency tampering, IaC drift, unauthorized config change.
- [ ] **Are integrity checks in place?** — Signatures (cosign, sigstore), checksums, HMACs, audit logs on changes.

### R — Repudiation (Denying an action)

- [ ] **Are all critical actions logged with actor + timestamp?** — Login, authZ decision, data access, config change, admin action, payment. No action = no denial.
- [ ] **Are logs tamper-evident?** — Append-only, shipped off-host, WORM storage, signed. A log the attacker can edit is evidence they will delete.
- [ ] **Can a user deny a transaction?** — Digital signatures, non-repudiation tokens, order confirmation flows. Especially for payments and legal commitments.

### I — Information Disclosure (Leaking data)

- [ ] **Could data leak to unauthorized parties?** — IDOR (see below), verbose error messages, exposed logs, client-side secrets.
- [ ] **Could data leak across tenants?** — Shared infrastructure without namespace isolation, metadata filter bypasses in vector DBs.
- [ ] **Could metadata leak?** — Stack traces, API version headers, source maps, directory listings, `/.git/`, `/swagger.json` in prod.
- [ ] **PII handling reviewed** — Collection minimization, masking in logs/UI, retention enforced. → [[Privacy-Impact-Assessment]].

### D — Denial of Service (Disrupting availability)

- [ ] **Could the service be overwhelmed?** — Request flooding, slow-loris, large payload, expensive query, unbounded pagination.
- [ ] **Could a dependency be exhausted?** — DB connection pool, file descriptors, memory, API rate limits, LLM token budget.
- [ ] **Is there a cleanup path?** — Circuit breakers, backpressure, graceful degradation, autoscaling. What happens when a dependency fails?
- [ ] **Economic DoS considered**** — LLM endpoints where cost-per-request is the DoS surface, not just CPU/memory.

### E — Elevation of Privilege (Gaining unauthorized access)

- [ ] **Could a user gain another user's access?** — IDOR/BOLA: `GET /orders/123` without ownership check. OWASP API #1.
- [ ] **Could a user gain admin access?** — BFLA: calling admin endpoints without role check, privilege escalation via mass assignment.
- [ ] **Could injection elevate access?** — SQLi to read users table, command injection to spawn shell, SSRF to reach metadata service.
- [ ] **Could a service account over-reach?** — Over-permissioned IAM roles, broad service account scopes, agent excessive agency.

---

## 3. Identify Threats (Alternative Frameworks)

> STRIDE covers the breadth. These frameworks add depth in specific dimensions. Use them when STRIDE alone feels too shallow.

### Attack Trees

- [ ] **Root goal defined** — State the attacker's objective in one sentence: "Steal customer PII," "Issue fraudulent refunds," "Take down checkout."
- [ ] **Sub-goals decomposed** — What intermediate steps achieve the goal? Branch recursively until leaves are concrete actions.
- [ ] **Each branch rated** — Feasibility, cost to attacker, likelihood, impact. Prune infeasible branches; rank remaining by risk.
- [ ] **AND/OR nodes labeled** — AND: all children required. OR: any child suffices. Makes the logic testable.

### PASTA (Process for Attack Simulation and Threat Analysis)

- [ ] **7 stages walked** — (1) Define objectives → (2) Define technical scope → (3) Decompose app → (4) Threat analysis → (5) Vulnerability analysis → (6) Attack modeling → (7) Risk analysis. Risk-centered, not checklist-centered.

### LINDDUN (Privacy-Focused)

- [ ] **Privacy threats covered** — Linkability, Identifiability, Non-repudiation, Detectability, Disclosure of information, Unawareness, Non-compliance. Use when processing personal data under GDPR/CCPA.

---

## 4. Rate the Risks

- [ ] **Each threat rated** — For each identified threat, assign likelihood (Low/Med/High) and impact (Low/Med/High). Or use CVSS for vulnerability-derived threats.
- [ ] **Likelihood considers** — Attacker access (internet-facing vs. internal?), skill required, exploit availability, existing controls. A CVE with a public PoC is High likelihood.
- [ ] **Impact considers** — Confidentiality (data leaked), Integrity (data tampered), Availability (service down), Financial, Reputational, Regulatory. A payment DB breach is Critical impact.
- [ ] **Uncertainty stated** — If you don't know the exploitability, say so and scope the investigation. Do not inflate confidence.
- [ ] **Risk matrix used** — Plot threats on a likelihood × impact matrix. Top-right (High/High) gets treated first. Bottom-left (Low/Low) may be accepted.

### DREAD (Optional Simplified Rating)

- [ ] **D — Damage potential** — How bad if exploited? (1=minor, 10=catastrophic)
- [ ] **R — Reproducibility** — How easy to reproduce? (1=hard, 10=trivial)
- [ ] **E — Exploitability** — How much effort to exploit? (1=expert, 10=script kiddie)
- [ ] **A — Affected users** — How many impacted? (1=none, 10=all)
- [ ] **D — Discoverability** — How easy to find? (1=hidden, 10=obvious)
- [ ] **Average ≤ 4 → Low · 4–7 → Medium · ≥ 7 → High**

---

## 5. Treat the Risks

> For every identified risk, choose one of four treatments. Document the decision.

| Treatment | When to choose | Example |
|---|---|---|
| **Mitigate** | Risk is real and cost-effective controls exist | Add authZ check, rate limit, encrypt |
| **Avoid** | Risk is too high and controls won't reduce it enough | Don't ship the feature, remove the endpoint |
| **Transfer** | Another party better positioned to bear it | Insurance, third-party pentest, cloud SLA |
| **Accept** | Residual risk is low and justifiable | Document with owner, expiry, review trigger |

- [ ] **Every risk has an owner** — A named person, not "the team." The owner is accountable for the treatment.
- [ ] **Every risk has a deadline** — Time-bound. "Someday" is not a treatment.
- [ ] **Every accepted risk has** — Scope, owner, compensating control (what reduces the risk you're accepting), expiry date, and a review trigger (what event causes re-evaluation).
- [ ] **Treatments have acceptance evidence** — A test, a config, a scan result, a review sign-off. "We added rate limiting" is a claim; "rate limit returns 429 at 100 req/min, tested in CI" is evidence.

---

## 6. Maintain the Threat Model

- [ ] **Refresh triggers defined** — Architecture change, new trust boundary or integration, major dependency change, new compliance requirement, after every security incident. A model that isn't maintained is a fossil.
- [ ] **Version the model** — Date, author, what changed since last version. A model without history is an artifact; a model with history is a decision system.
- [ ] **Threat model reviewed before launch** — The shipped architecture matches what was modeled. If the architecture drifted, update the model before launch.
- [ ] **Threat model accessible to the team** — In the repo, in the wiki, in Confluence — wherever the team works. A model in a security engineer's inbox is useless to developers.

---

## 7. Anti-Patterns to Avoid

- [ ] **No threat modeling done as a session** — A document written by one security engineer is not threat modeling. A whiteboard session with architecture, dev, and security present is.
- [ ] **Threat modeling done once and never refreshed** — The architecture changed three times since the model was written. The model still says "we use JWTs" when the system moved to Paseto.
- [ ] **STRIDE used as a checklist instead of a thinking framework** — Walking the six letters and checking boxes without asking "could this actually happen in *our* system?" misses system-specific threats.
- [ ] **Every threat rated High/High** — If everything is critical, nothing is. Use the risk matrix to differentiate. A threat you rate Low/High is a threat you've thought about.
- [ ] **No treatment decisions recorded** — The model lists 50 threats and no decisions. Untreated risks accumulate as silent debt.
- [ ] **Threat model = pentest report** — Pentest is *verification* of controls; threat modeling is *design* of controls. They feed each other but are not the same activity.

---

## Quick Sanity Check

- [ ] Scope, assets, actors, data flows, trust boundaries all drawn/documented
- [ ] STRIDE walked against every trust boundary
- [ ] Attack tree built for the top 2–3 adversary goals
- [ ] Each threat rated (likelihood × impact) with uncertainty stated
- [ ] Treatment decision recorded for every risk (mitigate/avoid/transfer/accept)
- [ ] Every treatment has an owner, deadline, and acceptance evidence
- [ ] Refresh triggers defined; model is versioned
- [ ] Reviewed before launch against shipped architecture

---

## Sources

- Master checklist: [[security]] §1.
- Deep reference: [[01 Threat Modeling and Risk]].
- Frameworks: STRIDE (Microsoft), PASTA, LINDDUN, Attack Trees (Bruce Schneier).
- Risk templates: [[Risk-Assessment-Report-Security]], [[Risk-Treatment-Plan]], [[Threat-Model]], [[Abuse-Misuse-Cases]].
