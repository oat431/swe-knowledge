---
title: "Risk Identification and Assessment"
role: Technical Program Manager
capability_area: Risk and Issue Leadership
topic: Risk Identification and Assessment
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - tpm
  - risk-management
  - risk-identification
  - risk-assessment
---

# Risk Identification and Assessment

> **Core skill:** Surfacing risks that the program would otherwise discover only when they fire — and scoring every risk on probability, impact, and proximity so that mitigation effort is allocated where it most reduces program exposure.

## Why This Matters

The risks that sink programs are rarely the ones everyone saw coming. They are the quiet ones: the assumption nobody checked, the dependency nobody owned, the constraint nobody surfaced, the decision that drifted past its deadline without a decision. These risks were visible — to someone, somewhere in the program — but they were never named in a forum where they could be assessed and mitigated.

Risk identification is the TPM's listening discipline. It extracts risks from the places they hide: team conversations, architecture reviews, dependency check-ins, stakeholder one-on-ones, and the gaps between workstreams that nobody owns. The TPM asks "what could go wrong?" repeatedly, in every forum, until the answer is "we have named everything we can see, and we will watch for what we cannot."

Assessment is the TPM's prioritization discipline. A program can carry a hundred named risks — but it can actively mitigate perhaps ten. Assessment tells the TPM which ten: the ones with high probability, severe impact, and short proximity to firing. The rest are watched. The top ten are managed.

## Risk Identification Methods

### Structured Identification

| Method | What It Is | Best For | When to Use |
|--------|------------|----------|-------------|
| **Pre-mortem** | "It is three months from now and we failed. What happened?" | Surfacing hidden assumptions and groupthink | Planning; major milestone gates |
| **Checklist-based** | Run a standard risk taxonomy against the program | Ensuring completeness; not missing common categories | Planning; periodic refresh |
| **Workstream walkthrough** | Walk each workstream's plan with its lead: "What could prevent this from happening?" | Workstream-specific risks | Planning; after major plan changes |
| **Dependency analysis** | For each critical dependency: "What if the target team slips, changes the spec, or stops engaging?" | Dependency risks | After dependency mapping; every status cycle |
| **Assumption log review** | Review the RAID log's assumptions section: which assumptions, if wrong, would hurt the program? | Assumption-to-risk conversion | Every status cycle |

### Unstructured Identification

| Source | What to Listen For | Risk Signal |
|--------|-------------------|-------------|
| **One-on-ones** | Hedging language: "should," "hopefully," "if everything goes well" | The person knows a risk but has not named it |
| **Team stand-ups** | Repeated blockers, slowing velocity, rising WIP | Systemic delivery risk |
| **Architecture reviews** | "We think this will scale," "the prototype worked" | Technical uncertainty |
| **Stakeholder conversations** | "I'm counting on this for Q2" — when the program has not committed to Q2 | Expectation misalignment risk |
| **Silence** | A workstream with no risks reported | Risk suppression; dig deeper |

The TPM's antenna is always on. A throwaway comment in a stand-up — "we're a bit behind on the database migration" — becomes a risk entry if the TPM follows up: "How behind? What depends on it? What is the worst case?"

## Risk Taxonomy

A taxonomy ensures the program does not overlook entire categories of risk:

| Category | What It Covers | Example |
|----------|---------------|---------|
| **Technical** | Architecture decisions, technology maturity, performance, security, scalability | "The chosen database may not handle the peak load" |
| **Dependency** | Internal team dependencies, external vendor dependencies, platform dependencies | "Payment processor certification may not complete by launch" |
| **Resource** | Staffing, skills, capacity, key person dependency | "The only engineer who knows the legacy system is leaving" |
| **Schedule** | Estimation uncertainty, parallelization constraints, external date commitments | "The regulatory review may take longer than the two weeks budgeted" |
| **Scope** | Undiscovered complexity, scope creep, stakeholder expectations | "The integration surface may be larger than estimated" |
| **Organizational** | Reorganizations, priority changes, sponsor changes, political shifts | "The Q3 reorg may reassign the program sponsor" |
| **External** | Market changes, regulatory changes, competitor moves, economic conditions | "New data residency regulations may require architecture changes" |
| **Operational** | Deployment, monitoring, incident response, support readiness | "The on-call team may not be trained on the new system before launch" |

The TPM walks the taxonomy against the program at planning and at every major phase gate. Categories with no entries are either genuinely risk-free (rare) or under-identified (common).

## Assessment: The Risk Score

Every identified risk is scored on three dimensions:

### Probability (P)

| Level | Definition | Heuristic |
|-------|------------|-----------|
| **High (70–100%)** | More likely to occur than not | History of this risk firing; weak or no mitigation |
| **Medium (30–70%)** | Could occur; plausible | Has fired before in similar programs; mitigation partial |
| **Low (0–30%)** | Unlikely but possible | No history; strong mitigation in place |

