---
title: Decision Rights and Delegation
role: Principal and Distinguished Engineer
capability_area: Decision Governance and Principles
topic: Decision Rights and Delegation
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - principal-engineer
  - governance
  - decision-rights
  - delegation
---

# Decision Rights and Delegation

> **Core skill:** The principal defines who decides what in a large technical organization — architecture, technology selection, standards, and exceptions — and how deep delegation goes, so decisions are made once, at the right level, without remaking them in every meeting.

## Why This Matters

At 500+ engineers, the organization's decision-making defaults to two failure modes without explicit decision rights. In the first, every decision drifts upward: no one is sure who decides, so the principal or CTO is pulled into every consequential choice, becoming the bottleneck. In the second, every decision is remade in every meeting: no one knows a decision was already made, so the same argument replays across forums until exhaustion settles it. Both modes waste the organization's most expensive time — senior technical judgment — on preventable churn.

Decision rights are the fix: a documented framework that names who decides each class of technical decision, how deep delegation goes, what requires escalation, and who may grant exceptions. The framework does not remove judgment — it channels judgment to the right level and makes decisions visible and durable. The principal's role is to design the framework, socialize it, and re-delegate as the organization grows so that the principal's own decision load stays bounded.

## The Decision Rights Framework

The framework assigns each decision class to a role or body, specifies the decision's materiality threshold, and names the escalation path. It is a variant of RACI adapted for technical decision-making at scale.

### Decision Classes

| Decision Class | Description | Typical Decider | Materiality Threshold |
|---------------|-------------|-----------------|----------------------|
| **Architecture** | System structure, boundaries, interfaces, data flow between systems | Team or domain architect within a bounded context; principal or architecture board across contexts | Spans 3+ teams, introduces a new platform, or changes a published interface |
| **Technology selection** | Choice of language, database, framework, infrastructure component | Team, within published standards; principal or governance board for new standard adoption | Introduces a technology not in the current standards portfolio |
| **Standards** | Adoption, revision, or deprecation of an organizational standard | Principal with governance board ratification | Any standard that applies to more than one team |
| **Exceptions** | Deviation from a standard or principle | Team lead or manager for time-boxed, documented exceptions; principal for indefinite or precedent-setting exceptions | Exception duration exceeds one quarter or sets a precedent for other teams |
| **Build vs buy** | Decision to build in-house or procure a solution | Team for tactical tools under a cost threshold; principal or CTO for platforms or strategic capabilities | Annual cost exceeds threshold or decision is difficult to reverse |
| **Sunset and decommission** | Decision to retire a system, service, or technology | System owner for services with no external consumers; principal for platform or shared services | System has active consumers or is referenced in compliance scope |

### The Decision Rights Table

The framework is published as a table that any engineer can consult:

| Decision | Decider | Consulted | Informed | Escalation |
|----------|---------|-----------|----------|------------|
| Service architecture within a bounded context | Team tech lead | Domain architect | Adjacent teams | Principal if the architecture crosses the bounded context boundary |
| Database choice within standards | Team | Platform team (for operational readiness) | None | Principal if the standard does not fit and an exception is needed |
| New programming language adoption | Principal | All senior engineers; platform team; security | CTO; engineering organization | Governance board if contested |
| Standard revision | Principal | Affected teams; platform team | Engineering organization | CTO if the revision is contested at scale |
| Exception to a standard | Team lead | Principal (for awareness, not approval) | Governance board (aggregate reporting) | Principal if exception becomes permanent |

## Delegation Depth

Delegation depth answers: how far down the organization does a decision right travel before it stops? Delegation that is too shallow concentrates decisions at the top; delegation that is too deep produces decisions that conflict because no one sees the whole.

| Delegation Depth | What It Means | When Appropriate |
|-----------------|---------------|------------------|
| **Principal only** | Only the principal or governance board decides | Enterprise-defining choices: new platform, new language, architecture that spans the organization |
| **Domain architect** | Architects within a product domain decide within principles and standards | Cross-team architecture within a domain; decisions that affect 2-5 teams |
| **Team tech lead** | Tech leads decide within standards and published patterns | Service-level architecture; technology choices from the standard portfolio |
| **Individual engineer** | Any engineer decides within clearly bounded constraints | Implementation details; library choices within an approved ecosystem; design patterns |

The rule of delegation: a decision right should sit at the lowest level where the decider can see all the systems affected by the decision. If a team tech lead cannot see the downstream impact on another domain, the decision right belongs at the domain architect level or above.

## Re-Delegation as the Organization Grows

As the organization doubles, the principal must re-delegate: decisions that the principal made at 300 engineers must move to domain architects at 600, and decisions that domain architects made must move to team tech leads. Re-delegation is not abdication — it is scaling.

| Organization Size | Principal Decides | Domain Architects Decide | Team Tech Leads Decide |
|-------------------|-------------------|-------------------------|------------------------|
| 100-300 | Enterprise architecture; standards; new platforms; build vs buy above threshold | None — principal covers this scope | Service architecture within standards |
| 300-800 | Standards; new platforms; enterprise architecture that spans all domains | Cross-team architecture within a domain; domain-specific technology choices | Service architecture; technology choices from standards |
| 800-2000 | Standards strategy; governance model design; enterprise architecture principles | Domain architecture; domain standards proposals; build vs buy within domain | Service architecture; technology choices; implementation patterns |
| 2000+ | Governance model; principle authorship; standards portfolio strategy | All domain decisions within enterprise principles and standards | All team decisions within domain architecture and organizational standards |

