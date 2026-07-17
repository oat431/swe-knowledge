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

*(No additional notes yet — only this overview)*

## What's Missing

- Economics Fundamentals (cash flow, time-value of money, business models)
- Engineering Decision-Making (7-step process, decision trees, sensitivity analysis)
- For-Profit Decision-Making (MARR, replacement decisions, depreciation)
- Nonprofit Decision-Making (benefit-cost analysis, cost-effectiveness)
- Present Economy (break-even, optimization)
- Multiple-Attribute Decision-Making (AHP, ATAM, compensatory/non-compensatory)
- Estimation (Delphi, Planning Poker, parametric models, COCOMO)
- Practical Considerations (business cases, TCO)

## Relationship to Other KAs

- **[[Software Engineering Management Overview|Software Engineering Management]]** — Estimation, budgeting, and resource allocation are management activities requiring economic analysis.
- **[[Software Quality Overview|Software Quality]]** — Quality economics quantify the financial impact of quality investments; rework is the largest cost driver.
- **[[Software Maintenance Overview|Software Maintenance]]** — Maintenance is the largest lifecycle cost; build-vs-replace and technical debt decisions require economic analysis.
- **[[Software Requirements Overview|Software Requirements]]** — Requirements scope drives cost; prioritization techniques are economic decisions.
- **[[Software Architecture Overview|Software Architecture]]** — ATAM for architecture tradeoff analysis; wrong architecture multiplies costs for years.
- **[[Professionalism of Software Engineering Overview|Professional Practice]]** — Code of Ethics §3.09 on estimation obligations; economic impact of professional decisions.
