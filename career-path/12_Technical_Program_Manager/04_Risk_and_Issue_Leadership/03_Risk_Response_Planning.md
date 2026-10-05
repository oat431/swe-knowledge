---
title: "Risk Response Planning"
role: Technical Program Manager
capability_area: Risk and Issue Leadership
topic: Risk Response Planning
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - tpm
  - risk-management
  - risk-response
  - mitigation
---

# Risk Response Planning

> **Core skill:** Developing and executing risk responses — mitigation, transfer, avoidance, acceptance — with named owners and dated actions, so that risks are actively reduced, not passively watched, and the program's exposure shrinks over time.

## Why This Matters

A risk identified and assessed but not responded to is a risk that will fire on its own schedule. Naming a risk without a response is not risk management — it is a bet that the risk will not fire, placed on behalf of the program without the program's consent.

Risk response planning converts the risk register from a catalogue of worries into a list of active interventions. For each risk above the low threshold, the TPM ensures a response strategy is selected, an owner is assigned, actions are dated, and progress is tracked. The response does not have to eliminate the risk — it has to reduce the program's exposure to a level the program can accept.

The discipline is proportionality: critical risks get robust responses with dedicated effort; medium risks get lighter responses; low risks are accepted and watched. A program that tries to mitigate every risk equally will exhaust its capacity and mitigate none of them well.

## The Four Response Strategies

| Strategy | Definition | When to Use | Example |
|----------|------------|-------------|---------|
| **Mitigate** | Reduce the probability and/or impact of the risk | The risk is significant enough to warrant active effort, and mitigation is feasible | "The vendor may miss the milestone. Mitigation: weekly checkpoint meetings plus a two-week buffer in the schedule." |
| **Transfer** | Shift the impact to a third party | The impact can be contractually or financially shifted; the third party can absorb it | "Data center outage risk. Transfer: contract includes SLA with penalty clauses and a disaster recovery obligation." |
| **Avoid** | Eliminate the risk by changing the plan | The risk is unacceptable and can be eliminated by a different approach | "New database technology risk. Avoid: use the proven database instead; accept the reduced feature set." |
| **Accept** | Acknowledge the risk without active mitigation | The cost of mitigation exceeds the expected impact, or mitigation is infeasible | "Minor UX changes from user testing may delay launch by a few days. Accept: the impact is small and the testing is valuable." |

Mitigation is the most common response and the one most likely to be done poorly. Transfer only works when the third party is able and incentivized to absorb the impact — a penalty clause is not a transfer if the vendor cannot pay it. Avoidance is the most powerful response but requires the authority to change the plan. Acceptance is a legitimate strategy, not a failure — provided it is a conscious choice, not a default.

## The Mitigation Plan

For every risk with a mitigation response, the TPM ensures the plan is actionable:

| Element | Content | Example |
|---------|---------|---------|
| **Risk** | Reference to the risk register entry | "RISK-014: Platform migration overruns the 12/01 completion window" |
| **Mitigation action** | What will be done to reduce probability or impact | "Run migration on a staging environment by 10/15 to surface unknown unknowns; staff a dedicated migration engineer for 4 weeks" |
| **Owner** | One named person responsible for executing the mitigation | "Carlos, Platform TL" |
| **Start date** | When mitigation begins | "2026-10-01" |
| **Completion date** | When mitigation is complete | "2026-10-15 (staging run); 2026-11-15 (dedicated engineer completes)" |
| **Residual risk** | What remains after mitigation: P, I, and rationale | "Probability reduced from High to Medium; impact unchanged at High" |
| **Fallback** | What happens if mitigation fails or the risk fires despite mitigation | "If migration overruns: defer the non-critical data pipeline migration to Q1; launch with the critical path migrated" |

A mitigation of "monitor" or "watch closely" is not a mitigation — it is passive acceptance dressed up as action. Every mitigation either reduces probability (by changing conditions that make the risk more likely) or reduces impact (by preparing a response that limits damage). Watching does neither.

## Risk Response by Criticality

| Risk Score | Response Expectation | Cadence |
|------------|---------------------|---------|
| **Critical** (High P × High I) | Active mitigation with dated actions; contingency plan in place; sponsor visibility | Weekly owner check-in; program review visibility |
| **High** (High P × Medium I or Medium P × High I) | Active mitigation with dated actions; contingency options identified | Biweekly owner check-in; flagged in program review |
| **Medium** (Medium P × Medium I) | Mitigation or acceptance with rationale documented | Monthly check-in |
| **Low** (all others) | Acceptance; watched for changes in P, I, or proximity | Aggregate review; re-assessed if conditions change |

