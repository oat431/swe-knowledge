---
title: "Dependency Mapping"
role: Technical Program Manager
capability_area: Dependency Management
topic: Dependency Mapping
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - tpm
  - dependency-management
  - dependency-mapping
---

# Dependency Mapping

> **Core skill:** Creating and maintaining a living program dependency map — who depends on whom, for what, by when, with what contract — that makes invisible coupling visible and catches drift before it blocks work.

## Why This Matters

Every multi-team program has a web of dependencies. One team needs an API from another before it can build the integration layer. The platform migration depends on the infrastructure team finishing the new cluster. The launch date depends on the vendor delivering the hardware. Each dependency is a thread in the web — and a failure in any thread can unravel the schedule.

The dependency map is the TPM's primary coordination artifact. It answers the questions that status reports miss: "If Team A slips by two weeks, which three other teams are blocked?" and "Which dependency has no named owner on the receiving side?" Without a map, dependencies are discovered when they break. With one, they are managed before they break.

The map is not a one-time planning artifact. It is a living document that evolves as the program moves through its phases — new dependencies emerge, resolved ones retire, and the critical path shifts. The map that was accurate at charter-signing is dangerously stale by the second sprint.

## Anatomy of a Dependency Map

| Element | What It Captures | Example |
|---------|------------------|---------|
| **Source** | The team or system that depends on the deliverable | "Mobile app team needs auth service" |
| **Target** | The team or system that must deliver | "Platform team owns the auth service" |
| **Deliverable** | The specific thing being depended on | "OAuth 2.0 token endpoint with refresh support" |
| **Contract** | The agreed interface, format, or specification | "OpenID Connect spec v1.0; JSON response" |
| **Need-by date** | When the source team must have it to stay on schedule | "2026-11-15 for integration start" |
| **Commit date** | When the target team has committed to deliver | "2026-11-01 (two-week buffer)" |
| **Owner (source)** | The person watching from the dependent side | "Alice, Mobile TL" |
| **Owner (target)** | The person responsible for delivery | "Bob, Platform EM" |
| **Status** | Current state: proposed, committed, at-risk, delivered, resolved | "Committed; on track per 10/01 checkpoint" |

Every dependency without both an owner and a dated commitment is a dependency by assumption — and assumptions are the dependencies that break silently.

## Building the Map

### Step 1: Structural Discovery

Start with the program structure: workstreams, teams, and milestones from the [[01_Program_Structure_and_Charter/00_overview|Program Charter]]. Every workstream boundary is a dependency surface. Map the structural dependencies first — these are the ones that exist regardless of implementation choices.

| Source | Method | Output |
|--------|--------|--------|
| Workstream decomposition | For each pair of workstreams: does A produce something B consumes? | Producer-consumer pairs |
| Architecture diagrams | Walk the system diagram; every arrow is a dependency | System-level dependency list |
| Milestone plan | For each milestone: what must be complete before this one can start? | Predecessor-successor chains |
| Team interviews | Ask each team lead: "What do you need from outside your team that you don't control?" | Hidden dependencies the artifacts missed |

### Step 2: Team-by-Team Validation

The structural map is a hypothesis. Validate it with every team: "Here is what we believe you depend on and who depends on you. What is missing, what is wrong, and what is assumed?" Teams know their dependencies better than any artifact — but they rarely write them down. The TPM's walkthrough extracts that tacit knowledge.

### Step 3: Contracting

For every dependency that matters — see [[02_Dependency_Classification]] for criticality thresholds — move from "Team A thinks Team B will deliver X" to a documented agreement: the interface contract from [[03_Interface_Contracts]]. The map records the dependency; the contract makes it enforceable.

### Step 4: Living Maintenance

The map dies the moment planning ends unless it has a review cadence. Integrate the map into the program status cycle: every status review includes a dependency health check. Dependencies that changed status, added owners, or crossed their need-by date are surfaced. The map is a living artifact or it is shelfware — there is no third option.

