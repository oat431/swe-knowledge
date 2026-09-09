---
title: "Retrieval Augmented Generation"
note_type: capability-topic
capability_area: llm-application-patterns
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - rag
  - retrieval
---

# Retrieval Augmented Generation

> Grounding generation in retrieved evidence: chunk and index a corpus, retrieve relevant passages at query time, and generate the answer from that retrieved context.

## Why This Is a Senior Skill

A mid-level engineer stands up a vector store, wires a framework's RAG chain, and calls the project done. A senior engineer owns retrieval quality end to end — chunking strategy, embedding choice, hybrid search, reranking — because the generator can never be better than the context it receives. When answers are wrong in a RAG system, the failure is usually retrieval (the right passage never made it into context), and the senior knows to measure and fix the retriever before touching prompts or swapping models.

The senior challenge is treating retrieval as a first-class software system: an index that is versioned and re-indexable, quality that is measured independently of generation, and a pipeline where every stage (chunk → embed → retrieve → rerank → augment) has a knob with a measured effect.

## Core Frameworks

### Pipeline Anatomy

| Stage | What It Does | Senior Questions |
|-------|-------------|------------------|
| Chunking | Splits documents into embeddable units | Does a chunk carry one idea and its own context? |
| Indexing | Embeds chunks into a searchable store | Metadata preserved? Re-indexable on document change? |
| Retrieval | Finds candidate chunks for the query | Dense, sparse, or hybrid? What k? |
| Reranking | Re-scores candidates for the top slots | Worth the added latency? Which reranker? |
| Augmentation | Assembles retrieved text into the prompt | Order, dedupe, citations, context budget |
| Generation | Produces the grounded answer | Temperature, output contract, refusal behavior |

### Chunking Strategy Trade-offs

| Strategy | Strengths | Weaknesses | Best For |
|----------|-----------|-----------|----------|
| Fixed-size with overlap | Simple, predictable, framework default | Cuts through ideas mid-sentence; diluted relevance | Homogeneous prose |
| Structure-aware (headers, sections, code blocks) | Preserves document logic; clean metadata | Requires parsing per format | Docs, manuals, codebases |
| Semantic/embedding-based splitting | Chunks align to topic boundaries | Slower indexing; depends on embedding quality | Heterogeneous corpora |
| Sentence-window / parent-child | Retrieves small, augments with surrounding context | More moving parts; larger context use | Dense technical content |

### Retrieval Strategy Trade-offs

| Strategy | How It Works | Wins On | Loses On |
|----------|-------------|---------|----------|
| Dense vector | Embed query, cosine/ANN search | Paraphrase, semantic similarity, multilingual | Exact codes, IDs, rare names, SKUs |
| Sparse (BM25) | Keyword/term matching | Exact tokens, part numbers, error codes | Paraphrase, synonyms, misspellings |
| Hybrid (dense + sparse, fused) | Combine scores (e.g., reciprocal rank fusion) | Broad coverage across both failure classes | Slightly more infrastructure and latency |

### Reranking Options

| Option | Quality Gain | Cost/Latency | When |
|--------|--------------|--------------|------|
| No rerank | Baseline | Zero added cost | Small corpora, high-relevance queries |
| Cross-encoder reranker (e.g., bge-reranker, Cohere Rerank) | Large, reliable | One extra model pass over top-N | Most production systems |
| LLM-based rerank | Best on subtle relevance | High token cost, latency | Low-QPS, high-stakes pipelines |

Retrieve wide (50–200 candidates), rerank narrow (top 5–10). Reranking the candidates beats enlarging k, because the generator's attention and the context budget — not candidate count — are the real constraint.

## In Practice

**Chunking is a retrieval-quality decision, not an implementation detail.** The chunk is the retrieval unit: too small and context is lost, too large and relevance is diluted. Choose chunk boundaries by document structure, keep 10–20% overlap, and evaluate chunking changes with retrieval metrics before changing anything else. A chunking experiment is cheap; a wrong chunk size shipped to production is not.

**Measure retrieval quality independently of answer quality.** Track recall@k, MRR, and nDCG on a labeled query–document set. A great answer built on the wrong passage is still wrong, and a good retriever feeding a weak generator is a generation problem — separate the two so you always know which layer is failing. Retrieval metrics are the earliest and cheapest signal of RAG health.

**Use hybrid retrieval by default for production corpora.** Real queries mix paraphrase ("how do I reset my access") with exact tokens ("error E-1042"). Dense-only search misses exact codes; sparse-only misses paraphrase. Hybrid fusion covers both classes and costs little compared to a wrong answer, so single-strategy retrieval needs a justification, not the other way around.

**Treat the index as a deployed artifact with a lifecycle.** Documents change, and stale chunks answer from outdated facts. Re-embed on a schedule or event, version the index together with the embedding model and chunking config, and run retrieval metrics against index changes exactly like a regression suite. An index is not a one-time import; it is a living component with a pipeline.

**Design augmentation for how models actually attend to context.** Models weight the beginning and end of context and miss material in the middle ("lost in the middle"). Order chunks by confidence, put the strongest evidence first, dedupe near-identical chunks, and prefix each with source metadata (title, section) so the generator can cite precisely and the middle never holds the best evidence.

**Cite from the retrieved context, and verify grounding against it.** Every claim in the answer should map to a retrieved passage; when none does, that is either hallucination or a retrieval miss — two different fixes. The grounding check lives in evaluation, but the pattern must make the context–answer mapping available to be checked in the first place.

## Practical Exercise

1. Choose a real corpus (about 100 pages of product docs or an internal wiki) and hand-label 25–30 query → relevant-passage pairs.
2. Implement fixed-size chunking plus dense retrieval; measure recall@5 and MRR as your baseline.
3. Add BM25 and hybrid fusion; measure the delta per query class (exact-token vs paraphrase).
4. Add a cross-encoder reranker over top-50 candidates; measure the quality delta against the added latency.
5. Classify the failing queries: missing from corpus, chunking artifact, embedding gap, or genuinely hard case.
6. Adjust chunking (structure-aware splitting) for the chunking-artifact class and re-run all metrics.
7. Freeze the winning configuration, version it alongside the index, and document it as the retrieval quality baseline.

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]: RAG fundamentals and the decision framework this extends
- [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]]: recall@k, faithfulness, and retrieval metrics
- [[computing-foundation-note/Artificial_Intelligence/07_NLP_and_Perception]]: embeddings and semantic similarity foundations
- [[05_Context_Injection_Patterns]]: augmentation is the hand-off point between retrieval and context design
- [[06_Pattern_Selection_and_Fallback_Design]]: deciding whether RAG is the right pattern at all
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]: the retrieval eval suite lives there

## Common Pitfalls

- Defaulting to 512-token chunks without evidence: chunk size is corpus-specific and must be measured
- Measuring only end-to-end answer quality: retrieval regressions hide behind generator variation
- Dense-only retrieval on corpora full of codes and identifiers: exact tokens never surface
- Retrieving top-3 and calling it done: too few candidates starve the generator — retrieve wide, rerank narrow
- Stale indexes: documents updated in the source system but never re-embedded

## Key Takeaways

- Generation quality is capped by retrieval quality: fix the retriever first
- Hybrid retrieval plus reranking is the production default, not an optimization
- Chunking is measurable: treat chunk strategy changes like code changes with a regression suite
- The index is a versioned artifact with an update lifecycle, not a one-time import
- Retrieved context must be ordered, deduped, and labeled so the model attends to it correctly
