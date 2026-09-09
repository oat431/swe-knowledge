---
title: "AI ROI and Business Case"
note_type: capability-topic
capability_area: responsible-ai-and-governance
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - ai-roi
  - business-case
  - business-value
---

# AI ROI and Business Case

> Framing AI investment in business terms — quantified benefit, total cost, and risk-adjusted return — so decisions rest on evidence rather than enthusiasm.

## Why This Is a Senior Skill

Mid-level engineers build the thing and hope it proves useful. Senior engineers define success criteria before building, price the full total cost of ownership including governance, and kill projects that fail to clear the bar. The hardest and most valuable decision in an AI roadmap is killing a project that technically works but delivers no business value — and it is a decision junior engineers avoid making.

The senior challenge is translation: every technical choice must be expressible as a business outcome. If an engineer cannot explain a decision in business terms, they have not finished thinking it through.

## Core Frameworks

### The ROI Translation Formula

Technical Claim -> Business Metric -> Why It Matters -> Revenue/Cost/Risk Impact

| Technical Claim | Business Metric | Impact |
|-----------------|-----------------|--------|
| RAG grounded in verified documents | Hallucination rate under 2% on audited samples | Fewer wrong answers, fewer complaints and refunds |
| Provider-neutral model configuration | Swap models without re-platforming | Cost flexibility; no vendor lock-in |
| Guardrails deployed before launch | Zero injection incidents in first 90 days | Avoids breach cost and reputational damage |
| Eval gate in CI | No silent quality regressions | Release velocity without quality risk |

### Benefit Taxonomy

| Type | Example | How To Measure |
|------|---------|----------------|
| Automation | Chat deflection, document processing | Hours saved x loaded cost; deflection rate |
| Optimization | Pricing, routing, targeting | Incremental revenue vs baseline (A/B) |
| Augmentation | Drafting, decision support | Throughput per employee; output quality scores |
| New products | AI feature as a differentiator | New revenue; retention lift; churn reduction |

### Total Cost of Ownership for AI

| Category | One-Time | Recurring |
|----------|----------|-----------|
| Model / API | — | Per-token or per-seat fees |
| Infrastructure | Vector DB setup, pipelines | Hosting; storage growth |
| Engineering | Initial build | Maintenance, prompt upkeep, eval updates |
| Governance and compliance | DPIA, board setup | Audits, reviews, model-card maintenance |
| Vendor and data | Contracts, DPA negotiation | License renewals, data licensing |

### Risk-Adjusted ROI

Expected ROI = (Probability of Success x Projected Value) - (Probability of Failure x Cost)

| Project Risk | Success Probability Band |
|--------------|--------------------------|
| High (novel tech, no data, uncertain market) | 10-30% |
| Medium (proven tech, some data) | 50-70% |
| Low (established pattern, abundant data) | 80-95% |

### Impact x Feasibility Matrix

| Quadrant | Action |
|----------|--------|
| High impact, high feasibility | Do now |
| High impact, low feasibility | Investigate: build data, hire, or partner |
| Low impact, high feasibility | Quick win: build for momentum |
| Low impact, low feasibility | Deprioritize |

Scale-or-kill: metrics met or exceeded -> scale; partially met -> iterate and re-measure; clearly missed -> kill and document the learnings.

## In Practice

**Define success criteria before writing code.** "How will we know this worked?" must be answerable in numbers before the first prompt is written — deflection rate, accuracy on audited samples, CSAT, cost per reply. If the team cannot state measurable success criteria, the project has no business case: engineering effort without a definition of done is a bet without odds.

**Price the full TCO, including governance.** Teams that price only API calls understate cost by an order of magnitude: eval maintenance, prompt upkeep, compliance reviews, model-card updates, incident response. The governance and compliance line from [[04_Regulation_and_Compliance]] is part of the case from day one, not a surprise expense discovered later.

**Apply risk-adjusted ROI to novel projects.** Most "AI will save us X" decks assume a 100% success probability. Show both numbers — the raw projected value and the risk-adjusted expected ROI — so leadership sees what is being bet, not just what could be won. This is the single most effective honesty device in AI business casing.

**Kill decisions are senior wins.** A technically working AI feature that fails its success criteria is a cost, not an asset. Killing it, documenting the learnings, and reallocating the engineers is the highest-value decision on the roadmap. Defending the kill against sunk-cost and enthusiasm bias is part of the senior job.

**Anchor every technical decision to a business metric.** "We use pgvector" is trivia; "semantic search lifts product-search recall to 95%, cutting support tickets" is a business case. Practice the translation formula until it is automatic — it is the most transferable senior skill in this area and the one most visible to leadership.

**Right-size the solution.** Choosing keyword search over a vector database for a 30-item catalog is a feature, not a limitation. Every layer of sophistication must earn its place in the TCO. Over-engineering is a ROI failure exactly as much as under-delivering — both waste money the business case was supposed to protect.

## Practical Exercise

Build a one-page business case for one candidate AI feature:
1. Classify the feature in the benefit taxonomy (automation, optimization, augmentation, or new product)
2. Quantify the benefit bottom-up, stating every assumption explicitly
3. Build the TCO: one-time and recurring, including governance and compliance
4. Assign a success-probability band and compute the risk-adjusted expected ROI
5. Score impact x feasibility and pick the quadrant action
6. Define 3-5 success criteria with targets and a measurement plan (per [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]])
7. Write the memo: claim, metric, TCO, risk-adjusted ROI, and a recommendation — do, investigate, or kill

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]] — the primary framework source for this topic
- [[career-path/14_Product_Manager/00_overview|Product Manager]] — the partner who owns the business metric side of the case
- [[04_Regulation_and_Compliance]] — compliance costs as a TCO line item
- [[06_Governance_Operating_Model]] — the review board that consumes these business cases
- [[01_AI_Risk_Management]] — risk-adjusted thinking applied to the portfolio, not just the register
