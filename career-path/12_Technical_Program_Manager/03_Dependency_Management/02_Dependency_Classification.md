---
title: "Dependency Classification"
role: Technical Program Manager
capability_area: Dependency Management
topic: Dependency Classification
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - tpm
  - dependency-management
  - dependency-classification
---

# Dependency Classification

> **Core skill:** Categorizing every dependency by type, criticality, and risk profile so that tracking effort, contingency investment, and escalation attention are applied where they matter most — not spread evenly across a flat list.

## Why This Matters

A program with forty dependencies cannot treat them all equally. Some are on the critical path with no alternatives; others have built-in slack and fallback options. Some are between two internal teams with a shared manager; others cross company boundaries into a vendor's opaque delivery pipeline. Some are technical interfaces with clear specifications; others are organizational — a decision, an approval, a budget release.

Classification is the TPM's triage system. It tells you which dependencies need a formal interface contract, which need contingency plans, which need weekly owner check-ins, and which can be watched with a monthly heartbeat. Without classification, every dependency gets the same lightweight tracking — and the ones that needed heavy attention break first.

Classification also drives communication: when a stakeholder asks "what keeps you up at night?", the answer is not "forty dependencies" — it is the three classified as critical-path, external, with no fallback.

## The Classification Dimensions

Every dependency is scored on three independent dimensions. The combination determines the tracking regime.

### Dimension 1: Type

| Type | Definition | Example | Tracking Implication |
|------|------------|---------|---------------------|
| **Technical** | System A needs a specific interface, service, or data from System B | "Mobile app needs auth token endpoint" | Interface contract required; integration testing gates |
| **Organizational** | Team A needs a decision, approval, or resource from Team B | "Program needs security architecture sign-off" | Escalation path defined; decision deadline explicit |
| **Resource** | Workstream A needs people, budget, or environment from Workstream B | "QA team shared between two programs; capacity needed by 11/01" | Capacity tracking; resource contention resolution |
| **External** | Program depends on a vendor, partner, regulator, or third party | "Payment processor certification required for launch" | External tracking regime; see [[04_External_Dependency_Management]] |
| **Sequential** | Activity B cannot begin until Activity A completes | "Integration testing cannot start until platform migration finishes" | Milestone gating; predecessor health monitoring |

A single dependency can span multiple types — classify by the dominant dimension. The payment processor is external first, technical second. The security sign-off is organizational first, sequential second.

### Dimension 2: Criticality

| Level | Definition | Decision Rule |
|-------|------------|---------------|
| **Critical** | On the critical path, no fallback exists, slip directly delays program end date | Formal contract, weekly tracking, contingency plan mandatory |
| **High** | Significant schedule impact if it slips; limited fallback options | Contracted, biweekly tracking, contingency plan recommended |
| **Medium** | Moderate impact; workarounds exist or float absorbs partial slip | Documented agreement, monthly tracking |
| **Low** | Nice-to-have; slip has negligible program impact | Acknowledged; tracked in aggregate |

Criticality is not a measure of how hard the dependency is — it is a measure of what happens to the program if it fails. A technically trivial dependency (getting a port opened by the network team) can be critical if it blocks launch with no workaround.

### Dimension 3: Risk Profile

| Profile | Signals | Tracking Response |
|---------|---------|-------------------|
| **Stable** | Committed with buffer, owned on both sides, similar dependencies have delivered reliably | Standard tracking |
| **Volatile** | Target team has competing priorities, commitment is soft, history of slips | Early warning indicators; accelerated escalation threshold |
| **Unknown** | New team, new technology, new vendor relationship; no track record | Conservative buffer; contingency plan from day one |
| **External-risk** | Beyond program control: vendor, regulator, partner | External tracking regime; see [[04_External_Dependency_Management]] |

Risk profile is a leading indicator — it predicts how likely the dependency is to need attention before it actually needs attention. Classify the risk profile at dependency creation and re-assess at every status cycle.

## The Classification Matrix

Combine the dimensions into a tracking regime:

