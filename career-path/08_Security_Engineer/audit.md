---
title: "Audit: Security Engineer"
note_type: audit
career_path: security-engineer
created: 2026-10-06
tags:
  - career-path
  - audit
  - security-engineer
---

# Audit: Security Engineer

> **Verdict:** The **best role overview in the map** (mid vs senior table, 12-item senior self-assessment, role-boundary table, evidence table explaining what each artifact proves), backed by a coherent AppSec/product-security lifecycle (threat model → design → DevSecOps → verify → identity/data → detect/respond → govern) with an exercise in every note. Its weakness is **vocabulary and currency**. It teaches self-authored decision frameworks but almost never names the **industry standards** a security engineer is hired and interviewed on: OWASP Top 10/ASVS, CVSS/EPSS/KEV, SAMM, SSDF, SLSA, ATT&CK. It has **zero coverage of AI/LLM security** and **no cloud or Kubernetes security note**.
>
> **Overall: 3.5 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 50 (7 modules × 6 topics + 8 overviews) |
| Words | ~54k (avg ~1,080/note) |
| Wikilinks | All resolve ✅ |
| Module → topic navigation | ❌ Modules 01, 02, 05, 06, 07 list topics as `` `file.md` `` text (30 topics not clickable); `07_.../06_Security_Governance_and_Enablement` has **no inbound links** |
| External template paths | ⚠️ 89 references (84 lines) across 33 notes point to `F:\obsidian_note\document_template\14_Security\...`, a folder **outside this vault**, as plain text (not clickable, machine-specific) |
| Frontmatter | `created` missing in **50/50** |
| Exercises | **42/42** ✅ |
| Checklists / templates in topics | 0/42 · 0/42 (module overviews do have checklists) |
| Sources | 0/42 topics; industry frameworks named in only 3 notes |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 3/5 | Lifecycle is complete; cloud, AI/LLM, vulnerability classes, and modern crypto are missing |
| Depth | 4/5 | Strong decision frameworks (e.g., triage scorecard, remediation economics), but no technical vulnerability depth |
| Practice | 4/5 | Exercise in every note; topics lack checklists/templates (the overviews have them) |
| Progression | 5/5 | Best in the map: mid vs senior table, senior checklist, role boundaries, evidence table, practice loop |
| Sources | 1/5 | Industry standards are almost never named |
| Vault hygiene | 2/5 | 30 unclickable topics, 89 out-of-vault paths, `created` missing everywhere |

## ✅ What's Good

- **Role overview is a model for the whole map.** Every other path should copy its structure: the "What Senior Security Engineers Do Differently" table, the capability checklist that asks for *linked evidence*, the role-boundary table (Security Engineer vs Staff vs Security Architect vs Security EM), and the "evidence artifact → what it demonstrates" table.
- **Security as an enabler, not a gate.** The framing is about reusable patterns, automated guardrails, and champions, so the security engineer doesn't become a bottleneck.
- **Decision frameworks with economics.** For example, `07_.../02_Risk_Based_Triage` combines a seven-dimension triage scorecard, an exposure × consequence matrix, remediation economics (patch vs mitigate vs isolate vs accept), and "record why your priority differs from the tool score".
- **Covers the full lifecycle, including the operate and govern phases** that AppSec curricula often skip: detection engineering, incident command, recovery, compliance evidence, and security metrics.
- **The security champion operating model** (`03_.../06`) is a high-leverage topic most guides miss.

## ❌ What's Missing

1. **AI/LLM security (0 notes).** Nothing on prompt injection, agent tool permissions, model and data supply chain, data poisoning, or the OWASP Top 10 for LLM Applications. The Applied AI path has a guardrails module, but the security engineer is usually the one asked to threat-model and approve AI features.
2. **Vulnerability-class knowledge.** The OWASP Top 10, API Security Top 10, ASVS, and classes like injection, XSS, SSRF, and BOLA/IDOR are never named. The vault's `software-engineering-note/13_Software_Security/` covers some of them, but this path doesn't link to them. Security engineers are interviewed on exactly this.
3. **Cloud and Kubernetes security.** "Cloud security" is listed as a specialization in the overview, but there's no note on cloud IAM, misconfiguration/CSPM, cloud-native threat models, or Kubernetes security (0 mentions).
4. **Industry framework vocabulary.** The self-authored frameworks are good, but the path should map them to the standards readers will meet: CVSS / EPSS / CISA KEV / SSVC (triage), OWASP SAMM and NIST SSDF (program maturity), SLSA (supply chain), MITRE ATT&CK (detection), and NIST CSF / ISO 27001 / SOC 2 (governance).
5. **Applied cryptography currency.** Key management (KMS/HSM), crypto agility, and post-quantum migration readiness appear in only 1 note.
6. **Secure code review and disclosure programs.** Manual secure code review gets 2 mentions; bug bounty and vulnerability disclosure programs get 0.
7. **Specialization and certification map.** The overview lists specializations (AppSec, cloud, identity, D&R, product security, architecture, tooling) but gives no guidance on choosing between them, and certifications (e.g., OSCP, CSSLP, CCSP, CISSP) are never discussed. This matters for security careers more than most.

