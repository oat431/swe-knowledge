---
title: "RAID Log Management"
role: Technical Program Manager
capability_area: Risk and Issue Leadership
topic: RAID Log Management
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - tpm
  - risk-management
  - raid-log
---

# RAID Log Management

> **Core skill:** Running the Risks, Assumptions, Issues, and Decisions log as the program's central nervous system — a single, shared, living artifact that makes the program's uncertainty, constraints, problems, and choices visible to everyone who needs to act on them.

## Why This Matters

Every program has risks, assumptions, issues, and decisions. The difference between programs that manage them and programs that are managed by them is whether these four things live in one place, visible to all, reviewed regularly — or scattered across spreadsheets, slide decks, meeting notes, and people's heads.

The RAID log is the TPM's primary artifact. It is to program management what the codebase is to engineering: the single source of truth that everyone can inspect, contribute to, and rely on. When a stakeholder asks "what are we worried about?", the RAID log answers. When a new team member asks "why did we choose this architecture?", the Decisions section answers. When an assumption is about to expire, the log flags it.

The RAID log is not a compliance document written for auditors. It is an operational tool written for the program team — reviewed in every status cycle, updated continuously, and treated as the agenda for program governance. A RAID log that is only opened before steering committee meetings is a RAID log that has already failed.

## The Four Sections

### Risks

| Field | Content |
|-------|---------|
| ID | RISK-XXX |
| Statement | One sentence: uncertain event + effect on program |
| Probability | High / Medium / Low with rationale |
| Impact | High / Medium / Low with quantified effect |
| Proximity | Imminent / Near-term / Distant |
| Owner | One named person |
| Mitigation | Action underway with date |
| Contingency | What happens if it fires |
| Status | Active / Retired / Fired (converted to issue) |
| Review date | Next assessment date |

See [[02_Risk_Identification_and_Assessment]] and [[03_Risk_Response_Planning]] for the full risk discipline.

### Assumptions

Assumptions are beliefs the program is treating as facts — without verification. Every assumption is a risk wearing a different label: if the assumption is wrong, the program is damaged.

| Field | Content |
|-------|---------|
| ID | ASMP-XXX |
| Statement | What the program is assuming to be true |
| Rationale | Why this assumption is being made (not verified) |
| Impact if wrong | What breaks if the assumption is false |
| Validation | How and when the assumption will be verified |
| Owner | One named person responsible for validation |
| Expiry date | When the assumption must be validated or converted to a risk |

| Assumption Example | Validation |
|--------------------|-----------|
| "The new deployment pipeline will be production-ready by 11/01" | Pipeline passes production-readiness test suite by 10/15 |
| "The vendor's API will support our peak load of 10K req/s" | Load test completed by 10/01 |
| "The security review will take no more than two weeks" | Security team confirms timeline by 09/15 |

The TPM's discipline: every assumption is either validated (and removed from the log) or proven false (and converted to a risk or issue) — before its expiry date. An assumption that lives past its expiry date is a program liability.

### Issues

See [[04_Issue_Tracking_and_Resolution]] for the full issue discipline. In the RAID context, the Issues section is the program's active problem list:

| Field | Content |
|-------|---------|
| ID | ISS-XXX |
| Statement | What is happening now |
| Impact | What is affected |
| Urgency | Immediate / High / Medium / Low |
| Owner | One named person |
| Resolution plan | How it will be resolved |
| Target date | When resolution is expected |
| Status | Open / In Progress / Resolved / Closed |

### Decisions

Decisions are the program's institutional memory. Every significant choice — architecture, vendor selection, scope trade-off, date commitment — is recorded with its context and rationale.

| Field | Content |
|-------|---------|
| ID | DEC-XXX |
| Statement | What was decided |
| Context | What situation prompted the decision |
| Options considered | What alternatives were evaluated |
| Rationale | Why this option was chosen |
| Decided by | Who made the decision (name and role) |
| Date | When the decision was made |
| Review date | When the decision will be revisited (if applicable) |

Decisions prevent the program from re-litigating settled questions. When someone asks "why did we choose this vendor?", the answer is DEC-017 — not a thirty-minute rehash of a debate that concluded two months ago.

## RAID Log Structure and Tooling

| Program Scale | Tool | Structure |
|---------------|------|-----------|
| **Small (1–2 workstreams)** | Shared spreadsheet | One sheet per section; simple filtering |
| **Medium (3–5 workstreams)** | Shared spreadsheet or lightweight database | One sheet per section; workstream tags; dashboard tab |
| **Large (6+ workstreams)** | Program management tool (Jira, Asana, Monday) or dedicated database | Tool-native RAID tracking; automated dashboards; alerting |

