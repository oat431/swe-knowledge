---
title: Scoping and Success Criteria
role: Forward Deployed Engineer
capability_area: Field Discovery and Problem Framing
topic: Scoping and Success Criteria
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - forward-deployed
  - scoping
  - success-criteria
  - baselines
---

# Scoping and Success Criteria

> **Core skill:** Defining what done means — measurable and agreed — before build commitments are made.

## Why This Matters

A deployment without agreed success criteria cannot succeed; it can only end ambiguously, with the customer remembering a promise that was never made and the vendor defending work that was never requested. Scoping is the act that converts a framed problem into a commitment: what will be built, what will not, how the change will be measured, and who has agreed to all three. It is the FDE's contract with reality, and it is written before the build, not after the pilot.

The field makes scoping harder than the office does. Customers discover scope as they see progress — a working demo invites "can it also..." from every direction, and each request is individually reasonable. Without an explicit out-of-scope list and a change path, every enthusiastic yes becomes delivery debt, and the deployment drifts away from the frame that justified it. The discipline is not rigidity; it is making scope changes visible, priced, and chosen rather than absorbed silently.

Success criteria carry equal weight, and they need baselines. "Cut processing time" is not a criterion until someone has measured today's processing time; "improve accuracy" is not a criterion until someone owns the accuracy number. The FDE who insists on baselines before build commitments looks pedantic for one meeting and vindicated for the rest of the deployment, because the moment the outcome is questioned, only measured evidence settles the question.

## The Anatomy of Scoping

| Element | Weak version | Strong version | Who agrees |
|---------|--------------|----------------|------------|
| Outcome | "Improve the process" | "Cut case handling from 40 to 25 minutes within two quarters" | Sponsor and operating lead |
| In scope | "The claims workflow" | "Intake through decision-ready summary for complex cases" | Operating lead |
| Out of scope | Unstated | "No changes to intake systems, pricing, or approval policy" | Sponsor |
| Measures | Vague adjectives | Named metric, source, and measurement method | Whoever owns the data |
| Baseline | Assumed or remembered | Measured and recorded before build starts | Customer analysts |
| Time box | A date someone announced | Milestones with decision gates, not a single deadline | Both sides |
| Dependencies | Ignored | Named owners for access, data, review, and deployment windows | Named individuals |

## Writing Success Criteria That Survive

| Criterion type | Example shape | Trap to avoid |
|----------------|---------------|---------------|
| Adoption | "Seventy percent of eligible users active weekly by month two" | Counting logins instead of completed work |
| Quality | "Error rate at or below the pre-change baseline" | Measuring only speed and letting quality erode |
| Time | "Median handling time down by a third" | Mean averages hiding the tail that hurts most |
| Risk | "Zero manual re-keys on the critical path" | Metric games where the work migrates elsewhere |
| Value | "Rework hours recovered per month, per the finance model" | Vendor-authored models the customer did not validate |

Each criterion should name its data source, its owner, and its measurement window. A criterion nobody can compute is a wish.

## Agreeing and Re-Agreeing

| Phase | What is fixed | What stays open | Re-agreement trigger |
|-------|---------------|-----------------|----------------------|
| Frame and scope | Problem, outcome, out-of-scope list | Implementation approach | Evidence contradicts the frame |
| Prototype | Demonstration goals | Solution shape and technology | Demo reveals a different bottleneck |
| Pilot | Success measures and baselines | Rollout shape and scale | Measures prove unmeasurable as defined |
| Production | Acceptance criteria and ownership | Operability details | Material change in data, systems, or policy |

```mermaid
flowchart LR
    FRAME["Agreed problem frame"] --> DRAFT["Draft scope and measures"]
    DRAFT --> BASELINE["Baseline each measure"]
    BASELINE --> SIGNOFF["Written agreement"]
    SIGNOFF --> GATES["Revisit at every phase gate"]
```

The right to re-open scope is itself part of the agreement. A re-scope that is anticipated, written, and jointly decided is a healthy deployment event; one that happens silently is how trust dies.

## Practical Applications

### Scope and Success Memo

```markdown
## Scope and Success Criteria — <customer, initiative>

| Element | Statement |
|---------|-----------|
| Outcome | <measurable change, horizon> |
| In scope | <workflows, users, systems, geography> |
| Out of scope | <explicit exclusions> |
| Success measures | <metric, source, method, window> |
| Baselines | <measured current values, date> |
| Dependencies | <named owners for access, data, review, windows> |
| Change path | <how scope changes are proposed, priced, decided> |
| Agreed by | <names, roles, date> |
```

### Scoping Checklist

- [ ] Outcome statement uses numbers a skeptical analyst could verify
- [ ] The out-of-scope list exists and the sponsor has acknowledged it
- [ ] Every success measure has a baseline measured before build starts
- [ ] Each measure has a named owner on the customer side
- [ ] The change path for new requests is written before the first request arrives
- [ ] Milestones are decision gates, not one ceremonial go-live date

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Success criteria written late** | Criteria reverse-engineered from what was built measure nothing | Agree measures while the problem frame is still being argued |
| **Vanity measures** | Logins and page views move while the customer's work does not | Measure the customer's outcomes, not the tool's activity |
| **Scope without exclusions** | Silence reads as a promise to every stakeholder | Write and acknowledge an explicit out-of-scope list |
| **Unowned baselines** | Disputed numbers stall every later outcome conversation | Have the customer's own analysts own the baseline |
| **Silent re-scoping** | Each quiet accommodation erodes the agreement invisibly | Route every change through the written change path |
| **One deadline instead of gates** | A single far date hides slippage until it is unrecoverable | Use milestone gates with decisions attached |

## Success Indicators

- The customer can state the out-of-scope list without looking it up
- Outcome claims at the end trace to measures and baselines agreed at the start
- Scope change requests arrive through the change path, priced and prioritized
- The mid-deployment re-scope, if any, was a planned conversation with both signatures
- Finance and operations treat the success numbers as credible because they helped define them

## Related Topics

- [[04_Problem_Framing_Under_Ambiguity]]
- [[07_Working_with_Sales_and_Account_Teams]]
- [[02_Solution_Design_and_Rapid_Prototyping/00_overview|Solution Design and Rapid Prototyping]]
- [[career-path/12_Technical_Program_Manager/00_overview|Technical Program Manager]]
- [[software-engineering-note/01_Software_Requirements/Software Requirements Overview]]

## Summary

Scoping and success criteria are how a framed problem becomes a commitment: a written statement of the outcome, the included and excluded work, the measures, and the baselines that make the measures real — all agreed before build commitments are made and revisited at every phase gate through an explicit change path. The FDE who scopes precisely builds less and proves more, and the deployment that survives its own success is the one that was measured from the start.
