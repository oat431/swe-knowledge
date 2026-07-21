---
tags: [estimation, techniques, delphi, planning-poker, story-points, software-engineering-economics]
source: "McConnell, Software Estimation: Demystifying the Black Art, Part II, Chapters 6–17"
---

# Estimation Techniques

> McConnell Part II — Fundamental Estimation Techniques (Chapters 6–17)

---

## 6. Introduction to Estimation Techniques

### Considerations in Choosing Estimation Techniques

| Factor | Description |
|---|---|
| **What’s Being Estimated** | Size (scope of technical work), Effort, Schedule, or Features (how many fit within constraints) |
| **Project Size** | Small (≤5 staff), Medium (5–25), Large (≥25) |
| **Development Style** | Sequential (RUP, staged delivery, Scrum sprints) vs. Iterative (XP, evolutionary prototyping, multi-sprint Scrum) |
| **Development Stage** | Early (concept → requirements), Middle (planning → early construction), Late (mid-construction → release) |
| **Accuracy Possible** | Function of technique, context, and timing within the Cone of Uncertainty |

> **Tip #29** When choosing techniques, consider what you want to estimate, size of project, development stage, development style, and accuracy needed.

---

## 7. Count, Compute, Judge

**The most fundamental estimation framework.** Three-tiered hierarchy from most accurate to least:

| Tier | Approach | Accuracy |
|---|---|---|
| **1. Count** | Count the answer directly (if possible) | Highest |
| **2. Compute** | Count something else, then convert using calibration data | High |
| **3. Judge** | Expert judgment alone | Lowest |

> **Tip #30** Count if at all possible. Compute when you can't count. Use judgment alone only as a last resort.

### What to Count

| Project Phase | Countable Items |
|---|---|
| **Early** | Marketing requirements, features, use cases, stories |
| **Middle** | Engineering requirements, Function Points, change requests, Web pages, reports, dialogs, screens, database tables |
| **Late** | Code written, defects reported, classes, tasks, test cases |

### Criteria for Selecting What to Count

1. **Highly correlated with software size**
2. **Available sooner rather than later** (Object Points ~ half the effort of Function Points)
3. **Produces statistically meaningful averages** (≥20 items)
4. **Consistent assumptions** between historical and current counts
5. **Minimal effort** to count

### Computation Examples

| Quantity Counted | Historical Data Needed |
|---|---|
| Marketing requirements | Avg hours/requirement (dev, test, docs) |
| Features | Avg hours/feature |
| Stories | Avg hours/story, stories/calendar-month |
| Function Points | Avg effort/FP, LOC/FP |
| Defects found | Avg hours/defect (fix, regression test) |
| Web pages | Avg hours/page (UI work) |

> **Tip #33** Don't discount simple, coarse models: avg effort/defect, avg effort/Web page, avg effort/story, avg effort/use case.

> **Tip #34** Avoid using expert judgment to tweak a computed estimate — it usually degrades accuracy.

---

## 8. Calibration and Historical Data

### Three Kinds of Calibration Data

| Data Source | Accuracy | Use Case |
|---|---|---|
| **Industry Data** | Low–Medium | Temporary backup when no organizational data exists |
| **Organizational Historical Data** | Medium–High | Accounts for organizational influences automatically |
| **Project-Specific Data** | High | From current project; most accurate once available |

### Why Historical Data Matters

- **Accounts for organizational influences** — productivity is an organizational attribute that can't easily be varied project to project (Putnam's "Yesterday's Weather")
- **Avoids subjectivity and unfounded optimism** — prevents "this time will be different" thinking
- **Reduces estimation politics** — sidesteps debates about whether "our programmers are below average"
- **Narrows estimate variability** — standard deviation drops from ~100% (industry) to ~25% (historical)

> **Tip #41** Use project/historical data rather than industry-average data whenever possible.

### Data to Collect (Start Small)

1. **Size** (LOC, Function Points, stories, etc.)
2. **Effort** (staff months/hours)
3. **Time** (calendar months)
4. **Defects** (classified by severity)

> **Tip #37** Start small, understand what you're collecting, and collect consistently.

