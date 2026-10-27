---
title: "Multimodal and Voice Applications"
note_type: capability-topic
capability_area: llm-application-patterns
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
created: 2026-10-21
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - multimodal
  - voice-pipelines
---

# Multimodal and Voice Applications

> Adding document, vision, and voice modalities to LLM applications: pipelines, latency budgets, and evaluation that changes with the modality.

## Why This Is a Senior Skill

A mid-level engineer drops a PDF or an image into a multimodal model call, gets a plausible answer, and calls the feature done. A senior engineer treats each new modality as an architectural commitment: what extraction and grounding pipeline feeds the model, how latency expectations shift when the input is a spoken stream instead of a text box, and how evaluation must change because a transcription error, a misread table, and a hallucinated clause fail in different ways and are caught by different checks. Modality work looks like plumbing until the first incident where the answer cites a document region that says the opposite.

The senior question is never "can the model perceive this?" but "what does this modality add to the failure surface, what does it add to the latency budget, and is the product still defensible when perception fails?"

## Core Frameworks

### Modality Taxonomy

| Modality | Input Form | What Changes in the Architecture | Characteristic Failures |
|----------|-----------|----------------------------------|-------------------------|
| Text | Typed or pasted strings | Nothing structural; standard prompt and context pipeline | Hallucination, instruction drift |
| Documents | PDFs, scans, spreadsheets, slides | Extraction stage (text layer vs OCR), layout-aware chunking, source-region tracking | Misread tables, lost headers, wrong page order |
| Images | Photos, screenshots, charts, diagrams | Vision input in the model call or a caption-then-text fallback; resolution and crop policy | Miscounted objects, unread text-in-image, chart axis confusion |
| Audio | Recorded speech, calls, meetings | Speech-to-text stage ahead of any language work; diarization and timestamps | Transcription errors on names, numbers, domain terms |
| Voice | Real-time spoken conversation | Full-duplex pipeline: streaming STT, turn detection, streaming TTS or speech-to-speech | Bad endpointing, talking over the user, synthesis artifacts |

### Pipeline Stages by Modality

| Stage | Document Path | Vision Path | Voice Path |
|-------|--------------|-------------|------------|
| Ingestion | Parse text layer or run OCR; keep page and bounding-box metadata | Normalize resolution; decide full image, crop, or tiles | Capture stream; noise suppression; voice activity detection |
| Understanding | Layout-aware chunking; table extraction; page-level grounding | Send image to a vision-capable model, or caption to text | Streaming speech-to-text with partial hypotheses |
| Model call | Text or native multimodal call over retrieved chunks | Multimodal call with image plus question | Text call over the transcript, or speech-to-speech model |
| Response | Answer plus citations to page or region | Answer plus region grounding where possible | Turn-taking controller; streaming synthesis |
| Verification | Citation check against source regions | Spot-check answers that depend on small visual detail | Transcript confidence checks; text fallback display |

### Latency Expectations and Evaluation by Modality

| Modality | Latency Expectation | Streaming Behavior | Primary Eval Concern | Privacy Sensitivity |
|----------|--------------------|--------------------|----------------------|---------------------|
| Text chat | Seconds to first token | Token streaming assumed | Faithfulness and answer quality | Prompt content |
| Document Q&A | Seconds, after a longer parse step | Stream the answer only after parsing completes | Faithfulness to source regions; extraction accuracy | Documents are often confidential |
| Image understanding | Comparable to text chat | Little useful partial output | Visual grounding correctness | Images may contain people, IDs, screens |
| Async audio | Minutes acceptable for recordings | Progress feedback, not token streaming | Transcription error rate plus downstream answer quality | Voice is biometric data; consent and retention rules apply |
| Live voice | Sub-second turn cadence | Partial transcripts and barge-in are mandatory | Turn-taking behavior; end-to-end task completion | Continuous recording triggers consent obligations |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Raw Input"] --> B["Ingestion and Extraction"]
    B --> C["Normalization and Chunking"]
    C --> D["Model Call"]
    D --> E["Grounding and Verification"]
    E --> F["Response Delivery"]
    F --> G["Modality Specific Evaluation"]
    G --> B