The TPM enforces proportionality: a critical risk with a "monitor" mitigation is escalated to the risk owner's manager. The program cannot afford currency-level responses to critical risks.

## Contingency vs Mitigation

These are distinct and complementary:

| Dimension | Mitigation | Contingency |
|-----------|-----------|-------------|
| **Timing** | Before the risk fires | After the risk fires |
| **Goal** | Reduce the probability or impact | Absorb the impact if the risk fires despite mitigation |
| **Example** | "Hire a second database engineer to reduce the key-person risk" | "Cross-train a backend engineer on database operations so someone can step in if needed" |
| **Cost** | Invested now | Reserved; spent only if the risk fires |

Every critical risk should have both: mitigation to reduce the chance of it firing, and contingency to absorb the impact if it fires anyway. A risk with mitigation but no contingency is half-managed.

## Risk Response Tracking

The TPM tracks risk response progress as diligently as workstream delivery progress:

| Tracking Practice | What It Looks Like |
|-------------------|--------------------|
| **Mitigation milestones** | Dated checkpoints within the mitigation plan; tracked in the risk register |
| **Owner check-ins** | TPM checks in with the risk owner at the defined cadence; status updated in the register |
| **Mitigation effectiveness** | Is the residual risk trending down? Is the probability or impact actually reducing? |
| **Response adjustment** | If mitigation is not working, the response strategy is revisited — not silently continued |
| **Response closure** | When mitigation is complete, the risk is re-scored with the residual P and I |

The TPM does not execute mitigations — the risk owner does. The TPM tracks execution and surfaces when mitigations are off-track, just as they would for any other program deliverable.

## When to Change the Response

| Trigger | Action |
|---------|--------|
| **Mitigation is not reducing exposure** | Reassess: is the mitigation the wrong approach, or is execution failing? |
| **New information changes P or I** | Re-score the risk; the response strategy may need to change |
| **Proximity shortens** | An accepted distant risk becomes imminent — acceptance may no longer be appropriate |
| **Program context changes** | Scope, schedule, or resource changes may make avoidance feasible or mitigation impossible |
| **Multiple risks in the same category fire** | The response strategy for the category may be systematically wrong |

The response is a living plan, not a one-time decision. The TPM revisits response strategies at every major program review.

## Practical Applications

- [ ] Every critical and high risk has a documented response strategy with owner and dated actions
- [ ] Mitigations reduce probability, impact, or both — "monitor" is not a mitigation
- [ ] Critical risks have both mitigation and contingency plans
- [ ] Response progress is tracked in the risk register at the defined cadence
- [ ] Response strategies are revisited when conditions change — not silently continued

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **"Monitor" as mitigation** | Watching is not reducing exposure; the risk fires on its own schedule | Every mitigation is an action that changes probability or impact |
| **Mitigation without owner** | "The team will mitigate" — nobody is accountable | One named owner per mitigation |
| **Mitigation without dates** | "We will do X" — but when? | Every mitigation has a start date and completion date |
| **Acceptance as default** | Every risk not actively mitigated is accepted by omission | Acceptance is a conscious choice with documented rationale |
| **No contingency for critical risks** | Mitigation fails; program has no fallback | Every critical risk has mitigation and contingency |

## Success Indicators

- The risk register shows residual risk scores declining over time
- No critical risk carries a "monitor" mitigation
- Risk responses are adjusted when they are not working — not silently abandoned
- Risks accepted consciously carry a rationale that holds up in review

## Related Topics

- [[02_Risk_Identification_and_Assessment]]: the scoring that determines which risks get which response
- [[05_RAID_Log_Management]]: the RAID log where response plans are documented and tracked
- [[07_Program_Contingency_Planning]]: program-level contingency beyond individual risk responses
- [[../03_Dependency_Management/05_Dependency_Risk_and_Contingency|Dependency Risk and Contingency]]: dependency-specific response planning

## Summary

Risk response planning is the TPM's action discipline: for every risk above the low threshold, select a strategy (mitigate, transfer, avoid, accept), assign a named owner, define dated actions, and track progress until the residual risk is at an acceptable level. Mitigation reduces exposure before the risk fires; contingency absorbs impact if it fires anyway. The TPM's test: when a risk fires, was there an active response underway, or was the program watching it happen?