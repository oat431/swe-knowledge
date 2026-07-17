---
document_type: Heatmap Report
version: "1.0"
status: Draft
author: "[UX Researcher]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [heatmap, click-tracking, ux-research]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Heatmap Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Heatmaps visualize where users click, scroll, and hover — revealing actual behavior vs. intended design.

## 2. Heatmap Tool

| Field | Detail |
|-------|--------|
| [Tool] | [Hotjar / FullStory / Microsoft Clarity] |
| [Data Period] | [YYYY-MM-DD to YYYY-MM-DD] |
| [Sessions Analyzed] | [X,XXX] |
| [Pages Analyzed] | [Top 5 user flows] |

## 3. Click Heatmaps

### 3.1 Customer Dashboard

| Area | Clicks | % of Total | Observation |
|------|--------|-----------|------------|
| [New Request Button] | [1,234] | [35%] | ✅ Primary CTA working |
| [Recent Request Row] | [890] | [25%] | ✅ Users finding requests |
| [Navigation — My Requests] | [567] | [16%] | ✅ Navigation clear |
| [Profile Menu] | [234] | [7%] | 🟢 Low — expected |
| [Help Link] | [12] | [0.3%] | ⚠️ Very low — consider removing |

### 3.2 Request Form

| Area | Clicks | % of Total | Observation |
|------|--------|-----------|------------|
| [Next Button] | [1,100] | [40%] | ✅ Clear action |
| [Save Draft] | [450] | [16%] | ✅ Users saving progress |
| [Form Fields] | [980] | [36%] | ✅ Normal interaction |
| [Back Button] | [200] | [7%] | 🟢 Expected navigation |
| [Help Icons] | [23] | [0.8%] | ⚠️ Very low — inline help not noticed |

## 4. Scroll Heatmaps

| Page | 25% Scroll | 50% Scroll | 75% Scroll | 100% Scroll |
|------|-----------|-----------|-----------|------------|
| [Customer Dashboard] | [95%] | [85%] | [60%] | [35%] |
| [Request Form — Step 1] | [98%] | [90%] | [80%] | [75%] |
| [Request Detail] | [90%] | [70%] | [50%] | [30%] |
| [Admin Dashboard] | [95%] | [80%] | [55%] | [30%] |

**Insight:** 65% of users don't scroll past 75% on the dashboard. Move critical KPIs above the fold.

## 5. Rage Clicks

| Area | Rage Clicks | Issue | Recommendation |
|------|-----------|-------|---------------|
| [Upload Button] | [45] | [No feedback on click] | [Add loading spinner] |
| [Status Badge] | [23] | [Expected it to be clickable] | [Make clickable → detail] |
| [Filter Dropdown] | [18] | [Dropdown not opening] | [Fix z-index issue] |

## 6. Dead Clicks

| Area | Dead Clicks | Issue | Recommendation |
|------|-----------|-------|---------------|
| [Logo] | [67] | [Expected navigation to home] | [Make logo → home] |
| [Request ID] | [34] | [Expected to copy] | [Add copy-to-clipboard] |
| [Table Header] | [23] | [Expected sorting] | [Add sort functionality] |

## 7. Recommendations

| # | Finding | Recommendation | Priority |
|---|---------|---------------|---------|
| 1 | [Help links barely clicked] | [Remove or redesign help placement] | 🟢 |
| 2 | [Rage clicks on upload] | [Add upload progress indicator] | 🔴 |
| 3 | [65% don't scroll past 75%] | [Move KPIs above fold] | 🟡 |
| 4 | [Dead clicks on logo] | [Make logo clickable → home] | 🟡 |
| 5 | [Dead clicks on Request ID] | [Add copy-to-clipboard] | 🟢 |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Analytics-Dashboard-Spec]] | Metrics tracking |
| [[Usability-Test-Report]] | Qualitative complement |
| [[User-Flows]] | Flows being analyzed |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** Heatmaps show *what users actually do* — not what they say they do. Use them to validate design decisions and find unexpected behavior.
