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

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 15 — Software Engineering Economics
> **Purpose:** Apply economic reasoning to software engineering decisions — from project investment to build-vs-buy to lifecycle cost management.

## What Is This?

Software Engineering Economics applies economic principles and financial analysis to the decisions that software engineers, managers, and organizations face every day. Should we build this feature or buy it? Is it worth investing in refactoring now or accepting higher maintenance costs later? How much should we spend on testing? When does it make sense to rewrite a legacy system? These are fundamentally economic questions that require quantitative analysis, not just intuition.

Software is peculiar from an economic standpoint: its development costs are front-loaded (high initial investment), while its marginal cost of reproduction approaches zero. This creates unique dynamics — network effects, lock-in, platform economics — that traditional manufacturing economics don't capture. Software also has high switching costs, long lifetimes, and compounding technical debt, making lifecycle cost analysis essential.

SWEBOK v4's economics chapter bridges engineering and business. Engineers who understand economics make better trade-off decisions: they can quantify the value of quality investments, estimate project costs with appropriate uncertainty ranges, and communicate business impact to stakeholders. Managers who understand engineering economics avoid the false economy of cutting testing budgets, skipping documentation, or accumulating unmanaged technical debt.

## The 6 Topic Areas

### 1. [[Engineering Economics Basics]]
- Time value of money: present value, future value, discount rates
- Cash flow analysis: investment costs, operating costs, revenue streams
- Financial statements basics: income statement, balance sheet, cash flow statement
- Depreciation and amortization of software assets
- Inflation, opportunity cost, and sunk cost in software decisions
- **Book:** *Software Engineering Economics* — Barry Boehm

### 2. [[ROI and Cost-Benefit Analysis]]
- Return on Investment (ROI): net benefits / investment cost
- Cost-Benefit Analysis (CBA): identifying, quantifying, and comparing all costs and benefits
- Intangible benefits: developer productivity, customer satisfaction, risk reduction
- Break-even analysis: when does the investment pay for itself?
- Sensitivity analysis: how do results change with different assumptions?
- **Book:** *Software Estimation: Demystifying the Black Art* — Steve McConnell

### 3. [[Estimation Techniques]]
- Algorithmic models: COCOMO II, Function Point Analysis, Use Case Points
- Expert-based: Wideband Delphi, planning poker, three-point estimation
- Analogy-based: comparing to similar past projects
- Parametric estimation: regression models calibrated to organizational data
- Estimation accuracy: the cone of uncertainty, ranges vs. point estimates
- **Book:** *Software Engineering Economics* — Barry Boehm

### 4. [[For-Profit and Nonprofit Decision Making]]
- Revenue models: licensing, SaaS subscriptions, freemium, advertising, support contracts
- Nonprofit and government contexts: benefit-cost ratio, social return on investment
- Make vs. buy vs. open-source decisions
- Vendor selection and contract evaluation
- Strategic alignment: does this investment support organizational goals?
- **Book:** *Balancing Agility and Discipline* — Boehm & Turner

### 5. [[Total Cost of Ownership]]
- TCO components: acquisition, deployment, operation, maintenance, retirement
- Hidden costs: training, productivity loss during transition, integration, customization
- Software lifecycle cost models: development typically 20-30%, maintenance 60-80%
- Technical debt as deferred cost: interest payments in the form of slower development
- Cloud economics: CapEx vs. OpEx, reserved vs. on-demand, cost optimization
- **Book:** *The Economics of Software Quality* — Capers Jones et al.

### 6. [[Build vs. Buy]]
- Decision framework: strategic importance, available alternatives, total cost comparison
- Build advantages: customization, control, competitive differentiation
- Buy advantages: faster time to market, lower initial cost, vendor support
- Open-source as a third option: licensing costs, support models, contribution requirements
- Risk analysis: vendor lock-in, dependency risk, obsolescence
- Contract economics: fixed price vs. time-and-materials vs. outcome-based
- **Book:** *Software Estimation: Demystifying the Black Art* — Steve McConnell

## Recommended Books (Priority Order)

| #   | Book                                                     | Author(s)                    | Pages |     Priority     |
| --- | -------------------------------------------------------- | ---------------------------- | :---: | :--------------: |
| 1   | *Software Estimation: Demystifying the Black Art* (2006) | Steve McConnell              |  368  |   🔴 Essential   |
| 2   | *Software Engineering Economics* (1981)                  | Barry Boehm                  |  767  |   🔴 Essential   |
| 3   | *The Economics of Software Quality* (2011)               | Capers Jones et al.          |  558  |  🟡 Recommended  |
| 4   | *Balancing Agility and Discipline* (2004)                | Barry Boehm & Richard Turner |  304  | 🟢 Supplementary |

## Relationship to Other KAs

- **[[Software Engineering Management Overview|Software Engineering Management]]** — Estimation, budgeting, and resource allocation are management activities that require economic analysis. Project decisions have financial consequences.
- **[[Software Quality Overview|Software Quality]]** — Quality economics (cost of quality, defect cost models) quantify the financial impact of quality investments. Prevention vs. appraisal vs. failure cost trade-offs.
- **[[Software Maintenance Overview|Software Maintenance]]** — Maintenance is the largest lifecycle cost. Build-vs-replace decisions, technical debt management, and legacy system migration all require economic analysis.
- **[[Software Requirements Overview|Software Requirements]]** — Requirements scope drives cost. Prioritization techniques (cost-value analysis, ROI ranking) help decide what to build first.
- **[[Software Architecture Overview|Software Architecture]]** — Architecture decisions have long-term economic consequences. Choosing the wrong architecture pattern can multiply costs for years.
- **[[Software Security Overview|Software Security]]** — Security investment decisions require risk-based economic analysis: probability × impact vs. cost of controls.

## Related
- [[SWEBOK v4 - Overview]]
- [[09_Software_Engineering_Management/Software Engineering Management Overview|SE Management Overview]]
- [[07_Software_Maintenance/Software Maintenance Overview|Maintenance Overview]]