## ⚠️ What to Improve

- **Make the template references usable.** Replace `F:\obsidian_note\document_template\14_Security\Threat-Model.md` and similar with `obsidian://open?vault=document_template&file=14_Security%2FThreat-Model` URIs, or copy the security templates into this vault's `document-template/`. Plain absolute paths aren't clickable and break on any other machine.
- **Fix navigation.** Convert backtick filenames to wikilinks in modules 01, 02, 05, 06, 07, and link `07_.../06_Security_Governance_and_Enablement` from its module overview.
- **Add a "Standards Mapping" line to each "Core Frameworks" section,** e.g., "This scorecard complements CVSS + EPSS + KEV; here's when each misleads".
- **Add `created`** to all 50 notes.
- **Add checklists to topic notes.** The module overviews have self-assessment checklists, but topics have none. Add a short "Senior review checklist" per topic.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `08_AI_and_LLM_Security/` | `01_AI_Attack_Surface_and_Threat_Model`, `02_Prompt_Injection_and_Agent_Permissions`, `03_Model_and_Data_Supply_Chain`, `04_Reviewing_AI_Features`; map to the OWASP Top 10 for LLM Applications and cross-link [[career-path/18_Applied_AI_Engineer/00_overview\|Applied AI Engineer]] |
| 🎯 High | `04_Security_Verification_and_Testing/07_Vulnerability_Classes_and_OWASP.md` | Top 10 / API Top 10 / ASVS as a map, linking to the vault's `13_Software_Security` foundation notes |
| 🎯 High | `05_Identity_Access_and_Data_Protection/07_Cloud_and_Kubernetes_Security.md` (or a new module) | Cloud IAM, misconfiguration, CSPM, workload identity, Kubernetes RBAC/admission/policy |
| Medium | "Standards Mapping" in each Core Frameworks section | CVSS/EPSS/KEV/SSVC, SAMM, SSDF, SLSA, ATT&CK, NIST CSF/ISO 27001/SOC 2 |
| Medium | `02_Secure_Architecture_and_Design/07_Key_Management_and_Crypto_Agility.md` | KMS/HSM, rotation, crypto inventory, post-quantum migration planning |
| Medium | `03_Secure_Development_and_DevSecOps/07_Secure_Code_Review.md` | Review heuristics, high-risk code paths, review of AI-generated code |
| Low | `00_Specializations_and_Certifications.md` | Choosing AppSec / cloud / D&R / identity / architecture, plus which certifications matter for each |
| Low | `07_.../07_Bug_Bounty_and_Disclosure_Programs.md` | VDP policy, triage at scale, researcher relations |

**Suggested sources to cite:** OWASP (Top 10, API Security Top 10, ASVS, SAMM, Top 10 for LLM Applications), NIST SSDF (SP 800-218), SLSA, MITRE ATT&CK, FIRST CVSS & EPSS, CISA KEV, *Threat Modeling: Designing for Security* (Shostack), *Alice and Bob Learn Application Security* (Janca), *Security Engineering* (Anderson; free online), *Building Secure and Reliable Systems* (Google; free online).

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Threat Modeling & Risk | ✅ Strong | Credit Shostack's four-question frame; fix navigation |
| 02 Secure Architecture & Design | ✅ Strong | Add key management / post-quantum |
| 03 Secure Development & DevSecOps | ✅ Strong (champions note) | Name SLSA and SSDF; add secure code review |
| 04 Security Verification & Testing | ✅ Good | Add vulnerability classes / OWASP map |
| 05 Identity, Access & Data Protection | ✅ Good | Add cloud and Kubernetes security; fix navigation |
| 06 Detection, IR & Resilience | ✅ Good | Map detections to ATT&CK; fix navigation |
| 07 Vuln Mgmt & Governance | ✅ Good economics | Map to CVSS/EPSS/KEV/SSVC; link the unreachable governance note |

## Quick Fixes

- [ ] Replace 89 absolute `F:\...` template paths (33 notes) with `obsidian://` URIs (or in-vault copies)
- [ ] Convert backtick filenames to wikilinks in modules 01, 02, 05, 06, 07
- [ ] Link `07_Vulnerability_Management_and_Governance/06_Security_Governance_and_Enablement` from its module overview
- [ ] Add `created:` to all 50 notes

## Related

- [[career-path/08_Security_Engineer/00_overview|Security Engineer overview]]
- [[career-path/06_Software_Architect/audit|Software Architect audit]]
- [[career-path/18_Applied_AI_Engineer/audit|Applied AI Engineer audit]]
- [[career-path/audit|Career path audit (summary)]]
