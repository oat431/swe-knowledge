---
tags: [plan, restructure, template-library, ed-a14, spec-driven, decision-record]
status: Approved — D1-D4 decided 2026-09-23; Phase 1 complete
version: "0.2"
created: 2026-09-23
owner: PO (product-owner persona)
decision_owner: Panomete (Founder)
supersedes_context: "[[Essential Documents Audit - 2026-08-03]]"
companion_data: "[[Example-Contamination-Scan-2026-09-23]]"
---

# Template Restructure Plan — ED-A14 Full Rebuild

> **Purpose:** Turn `document-template/` (363 templates + catalog) from a *template library* into a *spec-driven documentation system* with applicability rules, tiered metadata, contamination-free examples, and a regenerated master index aligned to the Founder's new vision (beyond Small/Medium/Large profiles).
>
> **Trigger:** Re-audit of 2026-09-23 (reasoning-model pass) confirmed the 2026-08-03 audit findings are still open, found incomplete "fixed" claims, and produced a full example-contamination scan of all 363 templates.
>
> **Mode:** This is a PLAN. No template content is rewritten in this session.

---

## 1. Current State (verified 2026-09-23)

### 1.1 Inventory facts (machine-verified)

| Metric | Value |
|---|---|
| Total `.md` files | 377 |
| Reusable templates | 363 (24 category folders) |
| Catalog/checklist files (`00_Essential Document/`) | 12 |
| Review/audit docs | 2 (ISO Compliance Review, 2026-08-03 Audit) |
| Templates with valid YAML frontmatter | 363/363 ✅ |
| Templates with `[Project Name]` placeholder | 360/363 ✅ |
| Templates with Document Control section | ~74/363 ⚠️ (undeclared tiering) |
| Templates with Revision History | ~43/363 ⚠️ (undeclared tiering) |
| Stubs/empty files | 0 ✅ |

### 1.2 Verified defects still open