The principal's re-delegation signal: when the principal is the bottleneck for a class of decisions that are structurally similar, delegate that class to the level below with clear principles and an escalation path for novel cases.

## Decision Escalation Paths

Escalation is a designed path, not a sign of failure. When a decision cannot be made at its designated level — because it is contested, because it sets a precedent, or because it exceeds the decider's authority — it follows a known escalation path to the next level.

| Escalation Trigger | Escalation Path | What the Escalation Must Include |
|--------------------|-----------------|----------------------------------|
| Decision is contested by an affected team | Decider to domain architect or principal | The decision, the objection, the options considered, and the decider's recommendation |
| Decision sets a precedent for other teams | Decider to principal | The decision, why it sets a precedent, and the decider's assessment of precedent scope |
| Decision exceeds the decider's authority threshold | Decider to the level that holds that authority | The decision, why it exceeds the threshold, and the decider's analysis of options |
| Decision involves novel risk not covered by the risk framework | Decider to principal or risk governance board | The decision, the novel risk, the decider's risk assessment |

Escalation is not failure. Escalation is the system working: the decision right was at the wrong level, and the escalation path moved it to the right level without remaking the decision from scratch.

## Decision Rights Anti-Patterns

| Anti-Pattern | What It Looks Like | Why It Fails | Fix |
|-------------|-------------------|--------------|-----|
| **The implicit decider** | Everyone assumes someone else decides; decisions are made by whoever shows up to the meeting | Decisions are remade, contradicted, or never made | Publish the decision rights table; make it discoverable |
| **The accountable committee** | "The architecture board decides" — a 12-person group with no named decider | The board never actually decides; decisions are deferred or dissolved | Every decision has a named decider; the board advises, the decider decides |
| **Delegation without principles** | "Teams decide their own architecture" with no principles or standards | Teams make conflicting decisions that compound into systemic fragility | Delegate with principles and standards as guardrails |
| **The rubber-stamp escalation** | Every decision escalates to the principal "just to be safe" | The principal is the bottleneck; decision rights framework is ignored | Refuse to decide at the wrong level; send decisions back with guidance |
| **Delegation that never re-delegates** | The same people decide the same class of decisions as the org doubles | Decision volume grows; lead time grows; the deciders burn out | Re-delegate at each organizational doubling |

## Practical Applications

### Decision Rights Checklist

- [ ] Decision classes are defined: architecture, technology, standards, exceptions, build vs buy, sunset
- [ ] Each class has a named decider at each level of the organization
- [ ] Materiality thresholds are explicit: when does a decision escalate?
- [ ] Delegation depth is appropriate for current organization size
- [ ] Escalation paths are documented and known
- [ ] Decision rights are re-delegated at each organizational doubling
- [ ] Decisions are recorded and discoverable so they are not remade

### Decision Rights Template

```markdown
# Decision Rights Framework v[version]

## Decision Classes and Deciders
| Decision Class | Team Tech Lead | Domain Architect | Principal | Governance Board |
|---------------|---------------|-----------------|-----------|-----------------|
| [Class] | [scope] | [scope] | [scope] | [scope] |

## Materiality Thresholds
| Threshold | Description | Escalates To |
|-----------|-------------|-------------|
| Spans 3+ teams | [description] | [role] |

## Escalation Path
1. Decider at designated level decides
2. If contested, precedent-setting, or above threshold: escalate with documented analysis
3. Escalation recipient decides or escalates further
```

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Decision rights that exist only in the principal's head** | Nobody knows who decides; decisions are remade or unmade | Publish the framework; make it the first page of the engineering wiki |
| **Too many decision classes** | The framework is so granular that nobody can navigate it | 5-8 classes maximum; each class must be meaningfully different |
| **Delegation without accountability** | "Teams decide" becomes "nobody is accountable for systemic outcomes" | Delegation with principles, standards, and escalation paths |
| **Principal as the default decider** | Every decision that is unclear escalates to the principal | Refuse to decide at the wrong level; return with guidance on who should decide |
| **Static decision rights** | Framework written once, never updated, irrelevant to current org structure | Review and re-delegate annually or at each organizational doubling |

## Success Indicators

- Engineers can name who decides each class of technical decision affecting their work
- Escalation volume is low; decisions are made at their designated level
- Decisions are not remade in subsequent meetings or forums
- The principal's decision load stays bounded as the organization grows
- Decision rights framework has been updated at least once in the past year

## Related Topics

- [[01_Technical_Principles]]: the principles that guide decisions at every level
- [[02_Architecture_Governance_at_Scale]]: the governance model decision rights operate within
- [[05_Exception_and_Escalation_Management]]: the escalation paths in detail
- [[07_Technology_Advisory_and_Review_Boards]]: the board's role in the decision framework
- [[career-path/11_Engineering_Manager/05_Organizational_Awareness_and_Influence/00_overview|Organizational Awareness and Influence (EM)]]: the manager's parallel decision rights perspective

## Summary

Decision rights transform the principal's judgment from a bottleneck into a framework: a documented assignment of who decides architecture, technology, standards, and exceptions at each level of the organization, with explicit materiality thresholds, delegation depth, and escalation paths. The framework is re-delegated at each organizational doubling so the principal's decision load stays bounded, and decisions are made once, at the right level, and are never remade because the framework makes them visible and durable.