### Key Definitional Issues

- **LOC**: Count all code or only released? Reused code? Blank lines/comments? Data declarations?
- **Effort**: Standard 8-hour days or actual? Overtime? Holidays/vacation? What activities included?
- **Calendar Time**: When does project start/end? (Fewer than 1% of projects have clearly defined start)
- **Defects**: All change requests or only defects? Duplicate reports? Detected by developers or testers?

### Project Data Refinement

> **Tip #40** Use data from your current project to create highly accurate estimates for the remainder. Switch from organizational/industry data to project data as soon as possible (iterative projects get there faster).

---

## 9. Individual Expert Judgment

Most common estimation approach (72–86% of estimates use it), but also the most hazardous.

### Who Creates Estimates

- **Task-level**: People who will actually do the work create the most accurate estimates
- **Early/Cone of Uncertainty**: Expert estimator or most experienced staff available

> **Tip #43** Have the people who will do the work create the task-level estimates.

### Granularity

Decompose tasks to ≤ 2 days of effort. Larger tasks hide unexpected work.

### Ranges and PERT Formula

**Single-point estimates tend to be Best Case estimates.** Use the PERT formula:

```
Expected Case = [BestCase + (4 × MostLikelyCase) + WorstCase] / 6
```

For downward-biased Most Likely estimates (optimism), use:

```
Expected Case = [BestCase + (3 × MostLikelyCase) + (2 × WorstCase)] / 6
```

### Checklist for Individual Estimates

1. Is what's being estimated clearly defined?
2. Does the estimate include all kinds of work needed?
3. Does it include all functionality areas?
4. Is it broken down into enough detail?
5. Did you use documented facts from past work (not just memory)?
6. Is the estimate approved by the person doing the work?
7. Is the productivity assumption consistent with past achievements?
8. Does it include Best, Worst, and Most Likely cases?
9. Is the Worst Case truly the worst case?
10. Is the Expected Case computed appropriately?
11. Are assumptions documented?
12. Has the situation changed since the estimate was prepared?

### Compare Estimates to Actuals

Use **Magnitude of Relative Error (MRE)**:

```
MRE = |(ActualResult − EstimatedResult) / ActualResult|
```

Set up a feedback loop — timely comparison of actual vs. estimated improves accuracy over time.

> **Tip #46** Compare actual performance to estimated performance to improve individual estimates over time.

---

## 10. Decomposition and Recomposition

Also known as "bottom-up," "micro estimation," "module build up."

### The Law of Large Numbers

When you decompose one large estimate into many small ones, **errors on the high side and low side cancel each other out** to some degree. A single aggregate estimate has all its error on one side.

> **Tip #47** Decompose large estimates into small pieces to take advantage of the Law of Large Numbers.

**Minimum**: 5–10 items before you get meaningful benefit. More is better.

### Work Breakdown Structure (WBS)

A generic, activity-based WBS prevents forgotten tasks. Categories include:

| Category | Create/Do | Plan | Manage | Review | Rework | Report Defects |
|---|---|---|---|---|---|---|
| General management | ● | ● | ● | ● | | |
| Planning | ● | ● | ● | ● | | |
| Requirements work | ● | ● | ● | ● | ● | ● |
| Architecture work | ● | ● | ● | ● | ● | ● |
| Detailed designing | ● | ● | ● | ● | ● | ● |
| Coding | ● | ● | ● | ● | ● | ● |
| Integration | ● | ● | ● | ● | ● | ● |
| Testing (manual/auto) | ● | ● | ● | ● | ● | ● |
| Software release | ● | ● | ● | ● | ● | ● |
| Documentation | ● | ● | ● | ● | ● | ● |
| Change management | ● | ● | ● | ● | ● | ● |

> **Tip #48** Use a generic WBS to avoid omitting common activities.

### Hazards of Summing Individual Estimates

Single-point estimates often represent Best Case (~25% likely). The probability of delivering ALL tasks on time:

- 1 task at 25% → 25%
- 2 tasks → 6.25% (1/16)
- 10 tasks → ~0.000095% (1 in 1,000,000)