| ID | Defect | Evidence | Status |
|---|---|---|---|
| RC-01 | ISO 27001 cited without `:2022` | 25 occurrences across 17 template files — review doc claimed "all 11 files" fixed | ✅ **Fixed 2026-09-23** — normalized to `ISO/IEC 27001:2022`; verified 0 bare citations remain |
| RC-02 | Stale `F:\projects\orlita_md\...` source paths (ED-A06) | All 7 discipline checklists in `00_Essential Document/` | ✅ **Fixed 2026-09-23** — repointed to `F:\obsidian_note\swe-knowledge\body-of-knowledge\` and `...\software-engineering-note\03_Software_Design\Human Computer Interaction\` (both verified to exist); only historical audit docs retain the old path as defect evidence |
| RC-03 | TEMPLATE-CHECKLIST.md self-contradiction | Header "189 unique" vs summary "357" vs parsed rows "359" vs disk "363"; duplicate item #63, #71, #90; RACI Matrix at #76 AND #82 | 🔴 Open — **regenerate, don't patch** (see §3, decision D2 — now unblocked by the 7-tier vision) |
| RC-04 | `mindmap` Mermaid (violates house convention) | 5 files: Potential-Value, NFR Catalog, Enterprise-Readiness-Assessment, BA-Performance-Assessment, Content-Classification-Taxonomy | ✅ **Fixed 2026-09-23** — converted to `flowchart TB` preserving hierarchy; verified 0 mindmaps remain |
| RC-05 | Hardcoded 2023–2026 gantt dates instead of placeholders | 173 dates across 30 files | ✅ **Fixed 2026-09-23** — all gantt dates re-based to placeholder epoch `2000-01-01` = project start, relative offsets preserved, `%%` explanatory comment injected after `dateFormat`; verified 0 real-era dates remain in gantt blocks |
| RC-06 | Example-content contamination (AI personas may inherit fake data as real) | Baseline scan: 59 HIGH / 97 MEDIUM / 207 LOW — see [[Example-Contamination-Scan-2026-09-23]] | ✅ **HIGH band fixed 2026-09-24** — 49 files stripped to placeholders (re-scan: HIGH=1, documented Business-Case.md method-content exception); MEDIUM band handled in Phase 3 rebuild |

### 1.3 ED-A14 structural gaps (from 2026-08-03 audit, still unimplemented)

1. No **row schema** (canonical name, aliases, purpose, owner, trigger, minimum form, formal form, source of truth).
2. No **applicability tiers** — "Must Have" still conflates universal / conditional / profile / evidence / technique.
3. No **tailoring metadata** — nothing tells a project (or an AI persona) *when* to use a template and what the lightest acceptable form is.
4. No declared **heavy vs light tier** even though the split exists de-facto (74 with Document Control vs 289 without).
5. Duplicate/overlapping identities unresolved: `Business-Requirements` (01) vs `Business-Requirements-Document` (04); `System-Requirements-Specification` (03) vs `Software-Requirements-Specification` (04); `Risk-Management-Plan` vs `Risk-Report`; `Project-Schedule` vs `Schedule-Management-Plan`; plus RTM appearing in 04 AND 13.

---

## 2. Target State

A spec-driven documentation system where:

1. **Every template declares its contract** in frontmatter: `tier` (heavy/light/record), `applicability` (universal/conditional/profile/evidence/technique), `trigger`, `minimum_form`, `owner_role`, `source_of_truth_note`.
2. **One canonical artifact per concept** — overlaps resolved via alias fields, not duplicate files.
3. **Zero ambiguous example data** — examples are either fenced as `> [!example]` callouts with internally consistent content, or converted to `[placeholders]`.
4. **A regenerated master index** computed from disk (never hand-maintained counts), aligned to the Founder's new categorization vision (D2 — pending).
5. **House Mermaid conventions enforced**: `flowchart` for architecture, `treeView-beta` for trees, never `mindmap`, gantt dates as placeholders.
6. **Standards citations governed**: edition pinned (`27001:2022`, `29148:2018`), reference type separated (normative / guidance / framework / taxonomy).

---

## 3. Decisions Required Before Execution

| ID  | Decision                                                                                                                   | Owner        | Options                                                                                                                                                      | Recommendation                                                                                                                                                                                                               | Final                                                                                                                                                                               |
| --- | -------------------------------------------------------------------------------------------------------------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| D1  | **Purge or rebuild the 363 templates?**                                                                                    | Founder      | (a) In-place upgrade of all 363; (b) purge & rebuild only the ~120 HIGH+MEDIUM-value set; (c) hybrid: in-place for LOW-contamination files, rebuild for HIGH | **(c) Hybrid** — 207 LOW files need only frontmatter upgrade (mechanical, scriptable); 59 HIGH files need per-file contamination decisions; nothing is worth blind purge since frontmatter/structure quality is already high | Let purge and Rebuild so option B                                                                                                                                                   |
| D2  | **New categorization vision** — what replaces Small/Medium/Large? (Founder: "not only small medium large project anymore") | Founder      | e.g., by lifecycle phase / by persona (PO–Designer–Dev–QA–DevOps) / by artifact type (plan–spec–record–report) / multi-axis                                  | **Blocker for Phase 4** — the master index cannot be regenerated until this is defined. Bring the vision to the next grill session                                                                                           | it will categorized by 5 level, POC, Prototype, Internal, Small Prod, Medium, Production Grade, Mission Critical you can see further here: F:\obsidian_note\swe-knowledge\checklist |
| D3  | **Example policy per HIGH file**                                                                                           | Founder + PO | fence-as-example vs strip-to-placeholders, per file                                                                                                          | Default: strip ID/money/date data to placeholders; keep ONE fenced, internally consistent example per heavy template                                                                                                         | strip to place holders per file, but when i need further example i will ask other agents (or you) ourself                                                                           |
| D4  | **Overlap resolution** — merge or alias?                                                                                   | PO           | merge files vs keep files + alias/source-of-truth frontmatter                                                                                                | Alias first (non-destructive); merge only Business-Requirements vs BRD after D2 clarifies category ownership                                                                                                                 | go with recommend                                                                                                                                                                   |

---

### 3.1 PO Review of Founder Decisions (2026-09-23)

**D1 = (b) Purge & Rebuild.** Accepted with one consequence: Phases 2 and 3 merge into the rebuild — contaminated files and schema upgrades are no longer patched in place; each rebuilt template is regenerated with v2 schema + clean placeholders from the start. The contamination scan ([[Example-Contamination-Scan-2026-09-23]]) becomes the rebuild worklist/priority order rather than a patch list. Archive-don't-delete still applies (`99_Archive/` + git history).

**D2 = 7-tier maturity model.** Source verified: `F:\obsidian_note\swe-knowledge\checklist\release-checklist\release.md` §"Which Tier Am I?" defines exactly these tiers: 🧪 POC/Spike → 🔧 Prototype/MVP → 🏠 Internal Tool → 🟢 Small Production → 🔵 Medium Production → 🟣 Production Grade → 🔴 Mission-Critical/Regulated. (Note: the decision text says "5 level" but lists 7 — the checklist confirms **7 tiers**; the new TEMPLATE-CHECKLIST regeneration in Phase 4 will use the 7-tier model, mapping each template to the minimum tier that requires it.)

**D3 = strip to placeholders everywhere.** No fenced examples ship in templates; examples are generated on demand by agents when asked. This simplifies Phase 2/rebuild: no "make example internally consistent" work — just strip.

**D4 = alias-first (recommendation accepted).** Overlap pairs get `canonical_name`/`aliases`/`source_of_truth` frontmatter in the rebuild; merges deferred until tier mapping clarifies ownership.

---

## 4. Execution Phases

```mermaid
flowchart LR
    P1["Phase 1<br/>Correctness<br/>(scriptable)"] --> P2["Phase 2<br/>Contamination<br/>clean-up"]
    P2 --> P3["Phase 3<br/>Row schema +<br/>frontmatter tiers"]
    D2["D2: new vision<br/>decision"] --> P4["Phase 4<br/>Index regeneration<br/>+ purge decisions"]
    P3 --> P4
    P4 --> P5["Phase 5<br/>Quality gate<br/>verification"]

    style P1 fill:#c0392b,color:#fff
    style P2 fill:#c0392b,color:#fff
    style P3 fill:#e67e22,color:#fff
    style P4 fill:#27ae60,color:#fff
    style P5 fill:#2980b9,color:#fff
    style D2 fill:#8e44ad,color:#fff
