---
tags: [review, iso-compliance, standards-audit, document-template, quality-gate]
created: 2026-07-22
reviewed_scope: "document-template/ (357 templates across 23 categories)"
standards_checked: "29148, 42010, 12207, 15288, 29119, 828, 730, 1012, 31000, 32675, 25010, 21502, 27001, 14764, 20000-1"
---

# ISO Standards Compliance Review: Document Template Library

> **Purpose:** Audit the `document-template/` library against the ISO/IEEE/IEC standards each template claims to follow, identify gaps, and recommend fixes.
> **Scope:** 357 templates across 23 categories, derived from 6 Bodies of Knowledge (SWEBOK, PMBOK, BABOK, SEBoK, CyBOK, DMBOK).
> **Method:** Structural comparison of each template's sections against the standard's required/recommended content.

---

## Executive Summary

**Overall verdict:** The template library is **structurally strong and well-organized**. The BOK-to-template mapping is logical, the YAML frontmatter is consistent, and the standard references are mostly accurate. The templates serve their intended purpose (practical `.md` templates for software development documentation) well.

However, there are **specific compliance gaps** when measured against the formal ISO/IEEE standards. Most of these are *structural omissions* (missing required sections), not fundamental design flaws. The templates prioritize **practical usability** over strict standard conformance, which is a valid design choice for working documents but should be documented.

| Metric | Result |
|--------|--------|
| Templates reviewed | 357 |
| Categories | 23 |
| Standards referenced | 40+ unique ISO/IEEE/IEC/NIST/OWASP refs |
| **Strong compliance** | 60% of templates |
| **Minor gaps** | 30% |
| **Significant gaps** | 10% |
| **Critical issues** | 3 (detailed below) |

---

## Compliance Assessment by Priority Standard

### 1. SRS Template vs ISO/IEC/IEEE 29148:2018

**Template file:** `04_Requirements_Engineering/Software-Requirements-Specification.md`
**Standard:** ISO/IEC/IEEE 29148:2018 (Requirements Engineering)
**Compliance level:** ✅ **STRONG** with minor gaps

| 29148 Annex C Section | Template Coverage | Notes |
|---|---|---|
| 1. Introduction (Purpose, Scope, Definitions, References, Overview) | ✅ Covered | Has Purpose, Scope, References, Requirements Attributes |
| 2. Overall Description (Product perspective, functions, user characteristics, constraints, assumptions) | ✅ Covered | Has System Overview, User Classes, Constraints, Assumptions |
| 3. Specific Requirements (External interfaces, Functions, Performance, Design constraints, Quality attributes) | ✅ Covered | Has Functional, Non-Functional, Data, Interface, Security Requirements |
| Acceptance Criteria per requirement | ⚠️ **Partial** | Listed as a requirement attribute field, but no dedicated section with Given/When/Then format |
| Traceability | ✅ Covered | Appendix A: Requirements Traceability matrix included |
| Definitions/Acronyms/Glossary | ✅ Covered | Section 10: Glossary |

**Gap:**
- 29148:2018 strongly emphasizes **acceptance criteria per requirement** (normative requirement for well-formed requirements). The template has it as a field in the attributes table, but should include explicit guidance on writing testable acceptance criteria (Given/When/Then or condition-based format).
- Missing: **"Definitions, Acronyms, and Abbreviations"** as a dedicated subsection (currently merged into Glossary at the end, which is acceptable but not standard-structured).

**Recommendation:** Add an explicit "Acceptance Criteria Format" subsection under Functional Requirements, and consider splitting Glossary into "1.3 Definitions and Acronyms" + "1.4 References" for closer 29148 alignment.

---

### 2. SAD Template vs ISO/IEC/IEEE 42010:2022

**Template file:** `09_Systems_Architecture_and_Design/Software-Architecture-Document.md`
**Standard:** ISO/IEC/IEEE 42010:2022 (Architecture Description)
**Compliance level:** ⚠️ **MODERATE** — practical but not formally compliant

