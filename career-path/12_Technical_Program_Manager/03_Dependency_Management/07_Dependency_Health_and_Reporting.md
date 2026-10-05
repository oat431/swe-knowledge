---
title: "Dependency Health and Reporting"
role: Technical Program Manager
capability_area: Dependency Management
topic: Dependency Health and Reporting
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - tpm
  - dependency-management
  - dependency-health
  - reporting
---

# Dependency Health and Reporting

> **Core skill:** Making dependency status visible and actionable — to teams, sponsors, and stakeholders — through a structured health dashboard that surfaces what is at risk, what has changed, and what needs attention, without drowning the audience in the full dependency map.

## Why This Matters

A dependency map that nobody reads is not a management tool — it is a documentation exercise. The map's value is realized only when its status is visible to the people who can act on it: teams that need to adjust their plans, sponsors who can unblock stalled dependencies, and stakeholders who need to understand why the program's date is moving.

Dependency health reporting answers the questions each audience is asking. The team lead wants to know: "Which of my incoming dependencies are at risk?" The sponsor wants to know: "What are the top three dependency threats to the program date?" The external partner wants to know: "Are we meeting our commitments?" A single report cannot serve all three — so the TPM produces multiple views from the same underlying data.

Health reporting also creates accountability. When a dependency shows red in the dashboard for three consecutive weeks, the pattern is visible to everyone. It is harder to ignore a red dependency on a shared dashboard than a red dependency buried in a spreadsheet only the TPM reads. Visibility is leverage.

## The Dependency Health Dashboard

The dashboard is a single-page view of the entire dependency portfolio, stratified by health status:

| Health Status | Definition | Color | Action |
|---------------|------------|-------|--------|
| **On Track** | Delivering against the contracted date and spec; owner engaged | Green | Standard tracking |
| **Watch** | Minor deviation detected; recovery plan exists; no impact yet | Yellow | Increased monitoring; TPM checks in with owner |
| **At Risk** | Significant deviation; no recovery plan; impact likely | Orange | Contingency activation considered; escalation prepared |
| **Blocked** | Delivery stopped; dependency cannot proceed without intervention | Red | Immediate escalation; contingency activated |
| **Resolved** | Delivered and accepted; dependency closed | Grey | Archived with resolution note |

The dashboard shows every dependency's status, criticality, owner, need-by date, and the last status change date. Dependencies are sorted by criticality and health — red critical dependencies at the top, green low-criticality at the bottom.

## Audience-Specific Views

| Audience | What They See | Cadence | Format |
|----------|--------------|---------|--------|
| **Program sponsor** | Top 5 risks; red/orange dependencies; escalation decisions needed | Every program review (biweekly/monthly) | One-page executive summary |
| **Team leads** | Their incoming and outgoing dependencies; full status of each | Weekly | Team-level dashboard slice |
| **Engineering teams** | Dependencies owed to them and by them; upcoming need-by dates | Weekly (integrated into team stand-up or status) | Lightweight view; integrated into team tools |
| **External partners** | Their commitments to the program; program assessment of their status | Per the contract's reporting cadence | Formal status report |
| **Program team** | Full dashboard; trend data; contingency consumption | Weekly | Living dashboard; reviewed in program status |

The TPM curates the view for each audience. The full dashboard is visible to everyone — but the TPM highlights what matters for each audience rather than expecting them to extract it themselves.

## The Program Status Integration

Dependency health is not a separate meeting. It is integrated into the existing program status cycle:

| Program Status Element | Dependency Health Content |
|------------------------|--------------------------|
| **Overall status** | Dependency health summary: green/yellow/orange/red counts; critical path status |
| **Accomplishments** | Dependencies resolved since last review |
| **Risks and issues** | Top dependency risks; blocked dependencies requiring escalation |
| **Decisions needed** | Escalation packages for blocked dependencies |
| **Forward look** | Dependencies coming due in the next period; watch-list additions |

The TPM spends five minutes of the program status review on dependency health — no more, no less. The dashboard carries the detail; the review carries the decisions.

## Trend Reporting

Point-in-time status is necessary but insufficient. Trends reveal whether the dependency portfolio is improving or deteriorating:

| Trend Metric | What It Measures | Signal |
|-------------|-----------------|--------|
| **Red/Orange ratio** | Percentage of dependencies at risk or blocked | Rising = dependency portfolio deteriorating |
| **Status age** | How long has each red/orange dependency been in that state? | Aging red = escalation failure |
| **Resolution rate** | Dependencies resolved per period vs new dependencies added | Below 1.0 = backlog growing |
| **Contract change frequency** | How often are contracts being renegotiated? | Rising = instability in the dependency portfolio |
| **Contingency consumption rate** | Buffer consumed per period vs buffer remaining | Ahead of plan = contingency will run out before program end |