```

### Phase 1 — Correctness sweep ✅ COMPLETE (2026-09-23)
1. ✅ Normalized 25 bare ISO 27001 citations → `ISO/IEC 27001:2022` across 17 files; re-scan: 0 remaining.
2. ✅ Repointed 7 stale `orlita_md` banners to live vault paths (targets verified to exist); re-scan: 0 remaining outside historical docs.
3. ✅ Converted all 5 `mindmap` diagrams to `flowchart TB` (hierarchy preserved); re-scan: 0 remaining.
4. ✅ Re-based 173 hardcoded gantt dates across 30 files to placeholder epoch `2000-01-01` (= project start), preserving relative offsets, with an injected `%%` comment; re-scan: 0 real-era gantt dates remaining.
5. ✅ TEMPLATE-CHECKLIST.md untouched — superseded by Phase 4 (RC-03).

> Historical audit docs (`ISO Standards Compliance Review.md`, `Essential Documents Audit - 2026-08-03.md`) were deliberately excluded from all sweeps — they are evidence records and must keep their original text.

### Phase 2 — Contamination clean-up ✅ COMPLETE (2026-09-24)
Executed per D3 (strip to placeholders, no fenced examples) by 4 parallel subagents over the HIGH worklist from [[Example-Contamination-Scan-2026-09-23]] (plan doc itself excluded): 17 Requirements-Engineering + 11 Business-Analysis + 11 Testing/Concept/Change + 10 Architecture/PM/Data/Security files; parent fixed 2 residuals (Prototypes.md named markers, verified scan update). Fully-worked examples reduced to ONE bracketed skeleton per section; method content (weights, scales, thresholds, formulas) preserved per D3 rules.
**Re-scan result: HIGH 59 → 1, MEDIUM 97 → 104, LOW 207 → 258.** The single remaining HIGH is Business-Case.md (score 16) — a documented policy exception: `$0` Do-Nothing semantics + scoring-matrix weights are method content, not fake data. Structural integrity verified across all 363 files: frontmatter intact, code fences balanced, 0 mindmaps. MEDIUM band re-scan deferred to the Phase 3 rebuild (files get regenerated with v2 schema anyway per D1(b)).

### Phase 3 — Row schema + tier metadata (mechanical + editorial) 🟡
1. Define the canonical frontmatter schema (v2): add `tier`, `applicability`, `trigger`, `minimum_form`, `canonical_name`, `aliases`, `source_of_truth`.
2. Script the frontmatter upgrade for all 363 files (defaults from category), then editorially set `applicability`/`trigger` for the ~246 currently-red items using the 2026-08-03 audit §7 reclassification guidance.
3. Resolve the 6 overlap pairs via D4 alias fields.

### Phase 4 — Index regeneration (BLOCKED on D2) 🟢
1. Founder defines the new categorization vision.
2. Generate the new master index **from disk** (script counts rows/files; no hand-maintained totals).
3. Purge decisions executed here: retire/merge files the new vision doesn't include; archive rather than delete (git history + `99_Archive/`).
4. Regenerate the three Project-Size checklists (or their successors) from the new index.

### Phase 5 — Quality gate (from 2026-08-03 audit §10, updated) 🔵
Re-run the scripted checks: citation editions, mindmap=0, hardcoded-dates=0, frontmatter schema conformance=100%, index totals==disk totals, contamination scan re-score (target: HIGH=0), wikilink resolution, and the two end-to-end traceability walkthroughs (small/startup path + high-assurance path).

---

## 5. Definition of Done

- [ ] All RC-01…RC-06 closed with machine-verified evidence
- [ ] All 363 templates carry v2 frontmatter schema
- [ ] Contamination scan re-run: 0 HIGH files
- [ ] New master index generated from disk; totals match file count exactly
- [ ] Every overlap pair has a declared canonical source of truth
- [ ] Quality gate Phase 5 passes end-to-end
- [ ] D2 vision recorded as a decision record in this plan's revision history

## 6. Risks

| Risk                                                   | Impact                    | Mitigation                                                                            |
| ------------------------------------------------------ | ------------------------- | ------------------------------------------------------------------------------------- |
| D2 vision never defined → Phase 4 stalls               | Index stays contradictory | Phases 1–3 proceed independently; index marked `⚠️ superseded-pending-D2` immediately |
| Mass purge destroys usable templates                   | Rework cost               | Archive-don't-delete policy; hybrid D1(c) preserves the 207 clean files               |
| Scripted frontmatter upgrade breaks Obsidian rendering | Vault usability           | Dry-run on 10 files per category; verify in Obsidian before full sweep                |
| Example stripping removes pedagogical value            | Template usability drops  | D3 keeps one fenced consistent example per heavy template                             |

## Revision History

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | 2026-09-23 | PO | Initial plan from reasoning-model re-audit; contamination scan of all 363 templates completed |
| 0.2 | 2026-09-23 | PO | Founder decisions D1(b purge&rebuild)/D2(7-tier maturity model, verified against release.md)/D3(strip to placeholders)/D4(alias-first) recorded in §3.1; Phase 1 executed and machine-verified — RC-01/02/04/05 closed; Phases 2+3 merged into rebuild per D1(b) |
| 0.3 | 2026-09-24 | PO | Phase 2 executed: 49 HIGH-contamination templates stripped to placeholders via 4 parallel subagents + parent verification; re-scan HIGH 59→1 (policy exception documented); structural integrity verified on all 363 files; RC-06 HIGH band closed |