**Solution**: Use Expected Case estimates (PERT), not simple sums of Best Cases.

### Computing Aggregate Best/Worst Cases

**For ≤10 tasks (simple formula):**

```
StandardDeviation = (SumOfWorstCases − SumOfBestCases) / 6
```

**For >10 tasks (complex formula):**
1. Compute individual standard deviation per task: `(Worst − Best) / Divisor`
2. Square each to get variance
3. Sum variances
4. Take square root for aggregate standard deviation

**The divisor matters**: Don't blindly divide by 6. Use a divisor based on how often your ranges capture actuals:

| % Outcomes in Range | Divisor |
|---|---|
| 30% | 0.77 |
| 50% | 1.4 |
| 70% | 2.1 |
| 80% | 2.6 |

**Percentage-Confident Estimates:**

| Confidence | Calculation |
|---|---|
| 50% | Expected Case |
| 75% | Expected + (0.67 × StdDev) |
| 90% | Expected + (1.28 × StdDev) |

> **Tip #52** Focus on making Expected Case estimates accurate. If individual estimates are accurate, aggregation won't create problems.

---

## 11. Estimation by Analogy

Compare a new project to a similar past project, decomposed piece by piece.

### The Five-Step Process

1. **Get detailed size/effort/cost results** for a similar previous project (decomposed by feature area or WBS)
2. **Compare the new project piece-by-piece** to the old project
3. **Build up the estimate** for the new project's size as a percentage of the old
4. **Create an effort estimate** based on the size ratio
5. **Check for consistent assumptions** (size differences < 3×, same technology, similar team/software type)

### Why Decomposition Matters for Analogy

Without decomposition:
- Estimators' results had **standard deviation of 46%**
- Range: 30 to 144 staff months (average 53)

With decomposition:
- **Standard deviation dropped to 7%**

### Handling Uncertainty

- **Don't bias the estimate** — address uncertainty by expressing the estimate in uncertain terms
- **Carry ranges through computations** rather than single points
- Uncertainty in one area should NOT spread to the whole estimate (decomposition localizes it)

> **Tip #54** Address estimation uncertainty by expressing the estimate in uncertain terms, not by biasing it.

---

## 12. Proxy-Based Estimates

Techniques that estimate a proxy correlated with what you really want, then convert using historical data.

### Fuzzy Logic

Classify features as **Very Small / Small / Medium / Large / Very Large**. Use historical averages (LOC or effort per category) to compute totals.

| Feature Size | Example Avg LOC | Example Avg Staff Days |
|---|---|---|
| Very Small | 127 | 4.2 |
| Small | 253 | 8.4 |
| Medium | 500 | 17 |
| Large | 1,014 | 34 |
| Very Large | 1,998 | 67 |

- Differences between adjacent categories should be ≥ 2× (some recommend 4×)
- Needs ≥20 features for statistical validity
- **Whole-rolled-up number has greater validity than individual estimates** — don't use for specific feature sizes

### Standard Components

For architecturally similar systems. Compute average LOC per component type from historical data, then estimate counts.

**Components**: Dynamic/static Web pages, database tables, reports, business rules, dialogs, screens, etc.

**Percentile variation**: Instead of estimating counts directly, classify each component as Very Small / Small / Average / Large / Very Large relative to past projects.

> **Tip #56** Use standard components as low-effort technique for early-stage size estimates.

### Story Points

Originally from Extreme Programming. Process:

1. **Assign points** to all stories using a relative scale (Fibonacci: 1, 2, 3, 5, 8, 13 or powers of 2)
2. **Plan an iteration** and deliver some number of points
3. **After iteration**, compute **velocity**: story points per staff week, story points per calendar week
4. **Project remainder**: `Total story points ÷ velocity`

> **Tip #57** Use story points to obtain early estimates based on data from the same project.

**Caution**: Numeric scales (Fibonacci, powers of 2) imply proportionate relationships. If a 13-point story isn't really 13/3× the effort of a 3-point story, numeric operations on those numbers are misleading. The categories may function more like verbal labels than true numbers.