| Criticality | Risk Profile | Tracking Regime | Cadence | Artifacts |
|-------------|-------------|-----------------|---------|-----------|
| Critical | Any | Maximum | Weekly owner check-in; program review visibility | Contract, contingency plan, early-warning dashboard |
| High | Volatile / Unknown | Elevated | Biweekly check-in; flagged in program review | Contract, contingency options |
| High | Stable | Standard | Biweekly check-in | Contract |
| Medium | Volatile / Unknown | Elevated | Monthly check-in; watched in aggregate | Documented agreement |
| Medium | Stable | Light | Monthly heartbeat | Documented agreement |
| Low | Any | Monitor | Aggregate review | Listed in map |

The matrix prevents the most common classification failure: treating a high-criticality, volatile-risk dependency with the same light touch as a low-criticality, stable one.

## Re-Classification Triggers

Dependencies change character as the program executes. A stable dependency becomes volatile when the target team loses a key engineer. A medium-criticality dependency becomes critical when predecessor slips eat its float. The TPM re-classifies when:

| Trigger | Action |
|---------|--------|
| Target team changes priorities or staffing | Re-assess risk profile |
| Predecessor dependencies slip | Re-calculate criticality (float may be consumed) |
| Contract renegotiation fails or stalls | Escalate risk profile to volatile |
| New information about target team track record | Adjust risk profile with evidence |
| Program scope or schedule changes | Re-classify all dependencies downstream of the change |

Re-classification is a standing agenda item in the dependency health review (see [[07_Dependency_Health_and_Reporting]]).

## Classification at Scale

With forty or more dependencies, classification must be fast and consistent. Use a lightweight rubric:

```
For each dependency:
  Type: [Technical | Organizational | Resource | External | Sequential]
  Criticality: [Critical | High | Medium | Low]
    → Critical if: on critical path AND no fallback
    → High if: significant schedule impact OR limited fallback
    → Medium if: workaround exists
    → Low if: negligible program impact
  Risk profile: [Stable | Volatile | Unknown | External-risk]
    → Stable if: committed + owned + track record
    → Volatile if: soft commitment OR competing priorities OR history
    → Unknown if: no track record
    → External-risk if: beyond program control
```

The rubric takes thirty seconds per dependency. The TPM's judgment fills the gaps the rubric cannot capture — but the rubric ensures consistency across the map.

## Practical Applications

- [ ] Every dependency in the map carries a type, criticality, and risk profile classification
- [ ] Classification drives the tracking regime: cadence, artifacts, escalation threshold
- [ ] Re-classification triggers are defined and checked at every status cycle
- [ ] The classification matrix is visible — teams understand why their dependency gets heavy or light tracking
- [ ] Critical + volatile dependencies are never more than three status cycles from sponsor visibility

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Flat list, equal attention** | Critical dependencies get the same light tracking as low ones | Classification matrix drives differentiated tracking |
| **Criticality by perceived difficulty** | A hard technical problem classified critical when a trivial blocker is the real threat | Criticality = impact on program end date if it fails |
| **Static classification** | Risk profile changes; classification stays frozen from planning | Re-classify at every status cycle |
| **Over-classifying** | Everything is "critical" so nothing is | Be stingy with critical; it loses meaning if overused |
| **Classification without action** | Dependencies are classified but tracking doesn't change | The matrix must change behavior, not just labels |

## Success Indicators

- Critical dependencies never go more than one week without an owner check-in
- Re-classification happens before — not after — a dependency surprises the program
- The sponsor can name the top three critical-dependency risks without looking at the map
- No low-classified dependency ever blocks the critical path

## Related Topics

- [[01_Dependency_Mapping]]: the map that classification organizes
- [[03_Interface_Contracts]]: contracts required for critical and high dependencies
- [[05_Dependency_Risk_and_Contingency]]: contingency proportionate to classification
- [[07_Dependency_Health_and_Reporting]]: health reporting stratified by classification

## Summary

Dependency classification is the TPM's triage: type, criticality, and risk profile determine how tightly each dependency is tracked, how much contingency it warrants, and how visible it is to leadership. Classification is not a one-time labeling exercise — it is a living assessment that changes as the program changes. The disciplines that matter are applying the rubric consistently, re-classifying at the right triggers, and ensuring that classification changes behavior, not just labels.