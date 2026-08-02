---
tags: [audit, essential-documents, software-engineering, swebok, babok, pmbok, sebok, cybok, dmbok, ux-ui]
status: complete
version: 1.1
audited: 2026-08-03
---

# Essential Documents — Audit Report

> **Audit scope:** `F:\obsidian_note\swe-knowledge\document-template\00_Essential Document`
>
> **Evidence sources:**
> - `F:\obsidian_note\swe-knowledge\body-of-knowledge\` — SWEBOK v4, PMBOK v8, SEBoK v2, BABOK v3, CyBOK v1.1, DMBoK v2
> - `F:\obsidian_note\swe-knowledge\software-engineering-note\` — master index, 15 software-engineering knowledge-area overviews, detailed notes, HCI/UX notes
>
> **Audit date:** 2026-08-03
> **Audit mode:** Read-only review. No existing essential-document or profile file was rewritten by this audit.

## 1. Executive Verdict

### Verdict: 🟡 **AMBER — Broad and useful, but not yet source-faithful as a universal "essential" checklist**

The folder is **not missing the entire software-engineering lifecycle**. It already covers the major document families from discovery through retirement, and it is especially strong on the visible delivery artifacts: requirements, architecture, design, construction, testing, deployment, maintenance, security, data, quality, and configuration management.

The doubt is valid, but the problem is not simply "more documents are missing." The larger issue is **classification and operating logic**:

1. Several documents described by the BOKs as **conditional outputs, techniques, evidence, or tool-generated artifacts** are labelled as universal Must Have items.
2. Several **management/process artifacts** emphasized in the completed software-engineering notes are absent or under-prioritized.
3. The folder is a **document inventory**, not yet a complete **tailoring system** that tells a project which artifact to create, who owns it, what triggers it, and what evidence closes it.
4. The source links in the seven discipline checklists still point to the old, non-existent `F:\projects\orlita_md\...` location even though the actual source is now under `F:\obsidian_note\swe-knowledge\...`.
5. The three profiles are useful, but their numbers and priorities are not consistently reconciled with the source checklists.

**PO conclusion:** Do not rewrite the whole template because of this audit. First correct the information architecture and tailoring rules. Then add a small number of high-value missing artifact families. The current set is a good baseline, not a failed design.

## 2. What Was Audited

| Area | Files / evidence reviewed | Result |
|---|---|---|
| Essential-document folder | All 11 Markdown files, including overview, six discipline checklists, UX/UI checklist, and three profiles | ✅ Reviewed |
| BOK source set | SWEBOK, PMBOK, SEBoK, BABOK, CyBOK, DMBoK overviews and detailed chapters | ✅ Reviewed |
| Software-engineering notes | `Software Engineering Note Content.md`, 15 KA overviews, selected detailed notes for requirements, architecture, quality, testing, operations, SCM, management, economics, and UX/UI | ✅ Reviewed |
| Link integrity | Wikilinks in the essential-document folder resolved against the local vault | ✅ No broken wikilinks found in the audit |
| External source-path integrity | Literal filesystem paths embedded in source banners | 🔴 Stale paths found |
| Source/index consistency | Counts, discipline labels, lifecycle terminology, and cross-source taxonomy | 🔴 Contradictions found |
| Standards/reference governance | Editions, scope, and reference category attribution | 🔴 Needs normalization |
| Existing working-tree state | Git status before audit artifact creation | ⚠️ Pre-existing modifications existed outside this audit; they were not changed |

### Inventory snapshot

- Essential-document folder: **11 Markdown files**.
- SWEBOK checklist: **145 document rows** in its phase/cross-cutting tables.
- Profiles: approximately **37 / 125 / 215 document rows** for Small, Medium, and Large respectively when checklist rows are counted consistently.
- Software-engineering note: **15 software-engineering knowledge areas**, with the master index reporting approximately **85–95% topical coverage** across those areas.

The row counts are inventory counts, not a quality score. A high row count is not evidence that a project should create all rows.

## 3. Findings Summary

| ID | Priority | Finding | Impact |
|---|---|---|---|
| ED-A01 | 🔴 Must Fix | "Essential" is used for both universal minimums and conditional evidence | Teams may create too much paperwork or misunderstand what is actually required |
| ED-A02 | 🔴 Must Fix | Tailoring decision record is not a first-class artifact | There is no auditable explanation for included, combined, deferred, or omitted documents |
| ED-A03 | 🔴 Must Fix | Scope baseline / scope statement is not explicit in the SWEBOK checklist | Scope control and requirements-to-delivery boundary are weaker than the BOK/source notes imply |
| ED-A04 | 🔴 Must Fix | Software Engineering Management and measurement artifacts are underrepresented | Planning, estimation, control, measurement, and learning are weaker than build/test coverage suggests |
| ED-A05 | 🔴 Must Fix | Software process selection/adaptation and process monitoring are not explicit deliverable families | Methodology becomes a label rather than a controlled project decision |
| ED-A06 | 🔴 Must Fix | Source banners contain stale, non-existent filesystem paths | Users can be sent to the wrong source and source provenance is harder to audit |
| ED-A07 | 🔴 Must Fix | Requirements and design validation evidence is fragmented | The set lists requirements, reviews, tests, and sign-off, but not one clear validation/approval chain |
| ED-A08 | 🟡 Nice to Have | Release/operations coverage omits or under-prioritizes deployment verification, service support, and problem management | Production readiness can be treated as "deployment completed" rather than "service accepted and supportable" |
| ED-A09 | 🟡 Nice to Have | Lifecycle retirement/decommissioning is present in profiles but missing from the main SWEBOK phase checklist | The top-level software checklist is not actually complete across the six life-cycle stages it claims |
| ED-A10 | 🟡 Nice to Have | Duplicate and overlapping document identities are not normalized | Teams may create multiple artifacts for the same information or fail to know which is authoritative |
| ED-A11 | 🟡 Nice to Have | Profiles contain priority/count inconsistencies and profile boundaries are too coarse | A project can choose the wrong profile or trust stale quick-start counts |
| ED-A12 | 🟡 Nice to Have | UX/UI checklist needs a stronger source and accessibility update | The content is useful, but the provenance is vague and some standards are outdated |
| ED-A13 | 🟢 Optional | Foundation / professional-practice / economics artifacts are not represented in the core project checklists | Valuable for some contexts, but not universal project documents |
| ED-A14 | 🔴 Must Fix | The folder is called `document-template`, but its contents are catalogs/checklists rather than reusable templates | Users may expect fillable work products and miss the required metadata, approvals, and evidence model |
| ED-A15 | 🔴 Must Fix | Overview/source scope and lifecycle terminology contain contradictions | The collection cannot be treated as an authoritative index until counts, disciplines, and lifecycle meanings are reconciled |
| ED-A16 | 🔴 Must Fix | Reference/standard attributions mix normative standards, guidance, frameworks, taxonomies, and tools | Readers may incorrectly infer that a standard mandates an artifact or that an unrelated standard validates a control |

## 4. Detailed Findings

### ED-A01 — Priority semantics are too broad 🔴

**Evidence**

- `SWEBOK Essential Documents.md` labels many outputs as 🔴 Must Have, including an entire cross-cutting set of project management, quality, configuration, and security artifacts (`SWEBOK Essential Documents.md:183–249`).
- The same checklist correctly says at the end that document choice depends on context and that no single project needs every document (`SWEBOK Essential Documents.md:253–264`).
- The software-engineering notes explicitly support tailoring: requirements specification choice depends on context, and process/lifecycle choice must be adapted rather than applied dogmatically (`body-of-knowledge/SWEBOK/01_Software_Requirements.md:43–45`, `body-of-knowledge/SWEBOK/10_Software_Engineering_Process.md:38–43`).

**Assessment**

The final tailoring note conflicts with the table semantics. A "Must Have" row currently means at least three different things:

1. universal minimum for nearly every software project;
2. required when a particular condition exists, such as safety, regulated data, multiple teams, or production service operation;
3. an important output in a formal lifecycle, but not necessarily a standalone document in an Agile/Lean project.

**Recommendation**

Keep 🔴/🟡/🟢, but add a separate **Applicability** field and distinguish:

- `Universal minimum`
- `Conditional — trigger required`
- `Profile minimum`
- `Evidence / record produced by an activity`
- `Technique / model / implementation artifact`
- `Reference / optional enhancement`

Do not solve this by simply changing every red item to yellow. The missing information is the trigger and the acceptable lightweight form.

### ED-A02 — Tailoring decision record is missing as a first-class artifact 🔴

**Evidence**

- PMBOK source notes define tailoring as deliberate adaptation of the approach, governance, and processes to project context (`body-of-knowledge/PMBOK/11_Tailoring.md`).
- SEBoK states that life-cycle/process formality is selected and tailored by context (`SEBOK Essential Documents.md:183–195`).
- The profile notes mention tailoring, but none of the three profiles provides a dedicated **Tailoring Decision Record / Documentation Strategy / Project Information Management Plan** row.

**Assessment**

The set tells the reader that tailoring is allowed, but does not require the project to record the decision. This is the central governance gap: a project can omit documents without showing why.

**Recommendation**

Add one lightweight, universal artifact—possibly called **Project Tailoring & Documentation Strategy**—containing:

- project context and risk/criticality;
- selected lifecycle/methodology;
- applicable BOKs and standards;
- required artifacts;
- combined artifacts and their source-of-truth sections;
- explicitly omitted/deferred artifacts and rationale;
- review cadence and approval authority.

For a small project this can be one page. For a safety-critical project it can become a controlled plan and compliance matrix.

### ED-A03 — Scope baseline is not explicit in the SWEBOK checklist 🔴

**Evidence**

- The source requirements note treats vision, scope, success criteria, baselines, change control, and scope matching as core requirements-management concerns (`software-engineering-note/01_Software_Requirements/01_Requirements_Fundamentals.md:254–345`; `body-of-knowledge/SWEBOK/01_Software_Requirements.md:23–25, 47–53`).
- PMBOK explicitly lists **Project Scope Statement** as a planning output (`PMBOK Essential Documents.md:41–51`).
- `SWEBOK Essential Documents.md` lists BRD, SRS, requirements change log, and RTM, but no explicit **Vision & Scope / Product Scope Statement / Scope Baseline** row (`SWEBOK Essential Documents.md:24–45`).
- The large profile includes `Solution Scope` but not a clearly named product/project scope baseline as a standalone row (`Profile-Large-Safety-Critical.md:69–85`).

**Assessment**

A BRD or SRS may contain scope, but that is not guaranteed. The checklist claims to support change control and traceability while leaving the boundary artifact implicit.

**Recommendation**

Add a scope artifact with aliases rather than forcing a new document in every project:

> **Vision, Scope & Success Criteria** — may be a standalone document, a section of the business case/charter, or a controlled product/project scope statement.

It should contain in-scope, out-of-scope, users, outcomes, constraints, assumptions, release boundary, and success measures.

### ED-A04 — Management and measurement are underrepresented 🔴

**Evidence**

The completed notes contain dedicated artifacts or templates for:

- project initiation and scope;
- estimation and planning;
- measurement and metrics using ISO/IEC/IEEE 15939 and GQM;
- risk management and project control;
- software acquisition management;
- quality planning and process monitoring (`software-engineering-note/09_Software_Engineering_Management/06_Project_Initiation_and_Scope.md`, `07_Estimation_and_Planning.md`, `07_Measurement_and_Metrics.md`, `08_Risk_Management_and_Control.md`, `09_Software_Acquisition_Management.md`, `10_Quality_Planning_and_Process_Monitoring.md`).

The SWEBOK checklist contains Project Plan/SMP, Risk Register, WBS, Gantt/Schedule, RACI, and selected quality artifacts, but it does not make the following explicit in the core software checklist:

- **Estimation Basis / Estimate Record**;
- **Measurement Plan / Metrics Definition**;
- **Project Status / Performance Report**;
- **Process Monitoring or Improvement Record**;
- **Acquisition / Supplier Management Plan** when external software/services are used;
- **Lessons Learned / Retrospective Record** as a general management output.

`SWEBOK Essential Documents.md:183–199` is therefore much stronger on static governance names than on the empirical management loop described by the source notes.

**Recommendation**

Add a small management family, with conditional applicability:

1. **Project Management / Software Development Plan** — lifecycle, roles, work products, schedule, resources, quality, risk, and measurement approach.
2. **Estimate Record / Basis of Estimate** — size, assumptions, method, range/confidence, and actual-versus-estimate follow-up.
3. **Measurement & Metrics Plan** — goals, questions, metrics, collection owner, cadence, thresholds, and decisions enabled.
4. **Performance / Status Report** — scope, schedule, effort/cost, quality, risk, and forecast.
5. **Retrospective / Lessons Learned Record** — improvement actions and closure evidence.
6. **Acquisition / Supplier Record** — only when external software, COTS, SaaS, contractors, or open-source obligations create project risk.

For a startup these can be sections in one project hub; for larger projects they can be separate controlled records.

### ED-A05 — Process selection and process monitoring are not explicit 🔴

**Evidence**

- SWEBOK v4 treats software process as a knowledge area, including process definition, lifecycle selection, process adaptation, monitoring, assessment, and improvement (`body-of-knowledge/SWEBOK/10_Software_Engineering_Process.md:11–24`).
- The note includes dedicated material on process assessment/improvement and process monitoring/adaptation (`software-engineering-note/10_Software_Engineering_Process/07_Process_Assessment_and_Improvement.md`, `08_Process_Monitoring_and_Adaptation.md`).
- The essential folder has methodology descriptions and a tailoring note, but no explicit **Project Process Definition / Lifecycle Adaptation Record** or **Process Improvement Log**.

**Assessment**

"Agile," "Hybrid," or "V-Model" is currently treated as a profile label. A label does not define the actual lifecycle: activities, work products, entry/exit criteria, roles, decision gates, or feedback loops.

**Recommendation**

Make **Lifecycle & Process Definition** a conditional-but-near-universal artifact. For Agile, it can be a one-page working agreement plus Definition of Ready/Done and review cadence. For formal projects, it becomes the software development/process management plan. Add a **Process Improvement / Retrospective Log** as the feedback output.

### ED-A06 — Source paths are stale 🔴

**Evidence**

The seven discipline checklist files contain banners pointing to:

`F:\projects\orlita_md\software-engineering-note\...`

The actual audited source is:

`F:\obsidian_note\swe-knowledge\body-of-knowledge\...`

The stale path occurs in:

- `BABOK Essential Documents.md:11`
- `CyBOK Essential Documents.md:11`
- `DMBOK Essential Documents.md:11`
- `PMBOK Essential Documents.md:12`
- `SEBOK Essential Documents.md:12`
- `SWEBOK Essential Documents.md:12`
- `UX UI Essential Documents.md:11`

**Assessment**

This is a real correctness issue, not cosmetic. The local wikilinks still resolve, but the explicit source-of-truth path does not.

**Recommendation**

During the later template rewrite, replace hard-coded machine paths with either:

- local Obsidian wikilinks and a vault-relative path; or
- a single maintained `Source Registry` note containing the current paths.

At minimum, update the stale paths before treating the folder as a published reference.

### ED-A07 — Validation/approval evidence is fragmented 🔴

**Evidence**

The set has requirements, acceptance criteria, reviews, test cases, RTM, UAT sign-off, V&V, and validation documents across different sections. However:

- `SWEBOK Essential Documents.md:24–45` does not state one required chain from requirement quality → approval/baseline → implementation → verification → validation → acceptance.
- The software-engineering notes explicitly describe requirements reviews, inspection entry/exit criteria, acceptance criteria, traceability, and the need to distinguish verification from validation (`software-engineering-note/01_Software_Requirements/08_Prioritization_Validation_and_Reuse.md`, `13_ATDD_BDD_and_Acceptance.md`, `software-engineering-note/12_Software_Quality/07_Verification_and_Validation.md`).
- The quality note gives concrete phase gate entry/exit criteria and a quality-gate record template (`software-engineering-note/09_Software_Engineering_Management/10_Quality_Planning_and_Process_Monitoring.md:293–328`).

**Recommendation**

Add a **Verification, Validation & Acceptance Strategy** or make its minimum content mandatory in the Test Plan / V&V Plan. It should define:

- what is verified vs validated;
- review/inspection methods;
- test levels and acceptance method;
- entry/exit criteria;
- evidence and approvers;
- exceptions, waivers, and residual risk.

Add a reusable **Quality Gate / Readiness Review Record**. This is more valuable than adding more diagram types.

### ED-A08 — Operations is missing the service-support loop 🟡

**Evidence**

The SWEBOK v4 operations source defines planning, delivery, control, incident management, problem management, service reporting, telemetry, capacity, continuity, rollback, and operational testing (`body-of-knowledge/SWEBOK/06_Software_Engineering_Operations.md:11–18, 27–36`).

The checklist covers CI/CD, deployment, DR, incident process, runbook, release notes, rollback, SLA, capacity, IaC, monitoring, and SLO/SLI (`SWEBOK Essential Documents.md:143–161`). It does not clearly name:

- deployment/operational verification record;
- problem management / root-cause workflow;
- service support / service desk model;
- operational readiness review;
- backup restore or DR exercise evidence;
- service report / post-deployment validation.

**Recommendation**

Do not add all of these as standalone files for every project. Add them as conditional records under a **Service Operations & Readiness** family, triggered when the software is operated as a service or has meaningful availability/data-loss risk.

### ED-A09 — Retirement is absent from the main SWEBOK checklist 🟡

**Evidence**

The checklist describes six generic life-cycle stages and explicitly lists **Retirement** with data migration, archival, and decommissioning (`SWEBOK Essential Documents.md:253–264`). However, the seven main phases stop at Maintenance (`SWEBOK Essential Documents.md:165–179`).

The Large profile does include `System Disposal / Retirement Plan` (`Profile-Large-Safety-Critical.md:419`), but the SWEBOK checklist itself does not.

**Recommendation**

Add **Retirement / Decommissioning Plan** as a conditional lifecycle family, with:

- replacement/migration decision;
- data retention and deletion;
- dependency/client notification;
- shutdown and access revocation;
- archival and evidence retention;
- final security and operational checks.

### ED-A10 — Artifact identities overlap and are not normalized 🟡

**Evidence**

The SWEBOK checklist has exact duplicate labels for **Activity Diagrams** in Requirements and Design (`SWEBOK Essential Documents.md:37` and `:82`). It also repeats the same model families at different abstraction levels, which is valid only when the level and purpose are explicit.

Other overlaps across the folder include:

- RTM in requirements, testing, PM, and SEBoK;
- SRS / SyRS / requirements documentation;
- Design Review Records, Review Records, Technical Review Records, Audit Reports;
- Change Log, Change Request, MR/PR, Maintenance Log;
- Data Dictionary across design/data-management/profile sections;
- Release Notes and Version Description Document;
- Risk Register across PM, SE, security, and data.

**Assessment**

The overlaps are not inherently wrong; the same information may be viewed from different disciplines. The missing rule is **one authoritative record with linked views**, instead of multiple independent copies.

**Recommendation**

Add a document identity convention:

- canonical artifact name;
- aliases;
- lifecycle owner;
- source-of-truth location;
- whether other BOK rows are views, inputs, or evidence;
- minimum content and trigger.

For example, one `Requirements Traceability & Coverage Matrix` can serve BABOK, SWEBOK, PMBOK, SEBoK, and QA views without five separate spreadsheets.

### ED-A11 — Profiles need reconciliation 🟡

**Evidence**

- Small profile states approximately 30 documents in `Essential Documents - Overview.md:55–63`, but its quick-start contains 22 red items and its main profile table contains approximately 37 document rows.
- Medium and Large profiles use different representations of the same artifacts and have duplicated concepts such as `Data Dictionary` (reported in the audited row analysis for Medium and Large).
- In the Medium profile, `Information Architecture (IA)` is 🔴 in the main table (`Profile-Medium-Enterprise.md:138`) but is not mirrored cleanly in the quick-start red checkbox extraction; the profile's own checklist and priority table are therefore not perfectly synchronized.
- The profiles mix team size, timeline, methodology, and regulatory exposure as if they were one axis. A five-person safety-critical team and a fifty-person non-regulated platform do not have the same documentation needs.

**Recommendation**

Treat profiles as **starting presets**, not the decision authority. Add independent tailoring dimensions:

- project/system criticality;
- regulatory/contractual obligations;
- data sensitivity/privacy;
- operational exposure/SLA;
- integration/distribution complexity;
- team/distribution/turnover;
- lifecycle/methodology;
- product maturity (prototype, MVP, production, legacy).

Then generate the final project checklist from those dimensions.

### ED-A12 — UX/UI source and accessibility need tightening 🟡

**Evidence**

- `UX UI Essential Documents.md:7` names the source only as **UI Design — Industry Standards**, unlike the BOK-specific files.
- The UX/UI material in the software-engineering notes is based on Nielsen Norman Group, IDEO, Lean UX, information architecture, Atomic Design, Material Design, Apple HIG, and usability-testing practice (`software-engineering-note/03_Software_Design/Human Computer Interaction/` overviews).
- The checklist uses ISO/IEC 40500 (WCAG 2.0) (`UX UI Essential Documents.md:70`), while the notes discuss WCAG 2.2 and modern accessibility testing.

**Recommendation**

Make the UX/UI source register explicit and distinguish:

- user research evidence;
- interaction/design artifacts;
- usability evaluation evidence;
- accessibility requirements and audit evidence;
- implementation handoff assets.

Use a current WCAG target appropriate to the project and jurisdiction; do not imply that an old standard citation alone proves accessibility.

### ED-A13 — Professional practice, economics, and foundations are mostly guidance, not project documents 🟢

**Evidence**

SWEBOK includes Professional Practice, Economics, Computing Foundations, Mathematical Foundations, and Engineering Foundations (`body-of-knowledge/SWEBOK/SWEBOK v4 - Overview.md:43–50`). The core essential-document set includes little explicit project evidence for:

- ethics / legal / IP / open-source obligations;
- trade-off and build/buy decision records;
- estimate uncertainty and actual-versus-estimate learning;
- training/competency or responsibility evidence;
- domain foundation models or formal proof artifacts.

**Assessment**

This is not a defect in a small-project checklist. These areas mostly provide knowledge, techniques, constraints, or conditional evidence—not universal Markdown documents.

**Recommendation**

Do not add a document for every SWEBOK knowledge area. Add conditional rows only where a project trigger exists:

- **Decision / Trade-off Record** for consequential alternatives;
- **Open-Source / License Compliance Record** when dependencies or distribution create obligations;
- **Privacy / Legal / IP Review** when applicable;
- **Formal Specification / Proof Evidence** for high-integrity systems;
- **Competency / Independence Evidence** when regulation or contract requires it.

### ED-A14 — The folder is a catalog, not yet a template set 🔴

**Evidence**

- The folder is located under `document-template`, but the files are named and structured as `Essential Documents`, `Profile`, and `Overview` checklists rather than reusable work-product templates.
- The rows mix plans, specifications, models, registers, logs, reports, configurations, source code, test code, datasets, and evidence. Examples include `Source Code` and `Unit Test Code` in `Profile-Small-Startup.md:83–96`, and the broad BABOK concept of **Business Analysis Information** (`body-of-knowledge/BABOK/00_Introduction_to_BABOK.md:111–127`).
- The completed communication notes identify standard document metadata such as author, date, version, status, context, decisions, and references (`software-engineering-note/14_Software_Engineering_Professional_Practice/03_Communication_Skills.md:85–97`). The catalog rows do not provide a common schema for these fields.

**Assessment**

This is an expectation and information-architecture defect, not merely a naming preference. A catalog answers *what may exist*; a template answers *how to produce it*. The current folder does the first but not the second.

**Recommendation**

Either reframe the folder as **Essential Documents Catalog** or add a real template layer. Every canonical artifact should eventually have at least: purpose, owner, consumers, inputs, outputs, lifecycle point, applicability trigger, minimum form, formal form, status, approval/evidence, review cadence, retention, source references, and source-of-truth location.

### ED-A15 — Source/index scope and lifecycle terminology are contradictory 🔴

**Evidence**

- `Essential Documents - Overview.md:7,12–22,55–68` describes seven disciplines but says the folder contains extracts from six BOKs; this is only correct if UX/UI is explicitly labelled as a non-BOK practice source.
- `body-of-knowledge/Body of Knowledge - Overview.md:7–18` says there are five major BOKs while listing six, including DMBOK. The same file later says “all six.”
- The Quick Comparison in `Essential Documents - Overview.md:34–41` omits UX/UI even though UX/UI is listed as a discipline at `:22`.
- `PMBOK Essential Documents.md:5–9` calls Initiating, Planning, Executing, Monitoring & Controlling, and Closing “phases,” while the PMBOK source explicitly calls them **Focus Areas**, not phases (`body-of-knowledge/PMBOK/03_Project_Life_Cycles.md:200–203`).
- `SEBOK Essential Documents.md:5–9` uses `Concept → Architecture → Realization → Operations → Maintenance`, while SEBoK’s typical life-cycle stages are `Concept → Development → Production → Utilization → Support → Retirement` (`body-of-knowledge/System Engineer BOK/05_Life_Cycles_and_Processes.md:19–40`). Architecture is a technical process/knowledge area, not a replacement for the development stage.
- The local source notes themselves contain a PMBOK domain-count inconsistency: the overview identifies seven performance domains (`body-of-knowledge/PMBOK/PMBOK v8 - Overview.md:22–29,42–52`), while another local note refers to eight. CyBOK also claims 21 knowledge areas while its local enumeration requires reconciliation.

**Assessment**

These contradictions do not invalidate the useful content, but they prevent the folder from being treated as a clean source index. A future reader cannot know whether a label is a BOK fact, a local organizing choice, or a stale source-note error.

**Recommendation**

Add a source registry and scope statement that explicitly distinguishes:

- six formal BOK sources in this vault: BABOK, SWEBOK, PMBOK, SEBoK, CyBOK, and DMBoK;
- UX/UI/HCI as a complementary practice source, not a seventh BOK;
- lifecycle stages, technical processes, knowledge areas, and project-management focus areas;
- source facts versus the catalog’s chosen presentation order.

### ED-A16 — Reference and standard attributions need governance 🔴

**Evidence**

- The column labelled `ISO/IEEE Reference` contains NIST, GDPR, MITRE ATT&CK, OWASP, FIPS, OpenAPI, SPDX, CVE/CWE/CVSS, and other non-ISO/IEEE references (`SWEBOK Essential Documents.md:266–302` and the cross-disciplinary checklists).
- `SWEBOK Essential Documents.md:110` associates SBOM with ISO/IEC 5230. ISO/IEC 5230 is the OpenChain open-source license-compliance specification; SPDX is the relevant software-bill-of-materials specification family (ISO/IEC 5962).
- `SWEBOK Essential Documents.md:54` associates ADRs with ISO/IEC/IEEE 42010. 42010 governs architecture descriptions, views, and viewpoints; it does not define a particular ADR file format.
- `SWEBOK Essential Documents.md:237–246` uses ISO/IEC 27001 as a reference for SAST, security architecture, security requirements, DAST, and penetration testing. ISO/IEC 27001 is an ISMS requirements standard; it should not be presented as if it directly specifies each of those engineering artifacts.
- `SWEBOK Essential Documents.md:32–35` uses bare ISO/IEC/IEEE 29148 without an edition, while the current source reference is 29148:2018. The UML references use legacy ISO/IEC 19501 without explaining version or applicability.
- The external standard check during this audit confirmed current official scope for ISO/IEC/IEEE 42010:2022, ISO/IEC 25010:2023, ISO/IEC/IEEE 29119-1:2022, and ISO/IEC/IEEE 14764:2022.

**Assessment**

The catalog currently treats a normative standard, a practice guide, a framework, a taxonomy, a tool ecosystem, and a regulation as equivalent references. That creates false authority and makes later compliance claims unsafe.

**Recommendation**

Replace `ISO/IEEE Reference` with separate fields:

- `Normative standard / regulation`;
- `Guidance / professional source`;
- `Framework / method`;
- `Taxonomy / specification / tool`;
- `Edition and verification date`;
- `Applicability note`.

The catalog should state that a reference supports or informs an artifact; it should not imply that the reference mandates a standalone document unless that has been verified.

## 4A. Additional Corroborated Gaps by Lifecycle

The following items refine the original findings. They are not a request to create every item as a separate Markdown file; they are information/evidence families that the future catalog should represent.

| Lifecycle / concern | Additional gap confirmed in source notes | Recommended treatment |
|---|---|---|
| Concept and scope | Vision/product scope, feasibility/business case, scope baseline, WBS dictionary, estimate basis, cost/schedule baselines | Make explicit; allow sections in a charter/project hub for small projects |
| Requirements governance | Requirements Management Plan/procedure, requirements baseline, backlog/release roadmap, requirements review/inspection record, issue/TBD register, CCB charter/decision record, business rules catalog, transition requirements | Add as conditional evidence families; baseline and change decision should be minimum evidence |
| Product and release | User manual, installation/deployment guide, training materials, support knowledge base, deployment package, build/reproducibility record, release-readiness/go-no-go record, data migration/transition plan | Trigger by user-facing, deployable, operational, or migrating product context |
| Testing and quality | Test policy, test summary report, test execution/incident log, test environment/configuration record, test artifact archive, V&V report, quality gate record, CAPA, assurance/safety case | Keep organizational/safety artifacts conditional; require a lightweight readiness decision for releases |
| Operations and support | Operations plan, service reports, incident/postmortem records, problem/known-error records, service desk/escalation model, restore-test/DR exercise evidence | Trigger by production operation, availability, support, or data-loss risk |
| Security governance | Security/Secure-SDLC plan, security risk assessment, accepted-risk/exception record, security incident records, security posture report, dependency/OSS license compliance, final security assessment | Add applicability triggers: exposure, data sensitivity, supplier/distribution, and regulation |
| Retirement | Retirement/decommissioning plan, migration, archival, access revocation, final security/operations checks, decommissioning report | Add a lifecycle stage/family rather than leaving retirement only in prose |

These additions are supported by the local notes on project initiation and scope, requirements management, technical communication, test-process layers, operations/service support, and configuration auditing (`software-engineering-note/09_Software_Engineering_Management/06_Project_Initiation_and_Scope.md`, `01_Software_Requirements/10_Requirements_Management.md`, `14_Software_Engineering_Professional_Practice/03_Communication_Skills.md`, `05_Software_Testing/12_Test_Process_and_Measures.md`, `06_Software_Engineering_Operations/08_Service_Operations_and_Support.md`, and `08_Software_Configuration_Management/08_Configuration_Auditing.md`).

## 4B. Profile-Specific Corroboration

### Small / Startup

The Small profile is a sensible compact starting point, but it is not sufficient as a production minimum without explicit conditional gates. It currently lacks or under-prioritizes monitoring/telemetry, rollback, incident response/postmortem, restore testing, maintenance/deprecation, NFR/security requirements, access-control baseline, SCA/secret scanning, privacy/retention decisions, and lightweight usability/accessibility evidence (`Profile-Small-Startup.md:98–140`).

Recommended interpretation:

- **Always:** scope/objectives, backlog/stories/acceptance, ADRs, source/build/dependencies, automated tests, CI, deployment/release procedure, threat model, risk/decision log.
- **If production/stateful:** rollback, monitoring, incident contact/runbook, backup/restore test, recovery objectives.
- **If internet-facing or sensitive-data handling:** NFR/security requirements, access control, SCA/secret scanning, privacy/retention decision.
- **If user-facing:** lightweight research/assumption record and usability/accessibility validation.
- **After release:** maintenance/change history and retirement/deprecation notes.

`Product Backlog` should be labelled as an adaptive equivalent/derived artifact, not as a universal PMBOK document.

### Medium / Enterprise

The Medium profile’s audit-trail claim is stronger than its selected artifacts. It omits or does not clearly name benefits realization, assumptions, requirements management, issue/change/status reporting, resource calendars, funding requirements, risk reporting, incident response/BCP, vulnerability management, data lineage, metadata, data integration, FCA/PCA/status accounting, and CCB workflow. It also marks Information Architecture red in the table but does not preserve parity in the quick-start list.

### Large / Safety-Critical

The Large profile’s selection trigger conflates **scale/duration** with **safety criticality** (`Profile-Large-Safety-Critical.md:19–36`). A large or long project is not automatically safety-critical, and a small medical/industrial/embedded system may be safety-critical. Separate dimensions are needed for team/scale, system hazard/mission criticality, regulatory/certification, security, and contractual assurance.

For an actually safety-critical project, the current profile also needs explicit integrity-level assignment, hazard acceptance authority, FRACAS/anomaly evidence, full safety-requirements trace, as-built documentation, operational performance, obsolescence/DMSMS, and regulator-specific evidence mapping. FMEA/FTA and assurance artifacts should be upgraded by trigger, not merely by profile name.

## 5. BOK-by-BOK Assessment

### SWEBOK v4 — 🟡 Strong inventory, weak tailoring/control layer

**Good coverage:**

- All 15 software-engineering KAs represented in the source notes are reflected in the broader folder through direct or cross-cutting artifacts.
- The lifecycle sequence requirements → architecture → design → construction → testing → operations → maintenance is clear (`SWEBOK Essential Documents.md:24–179`).
- v4 additions—architecture, operations, security, and modern DevOps concerns—are present.
- The profiles add systems engineering, data, security, quality, and operations depth where appropriate.

**Main SWEBOK gaps:**

- explicit project process/lifecycle definition;
- management measurement loop;
- scope baseline;
- quality gate/readiness record;
- operational verification/support loop;
- retirement;
- explicit conditional treatment of construction/implementation artifacts;
- professional/economic decision evidence where triggered.

### BABOK v3 — 🟡 Conceptually broad, but outputs are presented as documents too literally

The BABOK checklist correctly covers the six knowledge areas and important outputs such as business objectives, current/future state, change strategy, solution scope, elicitation results, verified/validated requirements, traceability, prioritization, approval, and solution performance (`BABOK Essential Documents.md:23–94`).

The main issue is that BABOK describes **tasks and outputs**, not a mandatory set of standalone files. `Requirements (verified)`, `Requirements (validated)`, `Stakeholder Engagement`, and `Requirements (approved)` may be states/evidence recorded in a requirements repository, review record, or decision log rather than separate documents.

### PMBOK v8 — 🟡 Good catalog, but it mixes current PMBOK structure with older document terminology

PMBOK v8's local source notes emphasize seven Performance Domains and tailoring (`body-of-knowledge/PMBOK/PMBOK v8 - Overview.md:42–60`). The essential checklist is organized into five Focus Areas, which is acceptable as a document workflow, but the document should explain the mapping instead of implying that PMBOK v8 itself prescribes those document groups.

The checklist is strong on scope, schedule, finance, resources, risk, stakeholders, quality, communications, change, procurement, and closure (`PMBOK Essential Documents.md:24–182`). The main improvement is to distinguish **plan**, **baseline**, **register/log**, **report**, and **record** so the same artifact is not recreated for each domain.

### SEBoK v2 — 🟡 Strong systems lifecycle coverage, but too formal for the generic baseline

SEBoK coverage is broad: concept definition, ConOps, SyRS, architecture, ICD, trade studies, realization, verification, validation, transition, operations, retirement, SEMP, risk, CM, technical reviews, TPMs, safety, security, compliance, and domain-specific evidence (`SEBOK Essential Documents.md:24–179`).

The main concern is universal priority inflation. SEMP, TPMs, MBSE models, FCA/PCA, safety cases, FMEA/FTA, and retirement plans are crucial for the applicable system context, but not all are universal for ordinary software-only projects. The checklist does include a tailoring note; it should move that logic into the main selection mechanism.

### CyBOK v1.1 — 🟡 Useful security catalog, but many organizational controls are not software-project minimums

CyBOK coverage is comprehensive across governance, threat intelligence, operations, secure software, infrastructure, identity, cryptography, and advanced topics (`CyBOK Essential Documents.md:22–124`). However, ISMS, SoA, SIEM rules, IDS/IDPS deployment, HSM configuration, physical security, security awareness records, and full compliance reports are organization/security-program artifacts, not universal artifacts for every software project.

The profile should classify security artifacts by trigger: application, service/platform, organization, regulated environment, or high-assurance system.

### DMBoK v2 — 🟡 Comprehensive, but data-program scope is not separated from application scope

DMBoK coverage is extensive across all 11 domains (`DMBOK Essential Documents.md:22–168`). It is valuable for data-intensive platforms, but Enterprise Data Model, MDM strategy, data warehouse architecture, ECM strategy, metadata repository, and data asset valuation are not default requirements for a small application.

The main recommendation is to add data applicability triggers: personal data, regulated data, shared/master data, analytical/BI workload, multiple producers/consumers, data migration, or high data-loss impact.

### UX/UI — 🟡 Practical content, weak provenance and inconsistent project-size tailoring

The checklist has a coherent Research → UX → UI → Testing → Handoff flow and includes user research, IA, flows, wireframes, prototypes, mockups, components, accessibility, usability, analytics, and implementation handoff (`UX UI Essential Documents.md:23–102`).

The main issues are vague source attribution, outdated WCAG citation, and the use of universal Must Have for artifacts that are not applicable to every interface or project (for example, sitemap, responsive specification, asset export package, and A/B testing evidence).

## 6. Missing Artifact Families — Recommended Additions

These are the additions that would materially improve the set. They do **not** all need to become separate files.

| Priority | Artifact family | Minimum form for a small project | Formal form when required |
|---|---|---|---|
| 🔴 | Project Tailoring & Documentation Strategy | One-page project decision record | Controlled tailoring plan + compliance matrix |
| 🔴 | Vision, Scope & Success Criteria | Section in project hub/charter | Approved scope statement/baseline |
| 🔴 | Lifecycle & Process Definition | One-page process map + DoR/DoD | Software Development / Process Management Plan |
| 🔴 | Requirements & Design Baseline/Approval | Status fields + review decision | Controlled baselines and approval records |
| 🔴 | Verification, Validation & Acceptance Strategy | Test/quality section with entry/exit criteria | V&V plan + independent evidence + acceptance record |
| 🔴 | Quality Gate / Readiness Review Record | Release checklist with decision/owner/date | Formal stage-gate/review record with exceptions |
| 🔴 | Estimate & Measurement Record | Estimate assumptions + a few actionable metrics | Basis of Estimate + Measurement Plan + performance reports |
| 🔴 | Decision / Trade-off Record | ADR can cover architecture and major product choices | Formal trade study / decision analysis |
| 🟡 | Operational Readiness & Post-Deployment Validation | Runbook checklist and smoke-test record | ORR/PVR, service report, DR exercise evidence |
| 🟡 | Problem Management / RCA Record | Link from incident/defect to root cause/action | Controlled problem report and CAPA evidence |
| 🟡 | Supplier / Acquisition / License Record | Dependency/license note when applicable | Procurement and supplier assurance package |
| 🟡 | Retirement / Decommissioning Record | Checklist for shutdown and data handling | Retirement plan, archival evidence, final security review |
| 🟡 | Document Identity / Source-of-Truth Register | README table of canonical artifacts | Controlled information architecture / repository index |
| 🟢 | Formal Methods / Assurance Evidence | Not applicable unless triggered | Proof, safety case, independence, accreditation package |

## 7. Priority Reclassification Recommendations

These are not proposed edits today; they are the priority decisions to carry into the later rewrite.

### Move from universal 🔴 to conditional 🔴

- SLA, if the software is not operated as a service or no external service commitment exists.
- Disaster Recovery Plan, if the system has no meaningful availability/data-loss requirement.
- SAST/SCA/DAST/penetration test reports, depending on threat model, exposure, data sensitivity, and release risk.
- SBOM, depending on distribution, supply-chain, contractual, or regulatory requirements.
- UAT Sign-off, when there is no separate business acceptance role; retain an explicit acceptance decision in another record.
- QMS/ISMS/SoA, which are organization/program-level unless a contract or certification scope applies.
- FCA/PCA, SEMP, TPMs, safety case, FMEA/FTA, MBSE models, and formal technical review gates, unless systems/safety/contract context triggers them.
- Enterprise data model, MDM, data warehouse, ECM, metadata repository, and full data-governance artifacts, unless the data triggers apply.

### Upgrade from 🟡/implicit to 🔴 minimum evidence

- Tailoring/documentation strategy.
- Vision/scope/success criteria.
- Lifecycle/process definition.
- Requirements/design review and baseline decision.
- Verification/validation strategy and release/readiness decision.
- A minimal estimate record and a minimal risk record.
- Post-deployment validation for production services.

### Keep as conditional or optional

- QFD House of Quality, formal methods, ADL models, reference architecture, CRC cards, communication diagrams, feature models, pseudocode, diary studies, heatmaps, A/B testing, formal asset valuation, digital twins, hardware security evaluation, and organization-level maturity assessments.

## 8. Structural Recommendations for the Future Rewrite

Again, this audit does not rewrite the template. It defines the design rules for that future project.

### 8.1 Separate four layers

1. **Artifact catalog** — what information products can exist.
2. **Applicability rules** — when each product is triggered.
3. **Profile presets** — starting defaults for common contexts.
4. **Project checklist** — the final tailored selection for one project.

### 8.2 Use one row schema everywhere

Every catalog row should answer:

| Field | Question |
|---|---|
| Canonical name | What is the authoritative artifact called? |
| Aliases | What other BOKs or teams call it? |
| Purpose | What decision, risk, or handoff does it support? |
| Owner | Who is accountable? |
| Consumers | Who uses it? |
| Lifecycle point | When is it created/updated/retired? |
| Applicability trigger | What condition makes it required? |
| Minimum form | What is the lightest acceptable representation? |
| Formal form | What is required for regulated/high-risk contexts? |
| Inputs/outputs | What does it consume and produce? |
| Traceability | Which objective/requirement/design/test/release does it link to? |
| Approval/evidence | Who approves it and what proves completion? |
| Source BOK/standard | Which source supports it? |
| Source of truth | Where does the authoritative copy live? |

### 8.3 Treat document form as flexible

A BOK output may be:

- a standalone Markdown document;
- a section of a larger plan;
- a repository/issue-board view;
- a generated report;
- a model/diagram;
- a test/code/build artifact;
- a review or approval record.

The essential set should prescribe **information and evidence**, not always a separate file.

### 8.4 Add status and lifecycle metadata

Each project artifact should have at least:

- status: proposed / draft / reviewed / approved / baselined / superseded / archived;
- version or revision;
- owner;
- last reviewed date;
- next review trigger;
- related artifact links;
- exception/waiver reference if applicable.

## 9. Recommended Audit-Driven Rewrite Order

When the template rewrite becomes its own project, use this order:

### 🔴 Phase 1 — Correctness and governance

1. Fix stale source paths.
2. Add the common row schema and artifact identity rules.
3. Add Tailoring & Documentation Strategy.
4. Add Vision/Scope/Success Criteria.
5. Add Lifecycle/Process Definition.
6. Add Verification/Validation/Acceptance Strategy and Quality Gate record.
7. Reclassify universal vs conditional priority semantics.

### 🔴 Phase 2 — Software-engineering completeness

1. Add Estimate/Basis of Estimate.
2. Add Measurement & Metrics Plan and Performance/Status Report.
3. Add Requirements/Design baseline and approval evidence.
4. Add operational readiness/post-deployment validation.
5. Add retirement/decommissioning.
6. Add problem management/RCA and lessons learned.

### 🟡 Phase 3 — Cross-BOK harmonization

1. Normalize shared artifacts across BABOK, PMBOK, SWEBOK, SEBoK, CyBOK, and DMBoK.
2. Define canonical source-of-truth and linked views.
3. Separate project-level, product/service-level, organization-level, and regulatory evidence.
4. Rebuild profiles using independent applicability dimensions.
5. Recalculate quick-start counts from the actual profile tables.

### 🟢 Phase 4 — Optional depth

1. Formal methods and assurance evidence.
2. Advanced architecture evaluation and quantitative metrics.
3. Product-line/feature-model artifacts.
4. Advanced UX research and experimentation artifacts.
5. Organization maturity, data asset valuation, and digital-twin artifacts.

## 10. Quality Gate for the Future Template Rewrite

Do not call the rewritten template complete until all checks pass:

- [ ] Every artifact has a canonical name and aliases.
- [ ] Every 🔴 item has an explicit applicability definition.
- [ ] Conditional items identify their trigger.
- [ ] Each profile has a reproducible selection rule.
- [ ] Quick-start counts are calculated from the profile table, not hand-maintained.
- [ ] Universal, conditional, evidence, technique, and optional items are distinct.
- [ ] Scope, lifecycle/process, tailoring, measurement, quality gates, and retirement are represented.
- [ ] Shared artifacts have one source of truth and cross-BOK views.
- [ ] Organization-level security/data/quality artifacts are not silently imposed on every project.
- [ ] Standards are labelled as guidance, applicability criteria, or compliance obligations.
- [ ] Source paths are vault-relative or centrally registered.
- [ ] A sample small/startup project can complete the checklist without producing bureaucratic duplicates.
- [ ] A sample high-assurance project can trace needs → requirements → design → implementation → verification → validation → release → operation → retirement.

## 11. Final Answer to the Original Doubt

> **Are documents missing?** Yes—but fewer than it feels like.
>
> **Is the current essential-document set unusable?** No. It is a broad and valuable inventory.
>
> **Does it fully cover the body of knowledge as a project-ready selection system?** Not yet.
>
> **What is the biggest risk?** Not omission of another diagram. It is calling conditional outputs "essential" without a trigger, while under-documenting tailoring, scope, lifecycle process, measurement, quality gates, operational validation, and retirement.
>
> **Recommended next move:** Keep this audit as the baseline. In the later rewrite, build a tailored artifact catalog and profiles from applicability rules rather than adding documents indiscriminately.

## Related

- [[Essential Documents - Overview]]
- [[SWEBOK Essential Documents]]
- [[BABOK Essential Documents]]
- [[PMBOK Essential Documents]]
- [[SEBOK Essential Documents]]
- [[CyBOK Essential Documents]]
- [[DMBOK Essential Documents]]
- [[UX UI Essential Documents]]
- [[Software Engineering Note Content]]
- [[SWEBOK v4 - Overview]]
- [[PMBOK v8 - Overview]]
- [[SEBoK v2 - Overview]]
- [[BABOK v3 - Overview]]
- [[CyBOK v1 - Overview]]
- [[DMBoK v2 - Overview]]

## External standards checked during audit

- [ISO/IEC/IEEE 29148:2018 — Requirements engineering](https://www.iso.org/standard/72089.html)
- [ISO/IEC/IEEE 42010:2022 — Architecture description](https://www.iso.org/standard/74393.html)
- [ISO/IEC 25010:2023 — Product quality model](https://www.iso.org/standard/78176.html)
- [ISO/IEC/IEEE 29119-1:2022 — Software testing concepts](https://www.iso.org/standard/81291.html)
- [ISO/IEC/IEEE 14764:2022 — Software maintenance](https://www.iso.org/standard/80710.html)

> **Note:** Standards are cited here to validate scope/current edition direction. This report is not a legal, regulatory, or certification determination.

## Revision History

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-08-03 | Initial audit of the Essential Document folder against the local BOK sources and completed software-engineering notes |
| 1.1 | 2026-08-03 | Incorporated the completed parallel review findings: catalog-vs-template scope, source/index contradictions, canonical taxonomy, lifecycle gaps, standards governance, and profile corroboration |

## Handoff

| Recipient | What they need from this audit |
|---|---|
| Future template-rewrite work | Use Sections 6–10 as the rewrite backlog and quality gate; the corroborated additions are in Sections 4A–4B |
| PO / Founder | Decide whether the future system optimizes for small/startup speed, broad portfolio reuse, formal assurance, or all three through tailoring |
| Designer / UX | Use ED-A12 to refine UX/UI source provenance and accessibility applicability |
| Dev / Architect | Use ED-A03, ED-A04, ED-A07, ED-A08, and ED-A09 to close engineering lifecycle gaps |
| QA | Use ED-A07 to define verification, validation, acceptance, and readiness evidence |
| Security / Data | Use the trigger model in ED-A01 and Section 7 rather than treating organization-level controls as universal |

**Audit status: COMPLETE — no template rewrite performed.**
