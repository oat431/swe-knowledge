---
tags:
  - overview
  - swebok
  - software-engineering-economics
  - estimation
  - cost-benefit-analysis
  - decision-making
---

# Software Engineering Economics — Overview

> **Source:** SWEBOK v4 Chapter 15
> **Purpose:** Apply engineering economics to software — the science of choosing how to invest limited resources for maximum value.

## What Is This?

Software Engineering Economics is fundamentally the science of choice, not the science of money. It asks: "Would this investment produce a higher return elsewhere?" Every technical decision — build vs. buy, scope inclusion, architecture selection, refactoring vs. living with technical debt — has economic implications. Engineers who understand economics make better trade-off decisions and communicate business impact to stakeholders effectively.

The discipline covers ROI analysis, estimation techniques, decision-making processes (for-profit, nonprofit, and present-economy contexts), lifecycle costing, and the alignment of technical decisions with business goals. A key insight: the Total Cost of Ownership (TCO) extends far beyond development — the operate-maintain-retire phases consume more resources than initial development. Rework is the single largest resource consumer in most organizations; reducing it via proactive quality actions is the most effective productivity improvement.

Software has unique economic properties: front-loaded development costs with near-zero marginal reproduction cost, high switching costs, network effects, and compounding technical debt. Value-based software engineering is non-negotiable for long-term sustainability — neutral or negative value is not sustainable. All estimates carry inherent uncertainty; they need only be good enough to lead to the right decision.

## Knowledge Areas

### Software Engineering Economics Fundamentals
- Cash flow streams, time-value of money, and equivalence for comparing proposals
- Bases for comparison: present worth, future worth, annual equivalent, IRR, discounted payback
- Business models, intangible assets, and the "do-nothing" alternative

### The Engineering Decision-Making Process
- Seven-step iterative process: understand problem → identify solutions → define criteria → evaluate → select → monitor → close loop
- Sensitivity analysis, decision trees, Monte Carlo analysis for decisions under risk
- Laplace, Maximin, Maximax, Hurwicz, Minimax Regret for decisions under uncertainty

### For-Profit Decision-Making
- MARR (Minimum Acceptable Rate of Return) as opportunity-cost threshold
- Economic life, replacement/retirement decisions, sunk cost, salvage value
- Inflation, depreciation, and income tax considerations

### Nonprofit Decision-Making
- Benefit-cost analysis: reject proposals with ratio below 1.0
- Cost-effectiveness: fixed-cost (maximize benefit within budget) and fixed-effectiveness (minimize cost for fixed goal)

### Present Economy Decision-Making
- Break-even analysis: finding where cost functions of alternatives are equal
- Optimization analysis: finding minimum total cost (e.g., space-time trade-offs)

### Multiple-Attribute Decision-Making
- Compensatory techniques: nondimensional scaling, additive weighting, AHP, ATAM, Gilb's Impact Estimation
- Non-compensatory techniques: dominance, satisficing, lexicography — no trade-offs allowed

### Estimation
- Five techniques in order of increasing accuracy: expert judgment, analogy, decomposition, parametric/statistical, multiple estimates
- Wide Band Delphi, Planning Poker for structured expert judgment
- Close the loop: compare estimates to actuals to improve future accuracy

### Practical Considerations
- Business case development, multi-currency analysis, systems thinking
- SIPAC method for identifying and characterizing intangible assets
- TCO: acquire, activate, and keep running — hidden costs dominate

## My Notes

### SWEBOK Economics Foundations
- [[01_Economics_Fundamentals]] — Cash flow, time-value of money, NPV, IRR, business models
- [[02_Decision_Making]] — 7-step process, decision trees, Monte Carlo, uncertainty methods
- [[03_Cost_Analysis_and_Tradeoffs]] — MARR, benefit-cost, break-even, TCO, AHP, ATAM

### Software Estimation (McConnell)
- [[04_Estimation_Concepts]] — Estimates vs. targets, Cone of Uncertainty, error sources
- [[05_Estimation_Techniques]] — Count/compute/judge, decomposition, analogy, proxy-based, Delphi
- [[06_Estimation_Challenges]] — Size/effort/schedule/cost estimation, presentation, sanity checks

## Relationship to Other KAs

- **[[Software Engineering Management Overview|Software Engineering Management]]** — Estimation, budgeting, and resource allocation are management activities requiring economic analysis.
- **[[Software Quality Overview|Software Quality]]** — Quality economics quantify the financial impact of quality investments; rework is the largest cost driver.
- **[[Software Maintenance Overview|Software Maintenance]]** — Maintenance is the largest lifecycle cost; build-vs-replace and technical debt decisions require economic analysis.
- **[[Software Requirements Overview|Software Requirements]]** — Requirements scope drives cost; prioritization techniques are economic decisions.
- **[[Software Architecture Overview|Software Architecture]]** — ATAM for architecture tradeoff analysis; wrong architecture multiplies costs for years.
- **[[Professionalism of Software Engineering Overview|Professional Practice]]** — Code of Ethics §3.09 on estimation obligations; economic impact of professional decisions.

---

## SWEBOK v4 Coverage Map

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 15 | **Last analyzed:** 2026-07-21 | **Coverage:** ~85% (updated)

| # | SWEBOK Topic | Status | Vault File(s) | Notes |
|---|---|---|---|---|
| 1 | Economics Fundamentals | ✅ | `01_Economics_Fundamentals.md` (7 KB) | Cash flow, time-value, PW/FW/annuity, IRR, business models |
| 2 | Decision-Making Process | ✅ | `02_Decision_Making.md` (8 KB) | 7-step process, certainty/risk/uncertainty, decision trees |
| 3 | For-Profit Decision-Making | ✅ | `03_Cost_Analysis_and_Tradeoffs.md` (9 KB) | MARR, economic life, replacement, depreciation |
| 4 | Nonprofit Decision-Making | ✅ | `03` | Benefit-cost ratio, cost-effectiveness |
| 5 | Present Economy | ✅ | `03` | Break-even analysis covered |
| 6 | Multiple-Attribute Decision-Making | ✅ | `03` | AHP, ATAM, additive weighting |
| 7 | Intangible Assets / SIPAC | ❌ | — | SIPAC method and 11 Generic Intangible Assets not covered |
| 8 | Estimation | ✅ | `04` (19 KB), `05` (26 KB), `06` (25 KB) | Excellent depth via McConnell |
| 9 | Practical Considerations | ⚠️ | Overview | Business case mentioned; multi-currency, systems thinking only in overview |
| 10 | Related Concepts | ⚠️ | `01`, Overview | TCO mentioned; accounting/finance not deeply covered |

### Gaps to Fill

| Priority | Gap | SWEBOK Topic | What's Missing |
|----------|-----|-------------|----------------|
| 🔴 High | SIPAC / Intangible Assets | Intangible Assets | 7-step method, 11 Generic Intangible Assets taxonomy, asset states |
| 🟡 Medium | Multi-Currency & Systems Thinking | Practical Considerations | Cross-border exchange rate analysis, holistic client ecosystems |
| 🟡 Medium | Accounting/Finance Fundamentals | Related Concepts | Accounting, costing, finance, controlling, project/program/portfolio |
| 🟢 Low | Inflation & Income Tax | For-Profit | Advanced considerations in for-profit decision-making |