Probability is assessed honestly, not optimistically. If every risk is "low probability," the assessment process is broken.

### Impact (I)

| Level | Definition | Heuristic |
|-------|------------|-----------|
| **High** | Program date moves by more than one reporting period; major scope cut or quality compromise | End-date impact; sponsor visibility |
| **Medium** | Workstream date moves; program absorbs within contingency | Absorbable within program buffers |
| **Low** | Minor disruption; team absorbs without schedule impact | Contained within a single workstream |

Impact is measured against the program's committed outcomes — not against how hard the team will have to work. Heroic effort to absorb a risk is not "low impact"; it is high impact masked by overtime.

### Proximity (X)

| Level | Definition | Heuristic |
|-------|------------|-----------|
| **Imminent** | Trigger expected within the current reporting period | Days to weeks |
| **Near-term** | Trigger expected within the next 1–2 reporting periods | Weeks to a month |
| **Distant** | Trigger beyond two reporting periods | Months away |

Proximity intensifies risk: a medium-probability, high-impact risk that is imminent demands more attention than a high-probability, high-impact risk six months away with mitigation time available.

### The Risk Score

**Risk Score = P × I**, modified by proximity. The TPM uses the score to rank risks for mitigation attention:

| Score | Label | Response |
|-------|-------|----------|
| High P × High I | **Critical** | Active mitigation with dated actions; sponsor visibility; contingency in place |
| High P × Medium I / Medium P × High I | **High** | Active mitigation; reviewed weekly |
| Medium P × Medium I | **Medium** | Mitigation plan; reviewed monthly |
| All others | **Low** | Acknowledged; watched for changes in P, I, or X |

## Group Assessment: Avoiding Optimism Bias

Individual risk assessment is vulnerable to optimism bias — the owner of a risk tends to underestimate its probability (because naming a high-probability risk feels like admitting failure). Group assessment counters this:

| Technique | How It Works |
|-----------|-------------|
| **Anonymous scoring** | Each participant scores P and I independently; results are aggregated |
| **Devil's advocate** | One person is assigned to argue the worst-case for each risk |
| **Reference class** | "In the last three programs that attempted this, how often did it fire?" |
| **Pre-mortem output** | The pre-mortem's "what went wrong" list is scored as risks |

The TPM facilitates, but does not impose, the assessment. The goal is consensus on the honest score — not the optimistic one.

## Practical Applications

- [ ] Risks are identified through structured methods (pre-mortem, taxonomy, walkthrough) and unstructured listening (stand-ups, one-on-ones, reviews)
- [ ] Every risk is scored on probability, impact, and proximity
- [ ] Risk scores drive mitigation priority: critical risks get active mitigation; low risks are watched
- [ ] Group assessment counters optimism bias; scores are honest, not aspirational
- [ ] The risk taxonomy is walked at planning and at every major phase gate

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Over-reliance on one method** | Pre-mortem alone misses checklist risks; checklist alone misses creative risks | Multiple identification methods, structured and unstructured |
| **Optimism bias in scoring** | Every risk is "low probability" until it fires | Group assessment; reference class; anonymous scoring |
| **Ignoring proximity** | A distant high-impact risk gets the same attention as an imminent one | Proximity modifies priority; imminent risks get more attention |
| **No taxonomy** | Whole categories of risk (organizational, operational) are overlooked | Walk a standard taxonomy against the program |
| **One-and-done identification** | Risks identified at planning, never refreshed | Continuous identification; every status cycle surfaces new risks |

## Success Indicators

- The top five risks in the register are different from the top five three months ago (risks are being retired)
- Pre-mortems surface risks that would not have been named otherwise
- The program has risks in every taxonomy category — no category is empty
- Risk scores are stable (not oscillating) — indicating honest, consistent assessment

## Related Topics

- [[01_Risk_vs_Issue_Management]]: the boundary that determines whether the identified item is a risk or an issue
- [[03_Risk_Response_Planning]]: what happens after assessment — mitigation, transfer, avoidance, acceptance
- [[05_RAID_Log_Management]]: the RAID log where risks are registered and tracked
- [[../03_Dependency_Management/02_Dependency_Classification|Dependency Classification]]: the dependency-focused risk classification

## Summary

Risk identification and assessment is the TPM's surfacing and scoring discipline: risks extracted from structured exercises (pre-mortems, taxonomy walks) and unstructured listening (stand-ups, one-on-ones), then scored honestly on probability, impact, and proximity so that mitigation effort goes where it most reduces program exposure. The TPM's test: when a risk fires, was it on the register with a credible score, or did it surprise the program? Surprises are identification failures — and they are the most expensive kind.