The TPM reads trends at a monthly cadence and surfaces deteriorating trends to the sponsor. A program where the red count is steadily rising may be on track this month — but it will not be next month.

## The Dependency Health Review Meeting

For programs with many dependencies, a brief dedicated health review keeps the status cycle focused:

| Agenda Item | Duration | Purpose |
|-------------|----------|---------|
| Changes since last review | 5 min | New dependencies, resolved dependencies, status changes |
| Red and orange dependencies | 10 min | Each: current status, owner action, escalation status, next step |
| Yellow dependencies aging to orange | 5 min | Prevention: what needs to happen before these degrade |
| Upcoming need-by dates (next 2 weeks) | 5 min | Forward look: which dependencies are approaching their commitment dates |
| Escalation decisions | 5 min | Decisions needed from the group |

Total: 30 minutes, weekly. The meeting is attended by team leads and the TPM. The sponsor attends when there are escalation decisions to make — otherwise they receive the summary.

## Reporting to External Partners

External dependency health reporting is both a communication and a relationship-management tool:

| Practice | Why It Matters |
|----------|----------------|
| **Share your assessment before the meeting** | Give the partner time to prepare their response; no surprises |
| **Report on their commitments, not their process** | "Milestone 3 was due 10/01; status is Yellow" — not "your team seems disorganized" |
| **Document every status exchange** | External partners' status claims are commitments; written record matters if they fail |
| **Tie status to contract milestones** | Status is measured against the contracted deliverable, not against your evolving needs |
| **Escalate internally before telling the partner they are Red** | The internal response (contingency, re-scope) must be aligned before the hard conversation |

The external partner report is formal, factual, and documented. It is also the evidence trail if the relationship deteriorates into a contractual dispute — write every status report as if it may one day be read by a lawyer, because in the worst case it will be.

## Tooling and Automation

| Capability | Lightweight (spreadsheet) | Integrated (program management tool) |
|------------|---------------------------|--------------------------------------|
| Dependency register | Shared spreadsheet with conditional formatting | Tool-native dependency tracking |
| Status updates | Owners update the spreadsheet | Tool sends reminders; owners update in-tool |
| Dashboard generation | TPM manually creates views | Tool auto-generates dashboard and trend charts |
| Alerting | TPM manually checks for aging reds | Tool alerts when a dependency crosses a threshold |

The tool should reduce the TPM's administrative burden, not increase it. A shared spreadsheet with conditional formatting and owner-update columns is superior to a sophisticated tool that nobody uses because the login is a barrier. Start lightweight; add tooling when the dependency count or program complexity demands it.

## Practical Applications

- [ ] Dependency health dashboard exists and is visible to all participating teams
- [ ] Dashboard is stratified by health status, sorted by criticality
- [ ] Audience-specific views are curated: sponsor summary, team slice, external partner report
- [ ] Dependency health is a standing agenda item in program status reviews
- [ ] Trend metrics are tracked monthly; deteriorating trends escalated to sponsor
- [ ] External partner status reports are formal, factual, and documented

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Dashboard without audience** | Beautiful dashboard nobody reads | Curate views for each audience; integrate into existing meetings |
| **Status inflation** | Everything is green because nobody wants to report yellow | Psychological safety: yellow is early warning, not failure |
| **Report without action** | Health is reported but nothing changes | Every red and orange dependency has a dated action and owner |
| **One-size-fits-all report** | Sponsor gets the same detail as the team lead | Curated views for each audience |
| **No trend analysis** | Each report is a fresh snapshot; deteriorating trends are invisible | Track trend metrics monthly; surface deteriorating patterns |

## Success Indicators

- The dependency health dashboard is opened by team leads before program status meetings
- Red dependencies do not age beyond two status cycles without escalation
- Trends are tracked and deteriorating patterns are surfaced before they become crises
- External partners accept the program's status assessment as fair and factual

## Related Topics

- [[01_Dependency_Mapping]]: the map is the data source for the dashboard
- [[02_Dependency_Classification]]: dashboard sorting by criticality
- [[05_Dependency_Risk_and_Contingency]]: contingency consumption tracked in trends
- [[06_Unblocking_Escalations]]: escalation status in the dashboard
- [[../04_Risk_and_Issue_Leadership/05_RAID_Log_Management|RAID Log Management]]: RAID log as the risk-side dashboard counterpart

## Summary

Dependency health and reporting is the TPM's visibility discipline: a living dashboard that shows every dependency's status, sorted by criticality, with curated views for each audience. It is integrated into the program status cycle, trended over time, and drives action — because a red dependency on a shared dashboard is harder to ignore than a red dependency in a private spreadsheet. The dashboard is not the point; the decisions it triggers are the point.