```

## In Practice

**Treat every new modality as a pipeline, not a model feature.** A multimodal model call is one stage in a chain that includes extraction, normalization, chunking, grounding, and verification. When you draw the pipeline first, you can see which stages a modality adds and which existing stages it replaces; skipping this step means discovering the real architecture incident by incident.

**Know when a modality changes the architecture versus adding a stage.** Documents and recorded audio usually add a stage before existing text logic and leave the core pattern intact. Live voice changes the architecture outright: turn-taking, barge-in, and streaming become first-class state machines around the model call, and the response contract shifts from text blocks to speech segments.

**Ground answers in source regions, not just source files.** For documents and images, keep page and bounding-box metadata through extraction and require the model to cite the region each claim comes from; verify citations programmatically where possible. An answer that cannot point to a region is an unverified answer, and unverified answers on document content are how confident wrong clauses reach users.

**Engineer turn-taking before you engineer the voice model's personality.** Endpointing decides when the user is done speaking; barge-in decides when they can interrupt; both matter more to perceived quality than the voice timbre. Test overlapping speech, long pauses mid-thought, and background noise explicitly, because these break naive voice activity detection and produce the interruptions users remember.

**Budget latency per modality and communicate it in the modality's own currency.** Users accept a parsing phase for uploads if progress is visible, accept minutes for call transcription, but tolerate almost nothing in live voice. Stream what can be streamed per stage, and design the waiting experience (progress, partial transcripts, typing indicators) as deliberately as the answer itself; see [[career-path/18_Applied_AI_Engineer/05_Model_and_Inference_Operations/04_Latency_Engineering|Latency Engineering]] for the general discipline.

**Change the evaluation suite the moment you change the modality.** Transcription error rate, visual grounding accuracy, and audio synthesis quality are distinct failure classes from answer faithfulness, and each needs its own metric and its own slice in the suite. A single end-to-end "answer correct" score hides the difference between a model that reasoned wrong and a pipeline that heard wrong, which are fixed in completely different places.

**Treat voice and face data as biometric data by default.** Record consent before capture, define retention limits, restrict who can replay recordings, and redact where the use case allows. The compliance and trust cost of a voice corpus is ongoing, so collect the minimum audio the product needs and prefer derived artifacts like transcripts where the raw recording is not required; coordinate with [[career-path/18_Applied_AI_Engineer/04_AI_Security_and_Guardrails/00_overview|AI Security and Guardrails]] on storage and access rules.

## Practical Exercise

Design the pipeline for a document-and-voice assistant that answers questions about uploaded contracts, both by text and by phone:
1. List the pipeline stages for the document path and the live voice path side by side, and mark which stages each modality adds or replaces
2. Define extraction choices for scanned contracts: OCR vs text layer, table handling, and what page/region metadata is preserved
3. Write the grounding contract: what the model must cite, and what the verifier checks automatically before an answer is shown
4. Specify the voice turn-taking state machine: voice activity detection, endpointing threshold, barge-in behavior, and maximum silence before the assistant responds
5. Assign a latency budget to each stage for both modalities, and decide what streams and what shows progress instead
6. Define three eval metrics per modality (extraction, grounding, transcription, turn-taking) and where each runs in the suite
7. Write the consent, retention, and access policy for recorded calls in three sentences

## Practical Applications

- [ ] Pipeline diagram exists per modality, with stages and ownership marked
- [ ] Extraction preserves page/region metadata end to end
- [ ] Answers over documents and images carry verifiable source-region citations
- [ ] Voice pipeline has explicit endpointing, barge-in, and silence-timeout behavior
- [ ] Latency budgets are set per modality and per stage, with a defined streaming or progress behavior
- [ ] Eval suite has separate slices for extraction accuracy, grounding, transcription error, and turn-taking
- [ ] Voice and biometric data has documented consent, retention, and access rules

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Pasting the whole PDF into the context window | Layout and table structure silently degrade; costs grow with document size | Extract layout-aware chunks, keep region metadata, retrieve per question |
| Trusting OCR output as ground truth | Scanned tables and small print produce confident garbage the model builds on | Score extraction quality separately; flag low-confidence regions for review |
| Treating voice as text chat with audio on both ends | Turn-taking failures dominate the experience regardless of answer quality | Engineer endpointing and barge-in as first-class state machines |
| One end-to-end score for the whole multimodal system | Perception errors and reasoning errors are indistinguishable in the number | Score each stage's failure class in its own eval slice |
| Ignoring biometric privacy until legal asks | Voice corpora are hard to un-collect; retroactive consent is rarely possible | Consent, retention, and access rules before the first recording |
| Waiting for full parse or full transcription before showing anything | Perceived latency balloons even when the model call is fast | Stream per stage; show progress for what cannot stream |

## Knowledge Connections

- [[01_Retrieval_Augmented_Generation]]: document chunking and grounding feed directly on retrieval discipline
- [[06_Pattern_Selection_and_Fallback_Design]]: deciding when a modality warrants a new pattern vs a fallback path
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/02_Offline_Evaluation_Suites|Offline Evaluation Suites]]: adding modality-specific slices to the suite
- [[career-path/18_Applied_AI_Engineer/05_Model_and_Inference_Operations/04_Latency_Engineering|Latency Engineering]]: streaming and perceived-latency mechanics
- [[career-path/18_Applied_AI_Engineer/04_AI_Security_and_Guardrails/00_overview|AI Security and Guardrails]]: biometric data handling and consent

## Sources

- AI Engineering (Chip Huyen)
- Hands-On Large Language Models (Jay Alammar and Maarten Grootendorst)
- Speech and Language Processing (Daniel Jurafsky and James H. Martin)
- Designing Machine Learning Systems (Chip Huyen)
