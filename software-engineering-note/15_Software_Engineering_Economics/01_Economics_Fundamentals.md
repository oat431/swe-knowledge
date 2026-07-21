---
tags:
  - software-engineering-economics
  - cash-flow
  - time-value-of-money
  - npv
  - irr
  - business-models
source: "SWEBOK v4 Chapter 15 — Software Engineering Economics Fundamentals"
created: 2026-07-21
---

# Economics Fundamentals — Cash Flow, Value, and Business Models

> Software Engineering Economics is the science of choice — how to invest limited resources for maximum value. Every technical decision has an economic dimension.

## 1. Why Economics for Software Engineers?

Software has unique economic properties:
- **Front-loaded development costs** with near-zero marginal reproduction cost
- **High switching costs** — once a system is embedded, replacing it is exponentially expensive
- **Network effects** — the value of a system increases with the number of users
- **Compounding technical debt** — deferred quality work accumulates interest over time

Engineering economics provides the analytical framework to answer: *"Should we invest in this, or would the money produce a higher return elsewhere?"*

## 2. Cash Flow and the Time Value of Money

### Core Concept

Money has a time value: **$1 today is worth more than $1 tomorrow** because today's dollar can be invested to earn interest.

### Cash Flow Diagrams

A cash flow diagram shows inflows (↑, positive) and outflows (↓, negative) over time:

```
↑ Revenue: ------- $50K ------ $75K ------ $100K
Year:      0          1           2           3
↓ Cost:   -$100K     -$20K      -$20K      -$20K
```

### Interest and Compound Interest

| Type | Formula | Use |
|---|---|---|
| **Simple Interest** | F = P(1 + n × i) | Short-term, one period |
| **Compound Interest** | F = P(1 + i)ⁿ | Multi-period investments |

Where: P = present amount, F = future amount, i = interest rate per period, n = number of periods

> [!note] Software engineering uses compound interest for multi-year lifecycle cost analysis. Maintenance costs over 5-10 years require compound calculations.

### Key Financial Formulas

| What You Want | Formula | What It Means |
|---|---|---|
| **Present Worth (PW)** of future F | P = F / (1 + i)ⁿ | What a future amount is worth today |
| **Future Worth (FW)** of present P | F = P(1 + i)ⁿ | What today's money grows to |
| **Annuity** (uniform series) | P = A[(1 + i)ⁿ − 1] / [i(1 + i)ⁿ] | Present value of repeated payments |
| **Capital Recovery** (reverse annuity) | A = P[i(1 + i)ⁿ] / [(1 + i)ⁿ − 1] | Annual payment to recover initial investment |

### Decision Metrics

| Metric | Formula/Definition | Decision Rule |
|---|---|---|
| **Net Present Value (NPV)** | Sum of all discounted cash flows (inflows − outflows) | Accept if NPV > 0. Rank by highest NPV |
| **Internal Rate of Return (IRR)** | The discount rate where NPV = 0 | Accept if IRR > MARR |
| **Discounted Payback Period** | Time until cumulative discounted inflows ≥ initial investment | Shorter is better; compare against threshold |
| **Benefit-Cost Ratio (BCR)** | Discounted benefits ÷ discounted costs | Accept if BCR > 1.0 (nonprofit context) |

### Example: Build vs. Buy Decision

| Option | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | NPV (@10%) |
|---|---|---|---|---|---|---|
| **Build** | −$200K | −$50K | +$80K | +$100K | +$120K | **+$8.3K** |
| **Buy** | −$300K | +$0K | +$90K | +$110K | +$130K | **−$2.1K** |

At a 10% discount rate, building has a positive NPV → **choose build**.

## 3. Business Models and the "Do-Nothing" Alternative

### Business Model Types

| Model | Revenue Source | Example |
|---|---|---|
| **Product** | License sales per unit | Desktop software, mobile apps |
| **SaaS** | Subscription per month/user | Cloud services, enterprise tools |
| **Marketplace** | Transaction fee percentage | App stores, gig platforms |
| **Ad-Supported** | Advertising revenue | Social media, search engines |
| **Open Source + Services** | Support/consulting fees | Red Hat, MongoDB |

### The "Do-Nothing" Alternative

Every economic analysis must include **not building the system** as a baseline alternative. This forces the question: *Is this project better than doing nothing?*

The "do-nothing" baseline includes:
- Current system maintenance costs (if replacing an existing system)
- Opportunity cost of the team's time (what else could they build?)
- Business impact of not having the capability

> [!important] **Sunk costs do not count.** Money already spent cannot be recovered. Only future costs and benefits matter in decision-making. The question is always: "Going forward from today, which alternative provides the most value?"

## 4. Intangible Assets and Costs

Not everything can be quantified in dollars:

| Intangible | Valuation Approach |
|---|---|
| **User satisfaction** | Proxy metrics: NPS, retention rate, support ticket volume |
| **Technical debt** | Estimated rework cost, velocity impact |
| **Developer morale** | Turnover cost ($50K-150K per engineer), productivity data |
| **Reputation/brand** | Customer acquisition cost change, churn rate change |
| **Security posture** | Breach probability × breach cost |

The **SIPAC method** (Identify, Characterize, Subset, Assign Cost) provides a structured approach for including intangibles in economic analysis.

## 5. Inflation, Depreciation, and Taxes

| Factor | Effect on Analysis |
|---|---|
| **Inflation** | Future dollars are worth less — use real (inflation-adjusted) discount rates for long projects |
| **Depreciation** | Reduces taxable income — straight-line vs. accelerated methods affect after-tax cash flow |
| **Taxes** | Income tax reduces net benefit — after-tax analysis is required for for-profit decisions |

> **Rule of thumb:** For software projects under 2 years, inflation can usually be ignored. For multi-year programs and lifecycle cost analyses (10+ years), inflation must be factored in.

## Key Takeaways

1. **A dollar today is worth more than a dollar tomorrow** — discount future cash flows to present value
2. **NPV > 0 = create value** — primary decision criterion for for-profit organizations
3. **Always include "do-nothing"** as a baseline — forces justification of the investment
4. **Sunk costs are irrelevant** — only future costs and benefits matter
5. **Intangibles matter** — use proxy metrics when direct dollar quantification is impossible
6. **Software's near-zero marginal cost** is unique — economic models from manufacturing don't always apply

## Related

- [[Software Engineering Economics Overview]] — All economics topics
- [[02_Decision_Making]] — Decision-making under risk and uncertainty
- [[03_Cost_Analysis]] — Cost-benefit, break-even, TCO, AHP/ATAM
- [[04_Estimation_Concepts]] — Estimation fundamentals
