---
title: "Audit: Data and ML Engineer"
note_type: audit
career_path: data-and-ml-engineer
created: 2026-10-06
tags:
  - career-path
  - audit
  - data-engineering
  - ml-engineering
---

# Audit: Data and ML Engineer

> **Verdict:** Accurate, senior-minded, and exercise-driven, but **compressed into cheat-sheet form**: it has the **lowest words per note in the map** (~825 on average; the integration notes are 627–704 words). It also merges two distinct careers but gives **ML engineering only 1 of 7 modules**. The data-engineering half is broad and DMBOK-aligned. The ML half is missing experimentation, training at scale, and the AI-era data work (unstructured data, embedding pipelines) that dominates 2026 job descriptions.
>
> **Overall: 3 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 50 (7 modules × 6 topics + 8 overviews) |
| Words | ~41k (avg ~825/note, the lowest in the map) |
| Wikilinks | All resolve ✅ (the role overview's reading route uses bare DMBOK basenames like `[[02_Data_Architecture]]`, which works today but is fragile) |
| Module → topic navigation | ❌ Modules 03 and 04 list topics as `` `file.md` `` text (12 topics not clickable) |
| Module overview formats | ⚠️ 3 different structures (modules 01–02, 03–04, 05–07) |
| Frontmatter | `created` missing in **50/50** |
| Exercises | **42/42** ✅ |
| Checklists / templates in topics | 1/42 · 0/42 (module overviews have checklists) |
| Sources | Topics rarely cite; module overviews 05–07 have Sources sections |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 3/5 | Data engineering is well covered; ML engineering, experimentation, and AI-era data are underweight |
| Depth | 2/5 | Correct and dense, but too short to learn from (decision tables without mechanics) |
| Practice | 4/5 | Exercise in every note, self-assessment per module |
| Progression | 2/5 | No guidance on Data Engineer vs ML Engineer vs Analytics Engineer, and no levels |
| Sources | 2/5 | Classic references are rarely named (2 notes) |
| Vault hygiene | 3/5 | 12 unclickable topics, 3 overview formats, `created` missing everywhere |

## ✅ What's Good

- **Senior judgment in a small space.** For example, the streaming note gives a delivery-semantics decision matrix, windowing strategies with late-data handling, and exactly-once implementation patterns, then advises that "most teams over-specify exactly-once". This is the right instinct to teach.
- **DMBOK-aligned data breadth.** Architecture, modeling (including bitemporal modeling and a semantic layer/metrics store), integration (CDC, lineage), quality (profiling, observability, scorecards), and security/privacy are all present.
- **Modern concepts are present:** lakehouse, data contracts, data mesh-style ownership, data observability, feature stores, model cards, and drift monitoring.
- **Production engineering module.** Distributed systems for data, cost optimization, CI/CD for data and ML, and runbooks/on-call treat data systems as production systems.
- **Clear scope boundaries.** Module overviews 06 and 07 have "Scope Boundary" sections separating this path from Applied AI and SRE.

## ❌ What's Missing

1. **ML engineering depth.** Only module 06 (6 notes) is ML, and it covers the *ops* lifecycle (tracking, features, training, serving, drift, governance). Missing:
   - ML fundamentals for engineers: metrics, leakage, train/validation/test discipline, baselines
   - Experimentation and A/B testing: the overview's own "Signals" mention statistics and experimentation, but only 3 notes touch the topic
   - Training and serving at scale: GPUs, distributed training, model optimization (quantization, distillation); 4 passing mentions
   - Foundation-model work on the ML side: fine-tuning, embedding models, evaluation datasets; the boundary with Applied AI needs to be explicit
2. **AI-era data engineering.** Unstructured-data pipelines, document processing, chunking/embedding pipelines, and vector stores appear in 3 notes. In 2026, these are a core data-engineering responsibility that feeds the Applied AI path.
3. **Learning depth behind the decision tables.** Watermarks and event time vs processing time, stateful stream processing, dimensional modeling (Kimball star schemas, slowly changing dimensions; 3 mentions), and query/partitioning optimization are named or implied but not taught.
4. **Career shape.** No note distinguishes Data Engineer, Analytics Engineer, ML Engineer, and ML Platform Engineer, or how to choose between them.
5. **Data as a product.** Mentioned once. Ownership, SLAs/SLOs for datasets, consumers, and versioning as product practice tie the contracts and quality modules together.

## ⚠️ What to Improve

- **Rebalance or split the path.** Either (a) split into `09_Data_Engineer` and a new ML Engineer path, or (b) add 2–3 ML modules so that ML is at least a third of the content.
- **Deepen the shortest notes first.** Module 03 (627–704 words each) and module 04 (707–808 words) should roughly double, adding mechanics and a worked example to each decision table.
- **Unify module overviews.** Three formats are in use; adopt the 05–07 format (it includes Scope Boundary and Sources).
- **Fix navigation.** Convert backtick filenames to wikilinks in modules 03 and 04.
- **Make the reading route robust.** Replace bare `[[02_Data_Architecture]]`-style links with path-qualified `[[body-of-knowledge/DMBOK/02_Data_Architecture]]`.
- **Add `created`** to all 50 notes.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `08_ML_Engineering_Foundations/` | `01_Evaluation_Metrics_and_Baselines`, `02_Data_Leakage_and_Split_Discipline`, `03_Experimentation_and_AB_Testing`, `04_Training_at_Scale_and_GPUs`, `05_Model_Optimization_for_Serving` |
| 🎯 High | `03_Data_Integration_and_Interoperability/07_Unstructured_Data_and_Embedding_Pipelines.md` | Document ingestion, chunking, embedding refresh, vector store choices, lineage for AI data; cross-link [[career-path/18_Applied_AI_Engineer/00_overview\|Applied AI Engineer]] |
| 🎯 High | Deepen modules 03 and 04 | Watermarks, event time, and state for streaming; worked validation and observability examples for quality |
| Medium | `02_Data_Modeling_and_Design/07_Dimensional_Modeling.md` | Star schemas, slowly changing dimensions, grain, and when to use Data Vault |
| Medium | `01_Data_Architecture/07_Data_as_a_Product.md` | Dataset SLOs, ownership, consumer contracts, deprecation |
| Medium | `00_Career_Variants.md` | Data Engineer vs Analytics Engineer vs ML Engineer vs ML Platform; skills, evidence, and next steps for each |
| Low | `06_ML_Lifecycle_and_MLOps/07_Foundation_Model_Fine_Tuning_and_Evaluation_Data.md` | Where ML engineering ends and Applied AI begins |

**Suggested sources to cite:** *Fundamentals of Data Engineering* (Reis & Housley), *Designing Data-Intensive Applications* (Kleppmann), *The Data Warehouse Toolkit* (Kimball & Ross), *Streaming Systems* (Akidau, Chernyak & Lax), *Designing Machine Learning Systems* (Huyen), *Reliable Machine Learning* (Chen, Murphy et al.), *Hidden Technical Debt in Machine Learning Systems* (Sculley et al., NeurIPS 2015), Google's *Rules of Machine Learning*, *Trustworthy Online Controlled Experiments* (Kohavi, Tang & Xu), DMBOK v2.

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Data Architecture | ✅ Good | Add data as a product |
| 02 Data Modeling & Design | ✅ Good (bitemporal, semantic layer) | Add dimensional modeling |
| 03 Data Integration & Interoperability | ⚠️ Too short (627–704w) | Deepen; add unstructured/embedding pipelines; fix navigation |
| 04 Data Quality | ⚠️ Short (707–808w) | Add worked examples; fix navigation |
| 05 Data Security & Privacy | ✅ Good | Cross-link Security Engineer module 05 |
| 06 ML Lifecycle & MLOps | ⚠️ The only ML module | Add ML foundations, experimentation, training at scale |
| 07 Production Engineering | ✅ Good | — |

## Quick Fixes

- [ ] Convert backtick filenames to wikilinks in modules 03 and 04
- [ ] Path-qualify the DMBOK links in the reading route
- [ ] Add `created:` to all 50 notes
- [ ] Standardize module overviews on the 05–07 format

## Related

- [[career-path/09_Data_and_ML_Engineer/00_overview|Data and ML Engineer overview]]
- [[career-path/18_Applied_AI_Engineer/audit|Applied AI Engineer audit]]
- [[career-path/06_Software_Architect/audit|Software Architect audit]]
- [[career-path/audit|Career path audit (summary)]]