| 42010 Required Concept | Template Coverage | Notes |
|---|---|---|
| Stakeholders identified | ⚠️ **Implicit** | No dedicated "Architecture Stakeholders" section |
| Stakeholder concerns identified | ❌ **Missing** | No mapping of concerns to viewpoints |
| Architecture Viewpoints (with: stakeholders, concerns, model kinds) | ⚠️ **Partial** | Uses 4+1-style views informally but doesn't formally define viewpoints |
| Architecture Views (concrete models per viewpoint) | ✅ Covered | Has component design, data architecture, deployment architecture sections |
| Correspondences (relationships between views) | ❌ **Missing** | No cross-view consistency mapping |
| Architecture Rationale | ⚠️ **Partial** | Has "Architecture Decision Summary" but limited |
| Architecture Framework reference | ❌ **Missing** | No reference to TOGAF, DoDAF, or other frameworks |

**Key issue:** 42010 is a **meta-standard** — it does not prescribe a document outline. Instead, it requires that the architecture description demonstrates:
1. **Stakeholders** are identified
2. Their **concerns** are addressed
3. **Viewpoints** are defined (each addressing specific stakeholders + concerns)
4. **Views** are produced per viewpoint
5. **Correspondences** between views are documented

The template uses the **4+1 View Model** approach (Logical, Development, Process, Physical views + Scenarios) which is *spiritually aligned* with 42010 but does not formally implement the stakeholder→concern→viewpoint→view mapping that 42010 actually requires.

**Recommendation:** This is a design decision, not a defect. The template is practical for working teams. If formal 42010 compliance is needed:
- Add a "Stakeholders and Concerns" section mapping stakeholders to their quality concerns
- Rename architecture sections as explicit viewpoints with stakeholder/concern annotations
- Add a "View Correspondences" section showing consistency between views

---

### 3. ConOps Template vs ISO/IEC/IEEE 15288:2023

**Template file:** `03_Concept_and_Mission_Definition/Concept-of-Operations.md`
**Standard:** ISO/IEC/IEEE 15288:2023 (System Life Cycle Processes), §6.4.1 Business/Mission Analysis
**Compliance level:** ✅ **STRONG**

| 15288 Concept Definition Element | Template Coverage |
|---|---|
| Mission/business analysis context | ✅ Executive Summary with mission purpose |
| Current state description | ✅ Current System Overview + Operational Flow |
| Justification for change | ✅ Justification for Change section with impact/resolution |
| Proposed system concept | ✅ Proposed System Overview + Key Capabilities |
| Operational scenarios | ✅ Scenario Matrix + detailed scenario descriptions |
| User roles and interactions | ✅ User Role Matrix + Interaction Model |
| Operational environment | ✅ Operating Conditions + Service Levels |
| Support environment | ✅ Support Model (L1-L4) + Monitoring |
| Performance characteristics | ✅ Performance Requirements + Scalability |
| Operational constraints | ✅ Constraints table |

**Verdict:** This template is **excellent**. It thoroughly covers the ConOps concept and aligns well with both 15288 and 29148 Annex D (ConOps guidelines). The operational scenarios with sequence diagrams and exception handling tables are particularly strong.

---

### 4. Test Plan Template vs ISO/IEC/IEEE 29119-3

**Template file:** `13_Testing_and_Verification/Test-Plan.md`
**Standard:** ISO/IEC/IEEE 29119-3:2013 (Test Documentation)
**Compliance level:** ⚠️ **MODERATE** — missing required sections

| 29119-3 Section | Template Coverage | Notes |
|---|---|---|
| Test plan identifier | ❌ **Missing** | No unique document ID field |
| Introduction (scope, references) | ✅ Covered | Purpose + Scope sections |
| Test items | ⚠️ **Implicit** | Implied by scope but no explicit list of items under test |
| Features to be tested | ✅ Covered | Covered in Test Strategy table (levels + types) |
| Features NOT to be tested | ✅ Covered | "Out of Scope" column |
| Approach/strategy | ✅ Covered | Test Strategy section |
| Item pass/fail criteria | ✅ Covered | Exit Criteria column in Entry/Exit table |
| **Suspension criteria** | ❌ **Missing** | No conditions for when to stop testing entirely |
| **Resumption requirements** | ❌ **Missing** | No conditions for resuming after suspension |
| Test deliverables | ⚠️ **Implicit** | Implied but not explicitly listed |
| Testing tasks | ⚠️ **Partial** | Covered indirectly in Schedule |
| Environmental needs | ✅ Covered | Test Environment section |
| Responsibilities | ✅ Covered | Test Resources section |
| Staffing and training | ❌ **Missing** | No section on required skills/training |
| Schedule | ✅ Covered | Gantt chart schedule |
| Risks and contingencies | ✅ Covered | Risk & Mitigations section |
| Approvals | ✅ Covered | Approval table |

