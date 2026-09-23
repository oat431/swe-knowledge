---
document_type: Project Tier Checklist — Tier 2 Prototype / MVP
schema_version: 2
canonical_name: Tier 2 Prototype / MVP Checklist
doc_form: record
applicability: evidence
min_project_tier: 2
minimum_form: This checklist itself
version: "1.0"
status: Active
created: "2026-09-24"
last_updated: "2026-09-24"
tags: [tier-checklist, tier-2, project-size, generated]
generator: Generated from template frontmatter — do not hand-edit; regenerate instead
---

# 🔧 Tier 2 — Prototype / MVP — Document Checklist

> **Description:** Waiting for integration or user validation. Might become real.
> **Team:** 1–2 devs | **Users:** Beta testers | **Lifespan:** Weeks–months
> **Tier source:** `checklist/release-checklist/release.md` §Project Tier Scoping Matrix
>
> **Key principle:** Everything a Tier-2 project needs = every template with `min_project_tier ≤ 2` whose trigger applies. Anything above tier 2 is over-engineering for this context.
>
> **Scope:** 6 artifacts (5 new at this tier, 1 inherited from lower tiers).

## How to use

1. Confirm your tier with the decision flow in `release.md` §"Which Tier Am I?".
2. Tick ☐ → ✅ as each document is created. **Skip rows whose trigger condition doesn't apply** and note the omission in [[Tailoring-Justification]].
3. `Form` column: **heavy** = full controlled document · **light** = lean doc · **record** = produced by an activity (create when the activity happens, not upfront).
4. Priority: 🔴 universal · 🟡 conditional/evidence (check trigger) · 🟢 technique (embed in parent docs, never standalone).

## Tier ladder

| Tier | Checklist | Artifacts |
|---|---|---|
| 1 | [[Tier-1-POC-Spike-Checklist|🧪 POC / Spike]] | 1 |
| 2 | **→ this tier** | 6 |
| 3 | [[Tier-3-Internal-Tool-Checklist|🏠 Internal Tool]] | 21 |
| 4 | [[Tier-4-Small-Production-Checklist|🟢 Small Production]] | 228 |
| 5 | [[Tier-5-Medium-Production-Checklist|🔵 Medium Production]] | 321 |
| 6 | [[Tier-6-Production-Grade-Checklist|🟣 Production Grade]] | 342 |
| 7 | [[Tier-7-Mission-Critical-Checklist|🔴 Mission-Critical / Regulated]] | 360 |

## Requirements Engineering

> **Owner:** PO / BA · 2 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Assumption-Log]] | 🟡 | record | T2 🆕 | Produced by the activity it records | ☐ |
| [[Definition-of-done]] | 🔴 | light | T2 🆕 | always | ☐ |

## Project Management Planning

> **Owner:** PM · 1 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Risk-Register]] | 🟡 | record | T2 🆕 | Produced by the activity it records · view of `Risk-Management-Plan` | ☐ |

## Systems Architecture and Design

> **Owner:** Architect · 1 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Architecture-Decision-Records]] | 🔴 | light | T2 🆕 | always | ☐ |

## SE Cross Cutting

> **Owner:** Systems Engineer · 2 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Decision-Records]] | 🔴 | light | T2 🆕 | always | ☐ |
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