> **Tip #58** Ensure numeric categories in rating scales actually work like numbers, not like verbal categories.

### T-Shirt Sizing

Classify features on two axes:
- **Development Cost** (Small, Medium, Large, Extra Large)
- **Business Value** (Small, Medium, Large, Extra Large)

Create a **Net Business Value lookup table** to sort features into cost/benefit order. Enables early "definitely yes" / "definitely no" decisions.

> **Tip #59** Use t-shirt sizing to help non-technical stakeholders rule features in/out while in the wide part of the Cone of Uncertainty.

---

## 13. Expert Judgment in Groups

### Group Reviews

Three rules:
1. Each member estimates individually, then meet to compare
2. Discuss differences — don't just average
3. Arrive at consensus — no voting, must obtain buy-in

**Effectiveness**: Reduces average error from 55% (individual) to 30% (reviewed). 92% of group estimates more accurate than individual estimates.

- Use 3–5 experts with **different backgrounds/roles/techniques**

> **Tip #62** Use group reviews to improve estimation accuracy.

### Wideband Delphi

A structured, iterative group-estimation technique developed by Barry Boehm (extending Rand Corporation's Delphi method).

**Procedure:**

| Step | Action |
|---|---|
| 1 | Coordinator presents specification and estimation form |
| 2 | Estimators prepare initial estimates individually |
| 3 | Group meeting to discuss estimation issues |
| 4 | Estimators submit estimates **anonymously** |
| 5 | Coordinator summarizes estimates on iteration form |
| 6 | Group discusses variations |
| 7 | Anonymous vote on whether to accept average — if any "no," return to step 3 |
| 8 | Final estimate is single-point or range (single-point = expected case) |

**Key features:**
- **Anonymity** prevents dominant personalities from swaying results
- **Multiple rounds** with convergence visualization
- **Devil's advocate** assigned if agreement comes too easily
- Can be performed in person or electronically

**Effectiveness**: 
- Reduces estimation error by ~40% compared to simple averaging
- Improves results in ~2/3 of cases
- For worst initial estimates, improves 8/10 cases (avg 60% error reduction)
- **20% of groups' initial ranges don't include correct answer** — averaging can't help
- **1/3 of those groups move outside their initial range to get closer to correct answer** after Wideband Delphi

**When to use**: New business areas, new technologies, unfamiliar systems, projects drawing from diverse specialties. Most useful in the wide part of the Cone of Uncertainty. **Not appropriate for detailed task estimates** (too expensive in staff time).

> **Tip #63** Use Wideband Delphi for early-in-the-project estimates, unfamiliar systems, and when diverse disciplines are involved.

---

## 14. Software Estimation Tools

### What Tools Can Do That Manual Methods Can't

| Capability | Description |
|---|---|
| **Monte Carlo Simulation** | Simulate thousands of project outcomes accounting for variability in productivity, size, and staffing |
| **Probability Analysis** | Compute true percentage-confident estimates (e.g., 90% confident may require 6× nominal) |
| **Diseconomies of Scale** | Automatically account for nonlinear productivity at different sizes |
| **Creeping Requirements** | Built-in allowances for requirements growth |
| **What-If Analysis** | Instant recomputation of assumptions |
| **Objective Authority** | Tool as impartial third party for unrealistic expectations or tradeoff discussions |
| **Planning Integration** | Allocate effort across phases; export to project-planning tools |

### Calibration Data Needed

Minimal: Effort (staff months), Schedule (elapsed months), Size (LOC) from 1–3+ completed projects.

> Your own historical data from 3 projects usually produces more accurate estimates than a tool's generic industry database.

> **Tip #65** Don't treat estimation tool output as divine revelation — sanity-check tool outputs like any other estimate.

### Available Tools

| Tool | Basis |
|---|---|
| Construx Estimate (free) | Putnam model + Cocomo II |
| Cocomo II (free) | USC implementation |
| Costar | Full Cocomo II |
| SLIM-Estimate / Estimate Express | Putnam model (QSM) |
| KnowledgePLAN | Capers Jones / SPR |
| SEER-SEM | Galorath |
| Price-S | Suite of products |

---

## 15. Use of Multiple Approaches

> The most sophisticated commercial software producers use at least **three different estimation approaches** and look for convergence or spread.

### Principle

- **Convergence** (within ~5%) → you probably have a good estimate
- **Spread** → there are factors you've overlooked and need to understand better

### McConnell's Code Complete Example

| Estimate | Method | Result |
|---|---|---|
| #1 | Gut instinct (analogy to other books) | 250–300 pages |
| #2 | Expert judgment with decomposition (outline) | 802 pages |
| #3 | Historical data (pages per outline point) | 759 pages |

Estimates #2 and #3 converged within 5% — revealing that the gut-instinct estimate was wrong by 3×. Final result: 749 pages (1% from estimate #3).

### Best Practice

- Use **different kinds** of techniques (not variations of the same approach)
- Example combination: proxy-based + expert judgment + analogy + software estimation tool
- If multiple estimates agree and the business target disagrees, **trust the estimates**

> **Tip #66** Use multiple estimation techniques and look for convergence or spread among results.

> **Tip #68** If multiple estimates agree and the business target disagrees, trust the estimates.

---

## 16. Flow of Software Estimates on a Well-Estimated Project

### Individual Estimate Flow

**Poorly estimated projects**: Ad hoc process; inputs, process, and outputs all open to debate and downward pressure.

**Well-estimated projects**:

```
Technical scope ──┐
Priorities ───────┤
Constraints ──────┤──→ Standard Estimation Procedure ──→ Effort / Schedule / Cost / Features
Historical data ──┘
```

- Outputs are **not debated** — only inputs are adjusted and recomputed
- The only thing that ever needs "judgment-based estimation" is **size** — effort, schedule, cost, and features are all **computed** from size

```
Size → Effort → Schedule
              → Cost
              → Features
```

> **Tip #70** Focus on estimating size first. Then compute effort, schedule, cost, and features from the size estimate.

### Chronological Flow for an Entire Project

As the project progresses, switch from less accurate to more accurate techniques:

| Stage | Large Sequential | Small Sequential | Iterative |
|---|---|---|---|
| Early | Algorithms, tools, top-down, Wideband Delphi | Bottom-up from Day 1 | Algorithms, proxy-based |
| Middle | Counting + historical data | Bottom-up (own tasks) | Counting + project data (velocity) |
| Late | Bottom-up task-level, project-specific data | Bottom-up | Bottom-up, project data |

> **Tip #72** Change from less accurate to more accurate estimation approaches as you work through a project.

### Estimate Refinement

When you miss a milestone, the correct response is **option #3**: multiply the whole schedule by the magnitude of the slip.

- Projects **hardly ever make up lost time** — they tend to get further behind (van Genuchten 1991)

> **Tip #74** When reestimating after a missed deadline, base the new estimate on actual progress, not planned progress.

### Presenting Reestimates

**Wrong way** (single-point): 10 → 10 → 13 → 14 → 16 → 17 (appears to be constant slipping)

**Right way** (ranges): 3–40 → 5–20 → 9–20 → 12–18 → 15–18 → 17 (appears to stay within expectations, ranges tighten)

- **Ranges prevent anchoring** — initial single-point estimates contaminate future estimates
- **Communicate the reestimation plan in advance**

> **Tip #76** Communicate your plan to reestimate to other project stakeholders in advance.

### Characteristics of a Well-Estimated Project

- Single-point estimates may miss the mark, but **all estimation ranges include the eventual outcome**
- Ranges narrow systematically as the project progresses
- Poorly estimated projects: systematically underestimated with ranges too narrow

---

## 17. Standardized Estimation Procedures

A **well-defined process** adopted at the organizational level that provides guidance at the individual-project level.

### Usual Elements

1. **Emphasizes counting and computing** over judgment
2. **Calls for multiple estimation approaches** and comparison of results
3. **Communicates a plan to reestimate** at predefined points
4. **Defines how the approach changes** over the project lifecycle
5. **Contains a clear description of estimate inaccuracy**
6. **Defines when estimates can be used** for budgets and commitments
7. **Calls for archiving data** and reviewing procedure effectiveness

> Deviations from the procedure must be justified in writing and should be rare. The procedure is under formal change control — changed between projects, never "in flight."

> **Tip #77** Develop a Standardized Estimation Procedure at the organizational level; use it at the project level.

### Fitting into a Stage-Gate Process

Typical stage-gate SDLC:

| Stage | Activities | Gate | Estimate Accuracy | Usage |
|---|---|---|---|---|
| 0. Discovery | Market opportunity, feasibility | 1 | –75%, +300% | Vision Estimate (internal only) |
| 1. Scoping | Product vision, marketing requirements | 2 | –50%, +100% | Exploratory Estimate (internal) |
| 2. Planning | Detailed requirements, dev plans, budget | 3 | –20%, +25% | Budget Estimate (publish high end) |
| 3. Development | Main software development | 4 | –10%, +10% | Final Commitment Estimate |
| 4. Testing & Validation | Final test plan, release decision | 5 | –5%, +5% | Updated as needed |
| 5. Launch | Rollout, postmortem | — | — | — |

> **Tip #78** Coordinate your Standardized Estimation Procedure with your SDLC.

### Sequential Projects Procedure (Example)

| Phase | Approaches | Accuracy | Usage |
|---|---|---|---|
| **I. Exploratory** (Product Definition) | Bottom-up (WBS), Standard Components, Analogy. Wideband Delphi to converge. | –50%, +100% | Internal only |
| **II. Budget** (Design Complete) | Bottom-up (WBS), Standard Components, Function Points + estimation tool. Iterate to <5% convergence. | –20%, +25% | Budget allocation. Publish high end only. |
| **III. Preliminary Commitment** (After 2nd Interim Release) | Detailed task list. Individual developer estimates (Best/Worst/Expected + PERT). Compare with Phase II. | ±10% | External commitments to high end. |
| **IV. Final Commitment** (After 3rd Interim Release) | Compare actuals to estimates: `RemainingEffort = PlannedRemaining × (ActualToDate / PlannedToDate)`. Add omitted tasks. | ±10% | External commitments to nominal. |
| **V. Reestimate** | Any time major assumptions change (requirements, staff, schedule) | As appropriate | |
| **VI. Project Completion** | Archive data. Review estimation accuracy. Propose procedure revisions. | — | |

### Key Design Principles

1. **Early stages**: Multiple approaches, group techniques (Wideband Delphi), wide ranges
2. **Middle stages**: Counting/computing with historical data, narrowing ranges
3. **Late stages**: Project-specific data, bottom-up task estimation, tightest ranges
4. **Clear escalation**: Each stage states explicitly what the estimate can and cannot be used for
5. **Self-improving**: Data collected at project completion feeds future estimates

---

## Summary: Estimation Technique Selection Guide

| Technique | Best For | Accuracy | Project Stage |
|---|---|---|---|
| Count, Compute | Anywhere countable items exist | High | Early–Late |
| Historical Data (Org) | Medium–large projects | Medium–High | Early–Middle |
| Historical Data (Project) | All projects after initial data | High | Middle–Late |
| Individual Expert Judgment | Task-level estimates | Medium | Middle–Late (bottom-up) |
| Decomposition + PERT | Any multi-task estimate | Medium–High | Early–Late |
| Work Breakdown Structure | Preventing omissions | Medium | Early–Middle |
| Estimation by Analogy | Similar past projects exist | Medium | Early–Late |
| Fuzzy Logic | Early size estimates | Medium | Early |
| Standard Components | Architecturally similar systems | Medium | Early |
| Story Points | Iterative projects | Medium–High | Early–Middle |
| T-Shirt Sizing | Feature prioritization with stakeholders | N/A (prioritization) | Early |
| Group Reviews | Improving individual estimates | Medium | Early–Middle |
| Wideband Delphi | Unfamiliar systems, diverse disciplines | Medium | Early |
| Estimation Tools | Large projects, sanity-checking | High | Early–Middle |
| Multiple Approaches | All projects | High | Early–Late |
| Standardized Procedure | Organizational consistency | High | All stages |