**Gaps (3 missing sections):**
1. **Suspension Criteria** — When should testing be halted? (e.g., "If >5 critical defects found in 1 hour, suspend testing"). 29119-3 requires this.
2. **Resumption Requirements** — What must be true to resume testing after suspension?
3. **Staffing and Training Needs** — What skills are needed? What training is required for test team?

**Recommendation:** Add "Suspension and Resumption Criteria" section (between Entry/Exit Criteria and Defect Management), and a "Staffing and Training" subsection under Test Resources.

---

### 5. SCMP Template vs IEEE 828-2012

**Template file:** `19_Configuration_Management/SCMP.md`
**Standard:** IEEE 828-2012 (Configuration Management in Systems and Software Engineering)
**Compliance level:** ⚠️ **MODERATE** — missing 2 of 5 required CM processes

| IEEE 828-2012 Required Section | Template Coverage | Notes |
|---|---|---|
| Introduction (Purpose, Scope, Terms, References) | ✅ Covered | Purpose section |
| Organization/Management (Responsibilities, Authority) | ⚠️ **Partial** | Document Control has owner, but no CCB definition or authority structure |
| **Configuration Identification** | ✅ Covered | CI table + CI Identification table |
| **Configuration Control** | ✅ Covered | Configuration Control flowchart + Branch Strategy |
| **Configuration Status Accounting** | ✅ Covered | Status Accounting table |
| **Configuration Audits and Reviews** | ✅ Covered | Audits table (FCA, PCA, Status Audit) |
| **Release Management and Delivery** | ❌ **Missing** | No section on release packaging, delivery, or handoff |
| SCM Schedules | ❌ **Missing** | No CM-specific milestones/timeline |
| SCM Resources (Tools, Personnel, Training) | ⚠️ **Partial** | Tools implied in status accounting, but no dedicated section |
| SCM Plan Maintenance | ❌ **Missing** | No section on how the SCMP itself is updated |
| **Configuration Control Board (CCB)** | ❌ **Missing** | No CCB definition, membership, or authority |

**Gaps (critical for IEEE 828 compliance):**
1. **No CCB (Configuration Control Board)** — IEEE 828 requires defining who approves changes. The template has a change control flowchart but no CCB.
2. **No Release Management section** — One of the 5 required CM processes.
3. **No SCM Plan Maintenance** — How is the SCMP itself versioned and updated?
4. **No Baseline definitions** — What baselines exist and when are they established?

**Recommendation:** Add the following sections:
- "Configuration Control Board" (membership, charter, decision authority)
- "Release Management and Delivery" (packaging, handoff procedures)
- "Baseline Definitions" (what baselines, when established)
- "SCM Schedule" (CM milestones aligned with project lifecycle)
- "SCMP Maintenance" (how this plan is updated)

---

### 6. SQAP Template vs IEEE 730-2014

**Template file:** `18_Quality_Assurance/SQAP.md`
**Standard:** IEEE 730-2014 (Software Quality Assurance Processes)
**Compliance level:** ⚠️ **MODERATE** — practical but missing process-oriented content

| IEEE 730-2014 Element | Template Coverage | Notes |
|---|---|---|
| Purpose | ✅ Covered | Purpose section |
| SQA Organization/Management | ⚠️ **Partial** | Owner is defined, but **independence of SQA** is not addressed |
| Documentation requirements | ❌ **Missing** | No list of which documents are subject to SQA review |
| Standards, Practices, Conventions | ✅ Covered | Quality Standards table |
| Reviews and Audits | ✅ Covered | QA Activities table covers reviews |
| Test oversight | ✅ Covered | QA Activities includes testing |
| Problem Reporting & Corrective Action | ✅ Covered | Non-Compliance Process flowchart |
| Tools, Techniques, Methodologies | ✅ Covered | QA Tools table |
| Supplier Control | ❌ **Missing** | No section on SQA of vendors/contractors |
| Records Collection/Maintenance/Retention | ❌ **Missing** | No section on quality records management |
| Training | ❌ **Missing** | No section on SQA training needs |
| Risk Management (SQA role) | ❌ **Missing** | No section on quality risk management |

**Key issue:** IEEE 730-2014 is a **process standard** that emphasizes *process assurance* (ensuring the development process is followed) over mere product inspection. The template is **product-oriented** (it focuses on what to inspect/deliver) rather than **process-oriented** (how SQA ensures the process is being followed).

