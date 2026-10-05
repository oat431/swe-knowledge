---
title: "Interface Contracts"
role: Technical Program Manager
capability_area: Dependency Management
topic: Interface Contracts
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - tpm
  - dependency-management
  - interface-contracts
---

# Interface Contracts

> **Core skill:** Establishing and tracking formal contracts between dependent teams — the agreed deliverable, interface specification, timing, acceptance criteria, and change process — so that cross-team work proceeds on explicit agreements, not assumptions.

## Why This Matters

The most expensive words in a multi-team program are "I thought you were handling that." One team builds against an API they assumed would be ready; the providing team changed the schema without telling anyone. The integration fails in the final week. The postmortem reveals that both teams were acting reasonably — they just had different assumptions about what was promised.

Interface contracts eliminate this class of failure. They make explicit what each side commits to: the deliverable, the specification, the date, the acceptance criteria, and — critically — the process for changing any of those things. A dependency without a contract is a dependency by assumption. A dependency with a contract is a dependency by agreement.

Contracts are not bureaucracy. They are the minimum viable structure that lets two autonomous teams coordinate without constant renegotiation. The TPM owns the contract — not the contents (the teams own that), but the existence, the tracking, and the change process.

## What Goes in an Interface Contract

| Element | Description | Example |
|---------|-------------|---------|
| **Parties** | Source team (consumer) and target team (provider) | "Mobile App Team (consumer) ← Platform Auth Team (provider)" |
| **Deliverable** | What the provider will deliver | "OAuth 2.0 token endpoint: POST /auth/token" |
| **Specification** | The technical interface: API schema, data format, protocol, SLA | "OpenAPI 3.0 spec; JSON response; 99.9% uptime; <200ms p95 latency" |
| **Delivery date** | When the provider commits to have it ready | "2026-11-01 — integration-ready in staging" |
| **Acceptance criteria** | How the consumer will verify the deliverable meets the spec | "All endpoints pass the provided Postman collection; auth flow completes end-to-end" |
| **Dependencies of the dependency** | What the provider needs from others to deliver | "Infrastructure team must provision auth cluster by 10/15" |
| **Change process** | How either side proposes and agrees to changes | "Change request → TPM-facilitated impact assessment → joint sign-off within 5 business days" |
| **Escalation path** | What happens if either side cannot meet the contract | "TPM facilitates resolution; escalated to program sponsor if unresolved in one week" |
| **Sign-offs** | Names and dates of agreement from both sides | "Alice (Mobile TL), Bob (Platform EM) — agreed 2026-10-01" |

The contract is proportional to the dependency's criticality (see [[02_Dependency_Classification]]). A critical dependency gets every field filled with precision. A low-criticality dependency may need only the deliverable, date, and a handshake — but it still needs those three written down.

## The Contracting Process

### Phase 1: Initiation

The TPM identifies a dependency that needs a contract (critical or high criticality) and brings the two teams together. The agenda is not "sign this contract" — it is "align on what each side needs and can commit to." The TPM facilitates; the teams negotiate the contents.

### Phase 2: Negotiation

| Step | What Happens | TPM's Role |
|------|-------------|------------|
| Provider proposes | Target team states what they can deliver and by when | Reality-check against program schedule; surface constraints |
| Consumer validates | Source team confirms the deliverable meets their need | Ensure acceptance criteria are testable, not vague |
| Gap resolution | Differences between what is needed and what is offered | Facilitate trade-offs: date, scope, quality, alternatives |
| Commitment | Both sides agree to the contract terms | Document the agreement; capture sign-offs |

The TPM does not impose terms. The TPM surfaces gaps and facilitates resolution. A contract imposed by the TPM is a contract neither side owns — and it will fail the moment pressure increases.

### Phase 3: Ratification

The contract is documented, signed off by both sides, and registered in the dependency map. It becomes the reference point for every subsequent status check: "Is the deliverable on track against the contracted spec and date?"

### Phase 4: Change Management

Contracts change — requirements evolve, timelines shift, technical constraints emerge. The contract's change process exists so that changes are negotiated, not assumed.

