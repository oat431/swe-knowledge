---
document_type: Project Tier Checklist — Tier 3 Internal Tool
schema_version: 2
canonical_name: Tier 3 Internal Tool Checklist
doc_form: record
applicability: evidence
min_project_tier: 3
minimum_form: This checklist itself
version: "1.0"
status: Active
created: "2026-09-24"
last_updated: "2026-09-24"
tags: [tier-checklist, tier-3, project-size, generated]
generator: Generated from template frontmatter — do not hand-edit; regenerate instead
---

# 🏠 Tier 3 — Internal Tool — Document Checklist

> **Description:** Real users (employees), real traffic. No external exposure or paying customers.
> **Team:** 1–3 devs | **Users:** Employees | **Lifespan:** Ongoing
> **Tier source:** `checklist/release-checklist/release.md` §Project Tier Scoping Matrix
>
> **Key principle:** Everything a Tier-3 project needs = every template with `min_project_tier ≤ 3` whose trigger applies. Anything above tier 3 is over-engineering for this context.
>
> **Scope:** 21 artifacts (15 new at this tier, 6 inherited from lower tiers).

## How to use

1. Confirm your tier with the decision flow in `release.md` §"Which Tier Am I?".
2. Tick ☐ → ✅ as each document is created. **Skip rows whose trigger condition doesn't apply** and note the omission in [[Tailoring-Justification]].
3. `Form` column: **heavy** = full controlled document · **light** = lean doc · **record** = produced by an activity (create when the activity happens, not upfront).
4. Priority: 🔴 universal · 🟡 conditional/evidence (check trigger) · 🟢 technique (embed in parent docs, never standalone).

## Tier ladder

| Tier | Checklist | Artifacts |
|---|---|---|
| 1 | [[Tier-1-POC-Spike-Checklist|🧪 POC / Spike]] | 1 |
| 2 | [[Tier-2-Prototype-MVP-Checklist|🔧 Prototype / MVP]] | 6 |
| 3 | **→ this tier** | 21 |
| 4 | [[Tier-4-Small-Production-Checklist|🟢 Small Production]] | 228 |
| 5 | [[Tier-5-Medium-Production-Checklist|🔵 Medium Production]] | 321 |
| 6 | [[Tier-6-Production-Grade-Checklist|🟣 Production Grade]] | 342 |
| 7 | [[Tier-7-Mission-Critical-Checklist|🔴 Mission-Critical / Regulated]] | 360 |

## Business Analysis and strategy

> **Owner:** PO / BA · 1 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Business-Objectives]] | 🔴 | heavy | T3 🆕 | always | ☐ |

## Elicitation and Collaboration

> **Owner:** BA / PO · 2 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Elicitation-Results-Confirmed]] | 🟡 | record | T3 🆕 | Produced by the activity it records | ☐ |
| [[Elicitation-Results-Unconfirmed]] | 🟡 | record | T3 🆕 | Produced by the activity it records | ☐ |

## Requirements Engineering

> **Owner:** PO / BA · 5 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Acceptance-Criteria]] | 🔴 | heavy | T3 🆕 | always | ☐ |
| [[Assumption-Log]] | 🟡 | record | T2 | Produced by the activity it records | ☐ |
| [[Definition-of-done]] | 🔴 | light | T2 | always | ☐ |
| [[Nonfunctional-Requirements-Catalog]] | 🟡 | record | T3 🆕 | Produced by the activity it records | ☐ |
| [[User-Stories]] | 🔴 | heavy | T3 🆕 | always | ☐ |

## Project Management Planning

> **Owner:** PM · 2 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Project-Charter]] | 🔴 | light | T3 🆕 | always | ☐ |
| [[Risk-Register]] | 🟡 | record | T2 | Produced by the activity it records · view of `Risk-Management-Plan` | ☐ |

## Project Management Executing and MC

> **Owner:** PM · 2 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Issue-Log]] | 🟡 | record | T3 🆕 | Produced by the activity it records | ☐ |
| [[Meeting-Minutes]] | 🟡 | record | T3 🆕 | Produced by the activity it records | ☐ |

## Systems Architecture and Design

> **Owner:** Architect · 1 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Architecture-Decision-Records]] | 🔴 | light | T2 | always | ☐ |

## Construction

> **Owner:** Developer · 4 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Coding-Standards]] | 🔴 | light | T3 🆕 | always | ☐ |
| [[Commit-Messages-Changelog]] | 🟡 | record | T3 🆕 | Produced by the activity it records | ☐ |
| [[Dependency-Manifest]] | 🔴 | light | T3 🆕 | always | ☐ |
| [[README-Developer-Guide]] | 🔴 | light | T3 🆕 | always | ☐ |

## Deployment and Operations

> **Owner:** DevOps / SRE · 1 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[CI-CD-Pipeline-Configuration]] | 🔴 | light | T3 🆕 | always | ☐ |

## SE Cross Cutting

> **Owner:** Systems Engineer · 3 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Decision-Records]] | 🔴 | light | T2 | always | ☐ |
| [[Measurement-Plan]] | 🔴 | light | T3 🆕 | always | ☐ |
| [[Tailoring-Justification]] | 🔴 | light | T1 | always | ☐ |

---

## Tailoring record

| # | Document omitted / combined | Reason | Approved by |
|---|---|---|---|
| 1 | [Document] | [Why not needed at this tier] | [Name] |

> Copy omissions into the project's [[Tailoring-Justification]].

## Related

- [[7-Tier Applicability Matrix]] — full matrix view over all 363 templates
- [[release]] — tier model source (Project Tier Scoping Matrix)
- [[TEMPLATE-INDEX]] — regenerated master index