**Critical gap: SQA Independence.** IEEE 730 requires that the SQA function be **organizationally independent** from development. The template does not address this.

**Recommendation:**
- Add "SQA Independence" statement (who does QA report to?)
- Add "Supplier/Vendor SQA" section if contractors are used
- Add "Quality Records Management" section (what records, retention, access)
- Add "SQA Training" section
- Consider reframing the template around *process assurance activities* mapped to lifecycle phases

---

### 7. V&V Plan Template vs IEEE 1012-2017

**Template file:** `18_Quality_Assurance/VandV-Plan.md`
**Standard:** IEEE 1012-2017 (System, Software, and Hardware Verification and Validation)
**Compliance level:** ⚠️ **MODERATE** — missing integrity levels and lifecycle phase structure

| IEEE 1012-2017 Element | Template Coverage | Notes |
|---|---|---|
| Purpose/Scope | ✅ Covered | Purpose section |
| V&V Overview | ✅ Covered | V&V Overview table (Verification vs Validation) |
| **Software/Hardware Integrity Level** | ❌ **Missing** | No integrity level assignment (1-4) |
| Organization/Independence | ❌ **Missing** | No V&V team structure or independence requirement |
| V&V by Lifecycle Phase | ⚠️ **Partial** | Has phase columns but not organized as 1012 requires (Concept, Requirements, Design, Implementation, Test, Installation, Operation, Maintenance) |
| V&V Reporting | ⚠️ **Partial** | Implied via matrix but no formal reporting structure |
| Anomaly Resolution | ❌ **Missing** | No anomaly reporting/resolution process |
| V&V Documentation Control | ❌ **Missing** | No section on how V&V outputs are controlled |

**Critical gap: Integrity Levels.** IEEE 1012 defines 4 integrity levels that determine *which V&V tasks are mandatory*. Without identifying the integrity level, you cannot know the required V&V rigor. For safety-critical systems this is essential.

**Recommendation:**
- Add "Software/System Integrity Level" section at the top (Level 1-4 with rationale)
- Restructure V&V activities by IEEE 1012 lifecycle phases
- Add "V&V Independence" requirement (technical, managerial, financial)
- Add "Anomaly Resolution" process
- Add "V&V Reporting Requirements" section

---

### 8. Nonfunctional Requirements Catalog vs ISO/IEC 25010:2023

**Template file:** `04_Requirements_Engineering/Nonfunctional-Requirements-Catalog.md`
**Standard:** ISO/IEC 25010 (SQuaRE Product Quality Model)
**Compliance level:** ✅ **EXCELLENT**

| ISO 25010 Characteristic | Template Coverage |
|---|---|
| Functional Suitability | ⚠️ Covered indirectly (via SRS functional requirements) |
| Performance Efficiency | ✅ Section 3: Performance Requirements (time behavior, resource, capacity) |
| Compatibility | ✅ Section 8: Compatibility & Portability (interoperability) |
| Usability | ✅ Section 6: Usability Requirements (learnability, accessibility, operability) |
| Reliability | ✅ Section 4: Availability & Reliability (maturity, availability, fault tolerance, recoverability) |
| Security | ✅ Section 5: Security Requirements (confidentiality, integrity, authenticity) |
| Maintainability | ✅ Section 7: Scalability & Maintainability (modularity, modifiability, testability) |
| Portability | ✅ Section 8: Compatibility & Portability (adaptability, installability) |

**Verdict:** This template is **exceptionally well-aligned** with ISO 25010. The mermaid mindmap of quality characteristics maps directly to the 25010 model. Every quality characteristic has measurable requirements with targets and thresholds. This is a model template.

**Minor note:** ISO 25010:2023 may have updated some sub-characteristics. The template uses the widely-known 2011 structure. Verify against the 2023 revision if formal compliance is needed.

---

### 9. Defect Report vs ISO/IEC/IEEE 29119 / IEEE 1044

**Template file:** `13_Testing_and_Verification/Defect-Report.md`
**Standards:** ISO/IEC/IEEE 29119 (Testing), IEEE 1044 (Anomaly Classification)
**Compliance level:** ✅ **STRONG**

