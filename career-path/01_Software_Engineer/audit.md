---
title: "Audit: Software Engineer"
note_type: audit
career_path: software-engineer
created: 2026-10-06
tags:
  - career-path
  - audit
  - software-engineer
---

# Audit: Software Engineer

> **Verdict:** Clean and accurate, but it is the **thinnest folder in the map** — a single 509-word overview, while every other path has 37–71 notes. As the declared starting point it is the page most readers open first, and **"mid-level engineer" is used 218 times elsewhere as the comparison baseline, but this folder never defines it**.
>
> **Overall: 2 / 5** — the right skeleton, missing the body.

## Snapshot

| Metric | Value |
|---|---|
| Notes | 1 (`00_overview.md` only, no modules) |
| Words | ~509 (body) |
| Wikilinks | All resolve ✅ |
| Frontmatter | `title` + `tags` present; `created` missing |
| External sources | 1 (U.S. BLS) |
| Last changed | 2026-08-03 (before Applied AI and FDE paths existed) |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 2/5 | Lists six capability areas but has no topic notes behind any of them |
| Depth | 2/5 | One-line expectations per area; the vault's SWEBOK notes carry the knowledge, but nothing covers the *job* |
| Practice | 1/5 | No exercises, checklists, or templates |
| Progression | 3/5 | Good "Signals for Moving Forward" and "Evidence to Build", but no junior → mid → senior distinction |
| Sources | 1/5 | Only BLS + SWEBOK |
| Vault hygiene | 4/5 | Links resolve; `created` missing; stray self-link at the end |

## ✅ What's Good

- **Correct framing.** Positions the role as lifecycle work (requirements → design → test → operate), not "just coding".
- **Reuses the vault instead of duplicating it.** Each capability links to the matching SWEBOK area in `software-engineering-note/` (~400 notes), which is the right knowledge backbone.
- **Observable promotion signals.** "Deliver features with decreasing supervision" and "identify and communicate risks early" can be checked against real behavior.
- **Concrete evidence list.** For example, "a code review that explains a design trade-off" or "a production runbook" are real artifacts a manager can assess.
- **Simple progression diagram.** Learn the codebase → deliver → own a component → broaden → choose a path.

## ❌ What's Missing

1. **No detailed modules.** This is the only path without any. The SWEBOK notes cover *what software engineering is*; nothing covers *how to work effectively as an engineer on a team*.
2. **No definition of "mid-level".** Notes in Senior (63 mentions), Security (63), Data/ML (47), and Applied AI (43) repeatedly frame skills as "a mid-level engineer does X; a senior does Y", so this folder should own that baseline. It also folds junior (L3) and mid-level (L4) into one role, although most ladders separate them and the gap spans 2–4 years of growth.
3. **AI-assisted engineering.** In 2026, coding agents and assistants are part of a normal SWE workflow, but no note in the map teaches AI-assisted development as a workflow skill (verifying output, reviewing generated code, security, licensing, and when not to use it). Outside the two AI paths, the only related content is an "AI-Assisted Drafting Ethics" section in the Engineering Manager path.
4. **Daily-craft topics that SWEBOK doesn't teach:** debugging methodology, reading an unfamiliar codebase, Git/PR hygiene, giving and receiving code review, estimating small tasks, joining on-call, working with PM/design/QA, 1:1s and asking for feedback, and keeping a brag document.
5. **Incomplete next paths.** [[career-path/18_Applied_AI_Engineer/00_overview|Applied AI Engineer]] is missing (it was added 2026-09-09, after this note). The listed direct jumps also disagree with the target paths: SRE, Security, Data/ML, EM, TPM, Project Manager, and PM each list **only Senior** in their own `entry_from`.
6. **No self-assessment checklist.** Almost every module overview in other paths has one; the entry point doesn't.
7. **No reading list for this level.** Canonical sources are missing (see Recommended Additions).

## ⚠️ What to Improve

- **Stale framing.** The root overview still says detailed notes "will be created later", and the "Suggested Future Note Route" here is only a list of SWEBOK links.
- **Stray link.** `[[career-path/01_Software_Engineer/00_overview|Software Engineer]]` sits on its own line after `## Related`, which looks like a copy/paste leftover that links the note to itself.
- **Missing `created`.** Required by the vault's frontmatter convention.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `01_Leveling_Junior_to_Mid/` | Expectations matrix (scope, autonomy, ambiguity, impact, communication) for junior vs mid; self-assessment; this becomes the baseline every "mid-level engineer…" sentence elsewhere refers to |
| 🎯 High | `02_Daily_Engineering_Craft/` | `01_Reading_Unfamiliar_Codebases`, `02_Debugging_Method`, `03_Git_and_Pull_Request_Hygiene`, `04_Code_Review_Giving_and_Receiving`, `05_Testing_in_Daily_Work`, `06_Small_Task_Estimation` |
| 🎯 High | `03_AI_Assisted_Engineering/` | Working with coding agents, verifying generated code, context/prompting for code tasks, security and licensing risks, measuring real (not perceived) productivity |
| Medium | `04_Production_Basics/` | Shadowing on-call, reading dashboards and logs, following runbooks, joining an incident, blameless postmortems |
| Medium | `05_Collaboration_and_Growth/` | Working with PM/design/QA, running 1:1s with your manager, asking for feedback, brag document, a personal learning plan |
| Medium | `06_Choosing_a_Path/` | Decision guide that maps interests to the 18 paths (questions like "do I enjoy incidents?" → SRE) |
| Low | `07_Readiness_for_Senior/` | A short bridge into the Senior path's `09_Promotion_Evidence_and_Capstone` |

**Suggested sources to cite:** *The Missing README* (Riccomini & Ryaboy), *The Pragmatic Programmer* (Hunt & Thomas), *Software Engineering at Google* (Winters, Manshreck & Wright), *A Philosophy of Software Design* (Ousterhout), *Debugging: The 9 Indispensable Rules* (Agans), *Working Effectively with Legacy Code* (Feathers).

## Quick Fixes

- [ ] Add `created:` to the frontmatter
- [ ] Add Applied AI Engineer to `next_paths`, and reconcile direct next paths with the targets' `entry_from` (or mark them "via Senior")
- [ ] Remove the stray self-link under `## Related`
- [ ] Add a self-assessment checklist (even before the modules exist)
- [ ] Rename "Suggested Future Note Route" → "Foundation Reading Route"

## Related

- [[career-path/01_Software_Engineer/00_overview|Software Engineer overview]]
- [[career-path/02_Senior_Software_Engineer/audit|Senior Software Engineer audit]]
- [[career-path/audit|Career path audit (summary)]]