The tool is less important than the discipline. A well-maintained spreadsheet with owner columns and conditional formatting outperforms an enterprise tool that nobody updates. The TPM starts with the simplest tool that works and adds complexity only when the program's scale demands it.

## The RAID Review Cycle

| Cadence | What Happens | Who Attends |
|---------|-------------|-------------|
| **Weekly** | TPM reviews: new entries, status changes, aging items, upcoming expiry dates | TPM (solo review) |
| **Biweekly (program review)** | Top risks, active issues, expiring assumptions, decisions pending | Program team (workstream leads, TPM) |
| **Monthly (sponsor review)** | Top 5 risks; issues affecting program commitments; decisions needed | Sponsor, TPM, workstream leads |
| **Quarterly (steering committee)** | RAID log health summary; trends; systemic risks and issues | Steering committee, sponsor, TPM |

The weekly solo review is the TPM's hygiene pass: entries are updated, owners are chased for stale items, expiry dates are checked. The biweekly program review is where the team engages: risks are discussed, issues are triaged, assumptions are challenged. The monthly sponsor review is where decisions are made and escalations are resolved.

## RAID Log Hygiene

A RAID log degrades without active maintenance. The TPM's hygiene practices:

| Practice | What It Prevents |
|----------|-----------------|
| **Every entry has an owner** | Ownerless entries that nobody watches |
| **Every entry has a date** | Timeless entries that drift indefinitely |
| **No entry ages beyond its review date without update** | Stale entries that no longer reflect reality |
| **Closed entries carry a one-line closure note** | Institutional memory loss |
| **Duplicate or superseded entries are merged or retired** | Log bloat; signal-to-noise degradation |
| **Entries are written in plain language** | Jargon entries that only the author understands |

The TPM spends thirty minutes a week on RAID hygiene. It is the highest-leverage thirty minutes in the program management calendar — because a clean RAID log is a program that knows its own state.

## The RAID Log as a Communication Tool

| Audience | What They Get from the RAID Log |
|----------|-------------------------------|
| **Program team** | Full log; filtered by workstream; updated in real time |
| **Sponsor** | Top risks; active issues; decisions needed; assumption expiry watch |
| **Steering committee** | RAID health summary: counts by status, aging trends, systemic patterns |
| **New team members** | Decisions section — the program's rationale archive |
| **Auditors / post-program review** | Complete record of what was anticipated, what happened, and what was decided |

The RAID log is one artifact with many audiences. The TPM curates the view — nobody reads the full log except the TPM and the program team.

## Practical Applications

- [ ] RAID log exists as a single, shared artifact with all four sections
- [ ] Every entry has an owner and a date (created, review, or target)
- [ ] Weekly TPM hygiene review keeps the log clean and current
- [ ] Biweekly program review uses the RAID log as the agenda
- [ ] Assumptions have expiry dates and are validated or converted before expiry
- [ ] Decisions are recorded with context, options, and rationale

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **RAID as shelfware** | Created at program start; never opened again | Living artifact; reviewed at every status cycle |
| **Four separate documents** | Risks in one spreadsheet, issues in another, decisions in meeting notes | Single artifact; cross-references between sections |
| **No assumption validation** | Assumptions sit unvalidated; become silent risks | Every assumption has an expiry date and a validation plan |
| **Decisions without rationale** | "We chose Vendor X" — but nobody remembers why | Every decision records context, options, and rationale |
| **Log bloat** | Hundreds of entries; no differentiation between critical and trivial | Classify by criticality; archive low-criticality resolved entries |
| **Hidden log** | RAID log is a private TPM document | Visible to the full program team; transparency is leverage |

## Success Indicators

- The RAID log is the first artifact opened in every program status review
- Assumptions are validated or converted before their expiry dates
- Decisions are referenced, not re-litigated
- A new team member can understand the program's risk posture in thirty minutes from the RAID log alone
- The log's closed entries tell a coherent story of risks retired, issues resolved, and decisions made

## Related Topics

- [[02_Risk_Identification_and_Assessment]]: the process that populates the Risks section
- [[03_Risk_Response_Planning]]: mitigation and contingency in the Risks section
- [[04_Issue_Tracking_and_Resolution]]: the process that populates the Issues section
- [[06_Risk_Escalation_and_Communication]]: escalation driven by RAID log signals

## Summary

The RAID log is the TPM's central artifact — a single, shared, living document that holds the program's risks, assumptions, issues, and decisions. It is reviewed at every status cycle, maintained through weekly hygiene, and curated for different audiences. The RAID log's quality is the TPM's quality: a clean, current, honest RAID log is the sign of a program that knows itself. A stale, bloated, hidden RAID log is the sign of a program that will be surprised by what it should have seen coming.