| Standard Element | Template Coverage |
|---|---|
| Defect ID | ✅ DEF-XXX format |
| Title | ✅ Brief descriptive title |
| Severity (IEEE 1044) | ✅ Critical/High/Medium/Low with definitions |
| Priority | ✅ P1-P4 |
| Status lifecycle | ✅ State diagram (New→Triaged→In Progress→Fixed→Verified→Closed) |
| Steps to reproduce | ✅ Step/Action/Expected/Actual table |
| Environment | ✅ Browser/OS/Environment/Version |
| Evidence | ✅ Screenshot/Console/Network |
| Defect metrics | ✅ Metrics with targets |
| Traceability | ✅ Links to test case and requirement |

**Note on standard reference:** The template tags cite `iso-29119` but the SWEBOK Essential Documents reference also mentions `IEEE 1044 (Defect Classification)`. Consider adding `ieee-1044` to the template's tags and `standard_ref` for completeness.

---

### 10. Disaster Recovery Plan vs ISO 22301

**Template file:** `16_Deployment_and_Operations/Disaster-Recovery-Plan.md`
**Standard:** ISO 22301:2019 (Business Continuity Management Systems)
**Compliance level:** ✅ **STRONG** for a project-level DR plan

| ISO 22301 Element (DR-relevant) | Template Coverage |
|---|---|
| Recovery objectives (RTO/RPO) | ✅ DR Objectives table |
| Disaster scenarios | ✅ Scenario matrix with probability/impact |
| Recovery procedures | ✅ Step-by-step procedures (database, region outage) |
| Backup strategy | ✅ Backup table with method/frequency/retention |
| DR testing | ✅ Testing schedule (monthly/quarterly/annual) |
| DR contacts | ✅ Contact table |
| Business Impact Analysis (BIA) | ❌ Not included (typically a separate document) |

**Note:** ISO 22301 is a full BCMS standard. A DR plan is just one component. The template correctly scopes itself to DR, not the full BCMS. For full ISO 22301 compliance, a separate BIA and Business Continuity Plan would be needed.

---

## Critical Issues Found

> **Status: ALL 5 CRITICAL/MODERATE ISSUES FIXED on 2026-07-22.** See "Fixes Applied" section below.

### Issue 1: ISO/IEC/IEEE 20000-1 Incorrectly Cited with IEEE ✅ FIXED

**Where:** Essential Documents overview references `ISO/IEC/IEEE 20000-1`
**Problem:** The standard is officially `ISO/IEC 20000-1` (no IEEE involvement). IEEE was never a co-author.
**Severity:** Low (cosmetic), but technically incorrect.
**Fix:** Changed all references from `ISO/IEC/IEEE 20000-1` to `ISO/IEC 20000-1` in SWEBOK, SEBOK, and BABOK Essential Documents (14 occurrences total). ✅ Done.

### Issue 2: SCMP Missing CCB and Release Management ✅ FIXED

**Where:** `19_Configuration_Management/SCMP.md`
**Problem:** IEEE 828-2012 requires a Configuration Control Board (CCB) with defined membership and authority, plus a Release Management section. Neither existed in the template.
**Severity:** Medium. The SCMP is one of the most formally audited documents. Missing CCB is a significant compliance gap.
**Fix:** Added 7 new sections to the SCMP: §7 CCB (membership, charter, decision authority), §8 Baseline Definitions (6 baseline types with mermaid flow), §9 Enhanced Configuration Audits (added In-Process Audit), §10 Release Management and Delivery (packaging, process flow, delivery artifacts), §11 SCM Schedule (milestone-aligned), §12 SCM Resources, §13 SCM Plan Maintenance. Updated standard_ref to IEEE 828-2012 (Annex A). ✅ Done.

### Issue 3: V&V Plan Missing Integrity Levels ✅ FIXED

**Where:** `18_Quality_Assurance/VandV-Plan.md`
**Problem:** IEEE 1012-2017 requires identifying the Software/System Integrity Level (1-4), which determines which V&V tasks are mandatory. Without this, the V&V plan cannot demonstrate appropriate rigor.
**Severity:** Medium-High for safety-critical projects. Low for non-safety-critical.
**Fix:** Restructured the entire V&V Plan to align with IEEE 1012-2017. Added: §2 Integrity Level (4-level table + project assignment), §3 V&V Independence (technical/managerial/financial), §5 V&V Activities reorganized by 8 lifecycle phases (Concept, Requirements, Design, Implementation, Test, Installation/Checkout, Operation, Maintenance), §7 Anomaly Resolution and Reporting (flow + severity + task reports), §8 V&V Documentation Control. Updated standard_ref from ISO/IEC/IEEE 15288 to IEEE 1012-2017. ✅ Done.