| Change Trigger | Process |
|----------------|---------|
| Provider needs to change spec or date | Provider raises change request; TPM facilitates impact assessment with consumer |
| Consumer needs additional scope | Consumer raises change request; provider estimates impact on date and existing commitments |
| External event forces change | TPM brings both sides together; renegotiate with new constraints |

Every change to a contract is documented with a date and rationale. A contract that has been silently amended six times without documentation is worse than no contract — it is a contract that both sides believe says different things.

## Contract Types by Dependency Type

| Dependency Type | Contract Focus | Key Fields |
|-----------------|---------------|------------|
| **Technical** | API spec, data contract, SLA, test suite | Specification, acceptance criteria |
| **Organizational** | Decision, approval, resource allocation | Decision date, decision-maker, escalation path |
| **Resource** | Capacity, people, environments | Quantity, duration, start date, contention resolution |
| **External** | Vendor deliverables, regulatory approvals | See [[04_External_Dependency_Management]] |
| **Sequential** | Completion criteria for predecessor activity | Definition of done, handoff criteria, grace period |

The contract adapts to what is being promised. A technical interface contract looks different from an organizational decision contract — but both answer the same question: "What exactly is being committed, by whom, by when, and what happens if it changes?"

## Contract Health Monitoring

A contract is healthy when both sides agree on its status. The TPM checks contract health at every dependency review:

| Health Signal | Green | Yellow | Red |
|---------------|-------|--------|-----|
| **Delivery progress** | On track against committed milestones | Minor deviation; recovery plan exists | Significant slip; no recovery path |
| **Spec stability** | No changes since ratification | Changes proposed but not yet agreed | Changes made without agreement |
| **Owner engagement** | Both owners respond within one business day | One owner slow to respond | One owner unresponsive or disengaged |
| **Escalation readiness** | Both sides know the escalation path | Escalation path exists but untested | No escalation path defined |

Yellow signals trigger a TPM-facilitated conversation between the owners. Red signals trigger immediate escalation — see [[06_Unblocking_Escalations]].

## Practical Applications

- [ ] Every critical and high-criticality dependency has a documented interface contract
- [ ] Every contract includes: parties, deliverable, specification, date, acceptance criteria, change process, escalation path, sign-offs
- [ ] Contracts are negotiated by the teams, facilitated by the TPM — never imposed
- [ ] Contract changes follow a defined process and are documented with date and rationale
- [ ] Contract health is reviewed at every dependency status cycle

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Contracting everything** | Low-criticality dependencies buried in paperwork; teams resent the overhead | Contract proportionally to criticality |
| **TPM-imposed terms** | Neither side owns the commitment; contract abandoned under pressure | Teams negotiate; TPM facilitates and documents |
| **Spec without acceptance criteria** | "We'll deliver the API" — but what does done mean? | Every contract has testable acceptance criteria |
| **No change process** | Contract amended silently; sides diverge | Defined change process from day one |
| **Contract as shelfware** | Signed and never referenced again | Contract is the status-check baseline every cycle |

## Success Indicators

- Cross-team integration failures caused by mismatched assumptions are rare
- Contract changes are negotiated and documented — never discovered at integration time
- Both sides of every critical contract can state the current status without looking it up
- The change process is used — not bypassed — when requirements evolve

## Related Topics

- [[01_Dependency_Mapping]]: the map records which dependencies need contracts
- [[02_Dependency_Classification]]: criticality drives whether a contract is required
- [[04_External_Dependency_Management]]: contract adaptations for external dependencies
- [[06_Unblocking_Escalations]]: what happens when a contract cannot be met
- [[career-path/05_Tech_Lead/04_Team_Delivery_and_Execution_Leadership/04_Delivery_Risk_Management|Delivery Risk Management (Tech Lead)]]: the tech lead's ownership of the spec side of the contract

## Summary

Interface contracts are the TPM's mechanism for converting dependencies-by-assumption into dependencies-by-agreement. A contract makes explicit what is being delivered, by whom, by when, to what specification, and how changes are handled. The TPM facilitates negotiation, documents the agreement, and monitors contract health through every status cycle. The contract is not a straitjacket — it is a shared reality that both sides can rely on, and the change process ensures that when reality shifts, the contract shifts with it.