## Map Visualization Patterns

| Pattern | When to Use | What It Reveals |
|---------|-------------|-----------------|
| **Matrix grid** | Teams × teams; mark intersections | Who depends on whom at a glance |
| **Directed graph** | Flow of deliverables through the program | Critical path and bottleneck nodes |
| **Timeline swimlane** | Dependencies over time per team | When the dependency pressure peaks |
| **Heat map** | Criticality × probability of slip | Where to focus contingency effort |

Choose the visualization that answers the question the audience is asking. The steering committee wants the heat map. The teams want the matrix. The program lead wants the critical path.

## Critical Path Analysis

Not all dependencies are on the critical path. The critical path is the longest chain of dependent work through the program — any slip along this chain delays the end date.

| Analysis Step | What to Do |
|---------------|------------|
| Identify chains | Trace every dependency from program start to end |
| Calculate float | For each chain: how much can it slip before the end date moves? |
| Mark the critical path | Chains with zero float are critical |
| Protect the critical path | These dependencies get the tightest tracking and strongest contingency |

The critical path shifts as the program executes. A dependency that had two weeks of float in month one may be on the critical path by month three if its predecessors slipped. The TPM recalculates the critical path at every major status cycle.

## The Dependency Map as a Communication Tool

The map serves different audiences:

| Audience | What They Need from the Map |
|----------|---------------------------|
| **Program sponsor** | Top five dependency risks; what they can unblock |
| **Team leads** | Their incoming and outgoing dependencies; status of each |
| **Engineering teams** | What they owe others and by when; what they are owed |
| **External partners** | Their commitments to the program; the program's commitments to them |

The map is one artifact with many views. The TPM curates the view for the audience — never dump the raw map into a stakeholder presentation.

## Practical Applications

- [ ] Dependency map exists and is visible to all participating teams
- [ ] Every dependency has a named source owner and target owner
- [ ] Every dependency has a dated need-by date and (where committed) a commit date
- [ ] The map is reviewed in every program status cycle
- [ ] Critical path is calculated and updated as the program executes
- [ ] Map views are curated for each audience: sponsor, teams, partners

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Map created once, never updated** | The map diverges from reality; dependencies are discovered when they break | Living map; review every status cycle |
| **Dependencies without owners** | "Team A needs Team B" — but no one on either side is watching | Every dependency has two named owners |
| **Need-by dates without commit dates** | The source team knows when they need it; the target team never committed | Contract both dates; track the gap |
| **Missing the hidden dependencies** | Teams rely on things they forgot to name: environments, approvals, access | Walkthrough interviews catch what diagrams miss |
| **Critical path assumed static** | Early-float dependencies drift onto the critical path unnoticed | Recalculate critical path at every major review |

## Success Indicators

- No team discovers a blocking dependency more than one status cycle after it emerges
- Every dependency on the critical path has a contract and contingency plan
- The map is the first artifact teams check when their plan changes
- Stakeholders can answer "what depends on what" from the map, not from memory

## Related Topics

- [[02_Dependency_Classification]]: classifying dependencies by criticality and risk
- [[03_Interface_Contracts]]: formalizing the deliverable and timing agreement
- [[04_External_Dependency_Management]]: dependencies that cross organizational boundaries
- [[07_Dependency_Health_and_Reporting]]: making the map's status visible to stakeholders
- [[career-path/11_Engineering_Manager/04_Delivery_Leadership_for_Managers/05_Delivery_Risk_Ownership|Delivery Risk Ownership (EM)]]: the manager-level view of dependency risk

## Summary

The dependency map is the TPM's coordination backbone: a living, owned, contracted web of who depends on whom, for what, by when. It is built from structural discovery and team validation, maintained through the program status cycle, and visualized differently for each audience. The map that is accurate today and stale tomorrow is not a map — it is a dangerous assumption. The TPM's discipline is keeping it alive.