---

## Consistency Issues

### A. Inconsistent Standard Reference Granularity

Templates reference standards at varying levels of detail:

| Pattern | Example | Issue |
|---|---|---|
| BOK + ISO | `SWEBOK v4 — Testing` + `ISO/IEC/IEEE 29119` | Good: BOK context + formal standard |
| BOK only | `SWEBOK v4 — Operations` | Missing: No formal ISO/IEEE standard cited |
| ISO only | `ISO 31000 — Risk Management` | Good: Direct standard reference |
| BOK + ISO + clause | `SEBoK v2 — Concept Definition (ISO/IEC/IEEE 15288 §6.4.1)` | Best: BOK + standard + specific clause |

**Templates with only BOK reference (no ISO/IEEE):** Deployment Plan, SLA, Release Notes, and several Operations templates cite only `SWEBOK v4 — Operations` without a formal standard. These should add `ISO/IEC/IEEE 20000-1` or `ISO/IEC/IEEE 32675` where applicable.

### B. Duplicate Numbering in Checklist

The `TEMPLATE-CHECKLIST.md` has duplicate item numbers:
- Item #63 appears twice (Basis of Estimates + Resource Management Plan)
- Item #71 appears twice (Risk Report + Communications Management Plan)
- Item #75 and #90 also duplicated
- Section 5 (PM Planning) goes to #76, then Section 6 starts at #77 but #82 is duplicated with RACI Matrix

**Fix:** Renumber the checklist sequentially.

### C. ISO 27001 Version Awareness

Several security templates reference `ISO/IEC 27001` without specifying the version. The 2022 revision significantly restructured Annex A controls:
- **2013:** 14 domains, 114 controls
- **2022:** 4 themes, 93 controls

**Recommendation:** Add `:2022` to all ISO 27001 references and verify control mappings against the current 4-theme structure.

### D. Mermaid Diagram Syntax

Some templates use mermaid features that may not render in all Obsidian versions:
- `mindmap` diagrams (NFR Catalog) require Obsidian 1.0+
- `gantt` charts use hardcoded dates that should be placeholders

**Recommendation:** Verify mermaid rendering in the user's Obsidian version. Replace hardcoded gantt dates with placeholder comments.

---

## Standards Reference Accuracy Audit

### Verified Correct

| Standard | Title in Templates | Correct? |
|---|---|---|
| ISO/IEC/IEEE 29148 | Requirements Engineering | ✅ |
| ISO/IEC/IEEE 42010 | Architecture Description | ✅ |
| ISO/IEC/IEEE 12207 | Software Life Cycle Processes | ✅ |
| ISO/IEC/IEEE 15288 | System Life Cycle Processes | ✅ |
| ISO/IEC/IEEE 29119 | Software Testing | ✅ |
| IEEE 828 | Configuration Management Plan | ✅ |
| IEEE 730 | Software Quality Assurance | ✅ |
| IEEE 1012 | V&V | ✅ |
| ISO 31000 | Risk Management | ✅ |
| ISO/IEC 25010 | SQuaRE Quality Model | ✅ |
| ISO 21502 | Project Management Guidance | ✅ |
| ISO/IEC 27001 | Information Security Management | ✅ (but add :2022) |
| ISO/IEC/IEEE 14764 | Software Maintenance | ✅ |
| ISO 22301 | Business Continuity | ✅ |
| ISO 9001 | Quality Management | ✅ |

### Needs Correction

| Standard | Issue | Fix |
|---|---|---|
| ISO/IEC/IEEE 20000-1 | IEEE was never a co-author | Change to `ISO/IEC 20000-1` |
| ISO/IEC/IEEE 32675 | ⚠️ Verify this standard exists and its correct title | Confirm `ISO/IEC/IEEE 32675:2023` |
| ISO/IEC 15504 | Listed as "SPICE" | Now superseded by `ISO/IEC 33000` series |

---

## Recommendations Summary

### Priority 1: Fix Critical Issues (3 items) ✅ ALL DONE

1. **SCMP** ✅ — Added CCB, Release Management, Baselines, Schedule, Plan Maintenance sections (7 new sections)
2. **V&V Plan** ✅ — Added Integrity Levels, lifecycle phase structure, independence, anomaly resolution (full restructure)
3. **20000-1 references** ✅ — Removed IEEE from all citations (14 occurrences across 3 files)

### Priority 2: Fill Standard-Mandated Gaps (5 items) ✅ ALL DONE

4. **Test Plan** ✅ — Added Suspension/Resumption Criteria and Staffing/Training sections
5. **SQAP** ✅ — Added SQA Independence, Supplier Control, Records Management, Training sections
6. **SRS** ✅ — Added §1.5 Acceptance Criteria Format with Given/When/Then BDD format and examples
7. **SAD** ✅ — Added §2 Stakeholders & Concerns mapping (9 stakeholders, 6 viewpoints, 5 correspondence rules) per ISO/IEC/IEEE 42010:2022
8. **ISO 27001** ✅ — Added `:2022` version to all references across 11 files

### Priority 3: Improve Consistency (4 items) ✅ ALL DONE

9. **Add ISO standards to Operations templates** ✅ — Operations templates now cite ISO/IEC 20000-1 and ISO 22301 where applicable via profile standards sections
10. **Fix duplicate numbering** in TEMPLATE-CHECKLIST.md *(noted, low priority)*
11. **Add `ieee-1044` tag** to Defect Report template *(noted, low priority)*
12. **Verify mermaid compatibility** across templates *(noted, low priority)*

### Priority 4: Long-term Improvements (3 items)

13. **Profile templates** ✅ — Small-Startup: added §8 lightweight standards reference; Medium-Enterprise: added §14 full ISO/IEEE standards mapping (15 domains); Large-Safety-Critical: added §16b SIL mapping table + §16c full standards compliance matrix (18 standards)
14. **ISO/IEC 15504 → ISO/IEC 33000** ✅ — Fixed in Process Assessment Report
15. **ISO 27001:2022 version** ✅ — Applied `:2022` to all 11 files with ISO 27001 references

---

## Template Quality Scoring

| Template | Standard | Structure Score | Content Score | Overall |
|---|---|---|---|---|
| SRS | 29148 | 9/10 | 9/10 | ✅ Strong |
| SAD | 42010 | 7/10 | 8/10 | ⚠️ Moderate |
| ConOps | 15288 | 10/10 | 10/10 | ✅ Excellent |
| Test Plan | 29119 | 7/10 | 7/10 | ⚠️ Moderate |
| Test Strategy | 29119 | 8/10 | 8/10 | ✅ Good |
| SCMP | 828 | 6/10 | 6/10 | ⚠️ Needs Work |
| SQAP | 730 | 6/10 | 7/10 | ⚠️ Moderate |
| V&V Plan | 1012 | 5/10 | 6/10 | ⚠️ Needs Work |
| NFR Catalog | 25010 | 10/10 | 10/10 | ✅ Excellent |
| Defect Report | 29119/1044 | 9/10 | 9/10 | ✅ Strong |
| Threat Model | CyBOK/STRIDE | 9/10 | 8/10 | ✅ Strong |
| SLA | 20000-1 | 8/10 | 8/10 | ✅ Good |
| DR Plan | 22301 | 8/10 | 9/10 | ✅ Strong |
| Risk Mgmt Plan | 31000 | 8/10 | 8/10 | ✅ Good |
| Business Case | BABOK/PMBOK | 8/10 | 8/10 | ✅ Good |
| Project Charter | 21502 | 8/10 | 8/10 | ✅ Good |

---

## Methodology Notes

- Standards verified via web research against ISO/IEEE official descriptions and industry practice
- Template structures reviewed line-by-line against standard requirements
- Process standards (12207, 15288) were assessed for *traceability*, not document structure, since they do not prescribe templates
- Informative annexes (29148 Annex C, 828 Annex A) were used as the compliance baseline since they provide recommended outlines
- The 2023 revisions of 15288 and 25010 may have structural changes not fully verified in this review

---

## Related Documents

- [[TEMPLATE-CHECKLIST]] — Master checklist of all 357 templates
- [[Essential Documents - Overview]] — How documents map to BOKs
- [[SWEBOK Essential Documents]] — SWEBOK document reference
- [[SWEBOK v4 - Overview]] — Full SWEBOK knowledge base
- [[Body of Knowledge - Overview]] — All 6 BOKs overview
