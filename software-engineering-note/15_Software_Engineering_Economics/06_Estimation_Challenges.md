---
tags: [estimation, size, effort, schedule, cost, software-engineering-economics]
source: "McConnell, Steve. Software Estimation: Demystifying the Black Art. Microsoft Press, 2006. Part III-IV (Ch 18-22)."
created: 2026-07-21
---

# 06 — Estimation Challenges: Size, Effort, Schedule, Cost & Presentation

> Source: McConnell, *Software Estimation*, Part III "Specific Estimation Challenges" (Ch 18–21) and Part IV (Ch 22). Chapter 23 (Sanity Checks & Tips) was not present in the source extract.

---

## 1. Special Issues in Estimating Size (Ch 18)

### 1.1 Common Size Measures

| Measure | Notes |
|---------|-------|
| Lines of Code (LOC) | Most common; workhorse despite flaws |
| Function Points (FP) | Synthetic measure from requirements; IFPUG standard |
| Story Points | Iterative/Agile projects |
| Features, Use Cases, Web Pages, GUI components, DB tables, Classes, Functions | Various proxy measures |

### 1.2 Lines of Code — Pros & Cons

**Advantages:**
- Easily collected via tools for past projects
- Lots of historical data available
- Effort per LOC roughly constant across languages (more a function of project size/kind)
- Cross-project comparisons possible
- Most commercial estimation tools ultimately use LOC

**Disadvantages:**
- Simple "LOC per staff month" models are error-prone (diseconomy of scale, different coding rates)
- Cannot be used for individual task assignments (huge productivity variance between programmers)
- More code complexity than calibration projects undermines accuracy
- Counterintuitive for estimating requirements/design work
- LOC must be estimated by proxy (not directly)
- "Line of code" must be defined carefully (comments, blank lines, etc.)

> **Churchill analogy:** "The LOC measure is a terrible way to measure software size, except that all the other ways to measure size are worse."

**Tip #80:** Use LOC but remember the general limitations of simple measures and the specific hazards of the LOC measure.

### 1.3 Function-Point Estimation

Function points = synthetic measure calculable from requirements spec early in the project.

**Five components counted:**
1. **External Inputs** — screens, forms, dialogs, control signals (add/delete/change data)
2. **External Outputs** — screens, reports, graphs, control signals generated
3. **External Queries** — input/output combos; direct DB lookup with simple formatting
4. **Internal Logical Files** — major logical groups of data controlled by the program
5. **External Interface Files** — files controlled by other programs that this program interacts with

**Unadjusted FP Count** = Σ (count × complexity multiplier) for each component:

| Characteristic | Low | Medium | High |
|---------------|-----|--------|------|
| External Inputs | ×3 | ×4 | ×6 |
| External Outputs | ×4 | ×5 | ×7 |
| External Queries | ×3 | ×4 | ×6 |
| Internal Logical Files | ×7 | ×10 | ×15 |
| External Interface Files | ×5 | ×7 | ×10 |

**Influence Multiplier:** 14 factors (data communications, online entry, complexity, etc.) → multiplier 0.65–1.35.

> ⚠️ Studies show **Unadjusted FP** correlates *better* with ultimate size than Adjusted FP. ISO/IEC 20926:2003 is based on Unadjusted FP. Eliminate "low/high complexity" judgments — classify all as "medium."

**Tip #81:** Count function points to obtain an accurate early-in-the-project size estimate.

### 1.4 FP → LOC Conversion

| Language | Min | Mode | Max |
|----------|-----|------|-----|
| C | 60 | 128 | 170 |
| C# | 40 | 55 | 80 |
| C++ | 40 | 55 | 140 |
| Java | 40 | 55 | 80 |
| Visual Basic | 15 | 32 | 41 |
| SQL | 7 | 13 | 15 |
| Perl | 10 | 20 | 30 |

Ranges typically span factor of 2–3. Use organizational historical data for narrower ranges.

### 1.5 Simplified FP Techniques

**Dutch Method (NESMA Indicative):**
```
Indicative FP = (35 × InternalLogicalFiles) + (15 × ExternalInterfaceFiles)
```
Lower accuracy but much lower effort — useful for rough early estimates. **Tip #82.**

**GUI Elements:** Count GUI elements → map to FP equivalents → convert to LOC. Introduces multiple layers of uncertainty. **Tip #83.**

### 1.6 Summary of Size Estimation Techniques

| Technique | Chapter | What Can Be Estimated |
|-----------|---------|----------------------|
| Analogy | 11 | Features, FP, Web pages, GUI, DB tables, interfaces, LOC |
| Decomposition | 10 | Same as above |
| Dutch Method | 18 | FP, LOC |
| Estimation Tools | 14 | FP, LOC |
| Function Points | 18 | FP, LOC |
| Fuzzy Logic | 12 | FP, LOC |
| Group Reviews | 13 | Features, stories, story points, requirements, use cases, FP, etc. |
| GUI Elements | 18 | FP, LOC |
| Standard Components | 12 | FP, LOC |
| Story Points | 12 | Story points, LOC |
| Wideband Delphi | 13 | Features, stories, story points, requirements, use cases, FP, etc. |

**Tip #84:** Size is the single largest cost driver. Use multiple size-estimation techniques and look for convergence.

---

## 2. Special Issues in Estimating Effort (Ch 19)

### 2.1 Key Influences

- **Largest influence:** Size of the software being built
- **Second largest:** Organization's productivity
- Productivity can vary **10× or more** between organizations in the same industry (e.g., Microsoft Excel 3.0 = 13,000 LOC/staff-year vs. Lotus 123 v.3 = 1,500 LOC/staff-year)

### 2.2 Computing Effort from Size

#### Informal Comparison to Past Projects

1. Select past projects within a **factor of 3** size range of the new project
2. Compute productivity range (LOC/staff-month) from those projects
3. Divide size range by productivity range → effort range

**Example:** 65,000–100,000 LOC, past productivity 986–1,612 LOC/staff-month → **40–101 staff months** (±1 SD, ~68% confidence).

> ⚠️ The estimate includes whatever effort was in the historical data — may or may not include requirements, management, documentation.

#### Estimation Software (Construx Estimate)

With historical data: nominal 80 staff months, 20–80% range 65–94 months.
With industry-average data: nominal 84 staff months, range 47–216 months (much wider).

**Tip #85:** Use software tools based on the science of estimation for most accurate effort computation.

### 2.3 Industry-Average Effort Graphs (Figures 19-1 to 19-9)

Effort graphs exist for nine project types:
1. Real-time projects
2. Embedded systems
3. Telecommunications
4. Systems software & drivers
5. Scientific systems & engineering research
6. Shrink-wrap & packaged software
7. Public internet systems
8. Internal intranet projects
9. Business systems

- Bold blue line = industry-average total technical effort
- Upper black line = +1 SD above average
- Logarithmic scale
- Projects >100K LOC or >1,000 FP: manual methods introduce significant error
- Projects >500K LOC or >5,000 FP: failure to use estimation software = management malpractice (Jones)

**Tip #86:** Use industry-average graphs for rough estimates in the wide part of the Cone.

### 2.4 ISBSG Method

International Software Benchmarking Standards Group formula:

```
StaffMonths = A × FunctionPoints^B × MaximumTeamSize^C
```

| Project Type | A | B | C | Calibration Projects |
|-------------|---|---|---|-----|
| General | 0.512 | 0.392 | 0.791 | ~600 |
| Mainframe | 0.685 | 0.507 | 0.464 | 63–363 |
| Mid-Range | 0.472 | 0.375 | 0.882 | " |
| Desktop | 0.157 | 0.591 | 0.810 | " |
| 3GL | 0.425 | 0.488 | 0.697 | " |
| 4GL | 0.317 | 0.472 | 0.784 | " |
| Enhancement | 0.669 | 0.338 | 0.758 | " |
| New Development | 0.520 | 0.385 | 0.866 | " |

> Smaller teams → smaller total effort (e.g., team of 5 vs. 10: 43 vs. 75 staff months).

**Tip #87:** Use ISBSG + other methods; look for convergence or spread.

### 2.5 Comparing Effort Estimates

Weight techniques by accuracy:
1. **Highest weight:** Historical data-based (informal comparison, calibrated estimation tools)
2. **Lower weight:** Industry-average methods (graphs, ISBSG)

**Tip #88:** Give more weight to techniques that tend to produce the most accurate results.

---

## 3. Special Issues in Estimating Schedule (Ch 20)

### 3.1 The Basic Schedule Equation

```
ScheduleInMonths = 3.0 × StaffMonths^(1/3)
```

- Coefficient ranges 2.0–4.0 (calibrate with historical data)
- Schedule is a **cube-root function of effort** — one of the most replicated results in software engineering (Boehm 1981)
- Implication: schedule variability is much lower than scope variability
  - 4× scope variability → only 1.6× schedule variability
  - 2× scope variability → only 1.25× schedule variability

**Tip #89:** Use the Basic Schedule Equation for early schedule estimates on medium-to-large projects.

> ⚠️ Not intended for small projects or late phases. Switch to other techniques when specific team members are known.

### 3.2 Informal Comparison to Past Projects

```
EstimatedSchedule = PastSchedule × (EstimatedEffort / PastEffort)^(1/3)
```

- Exponent 1/3 for projects >50 staff months; 1/2 for smaller
- **Tip #90**

### 3.3 Jones's First-Order Estimation Practice

```
ScheduleInMonths = FunctionPoints^exponent
```

| Software Kind | Better | Average | Worse |
|--------------|--------|---------|-------|
| Object-oriented | 0.33 | 0.36 | 0.39 |
| Client-server | 0.34 | 0.37 | 0.40 |
| Business/intranet | 0.36 | 0.39 | 0.42 |
| Shrink-wrap/scientific/engineering/internet | 0.37 | 0.40 | 0.43 |
| Embedded/telecom/drivers/systems | 0.38 | 0.41 | 0.44 |

Very low effort, low accuracy — useful as a quick reality check. **Tip #91.**

### 3.4 Schedule Compression & the Impossible Zone

**Key findings (all researchers agree):**

1. **Shortening the nominal schedule increases overall effort** — larger teams require more coordination, more communication paths, more parallel work (rework risk).

2. **The Impossible Zone exists** — schedule compression beyond **25%** from nominal is not possible. Not by working harder, smarter, or finding creative solutions. It simply can't be done. **Tip #93.**

3. **Extending schedule beyond nominal usually reduces total effort** (if team size is reduced) — fewer communication problems, less overlap, more in-phase defect fixing.

![Schedule Compression Effects]

| Schedule Change | Effort Change |
|----------------|---------------|
| –15% | +100% |
| –10% | +50% |
| –5% | +25% |
| Nominal | 0% |
| +10% | –30% |
| +20% | –50% |
| +30% | –65% |
| >+30% | Not practical |

**Tip #92:** Do not shorten a schedule estimate without increasing the effort estimate.
**Tip #94:** Reduce costs by lengthening the schedule and conducting the project with a smaller team.

### 3.5 Team Size Economics (Putnam)

For medium-sized business systems (~57K LOC):

| Team Size | Effect |
|-----------|--------|
| 1.5–3 | Baseline |
| 3–5 | Schedule ↓, Effort ↑ (expected) |
| 5–7 | Schedule ↓, Effort ↑ (expected) |
| 9–11 | Schedule ↑, Effort ↑ (degradation begins) |
| 15–20 | Schedule flat, Effort ↑↑ (severe degradation) |

**Optimal: 5–7 people for medium business-systems projects (35K–100K LOC). Tip #95.**

### 3.6 Staffing Constraints

- Average team size = Effort / Schedule (e.g., 80 staff months / 12 months = 6–7 people)
- Macro formulas assume flexible team size — if fixed, schedule will vary more widely
- Detailed planning (actual availability, specific assignments) takes precedence over macro estimates
- **Tip #96:** Use schedule estimation for plausibility; use detailed planning for the final schedule.

### 3.7 Comparing Schedule Methods

Prioritize (highest to lowest weight):
1. Estimation tool calibrated with historical data
2. Basic Schedule Equation + Informal Comparison (with organization-specific coefficients)
3. Jones's First-Order (generic — remove before looking for convergence)

**Tip #97:** Remove overly generic techniques before looking for convergence.

---

## 4. Estimating Planning Parameters (Ch 21)

### 4.1 Activity Effort Breakdown

#### Core Technical Activities (by project size)

| Size | Architecture | Construction | System Test |
|------|-------------|--------------|-------------|
| 1 KLOC | 11% | 70% | 19% |
| 25 KLOC | 16% | 57% | 27% |
| 125 KLOC | 18% | 53% | 29% |
| 500 KLOC | 19% | 44% | 37% |

> As project size increases: architecture grows slightly, construction shrinks, system test grows significantly (diseconomy of scale).

#### Requirements & Management Add-ons

| Size | Requirements (add to base) | Management (add to base) |
|------|--------------------------|-------------------------|
| 1 KLOC | 5% | 10% |
| 25 KLOC | 5% | 12% |
| 125 KLOC | 8% | 14% |
| 500 KLOC | 10% | 17% |

#### Total Effort Breakdown (incl. requirements + management)

| Size | Req. | Architecture | Construction | System Test | Mgmt |
|------|------|-------------|--------------|-------------|------|
| 1 KLOC | 4% | 10% | 61% | 16% | 9% |
| 25 KLOC | 4% | 14% | 49% | 23% | 10% |
| 125 KLOC | 7% | 15% | 44% | 23% | 11% |
| 500 KLOC | 8% | 15% | 35% | 29% | 13% |

#### Adjustments by Project Type

| Activity | Business/Intranet | Embedded/Telecom | Shrink-wrap/Scientific/Internet |
|----------|-------------------|------------------|-------------------------------|
| Requirements | –3% | +20% | –20% |
| Architecture | –7% | +10% | –5% |
| Construction | +5% | –10% | +2% |
| System Test | –7% | +6% | +9% |
| Management | +3% | +3% | –15% |

**Tip #98:** Consider project size, project type, and calibration data when allocating effort.

### 4.2 Developer-to-Tester Ratios

| Environment | Typical Ratio |
|------------|---------------|
| Common business systems | 3:1 to 20:1 (often no dedicated testers) |
| Common commercial systems | 1:1 to 5:1 |
| Scientific/engineering | 5:1 to 20:1 (often no dedicated testers) |
| Common systems projects | 1:1 to 5:1 |
| Safety-critical systems | 5:1 to 1:2 |
| Microsoft Windows 2000 | 1:2 |
| NASA Space Shuttle Flight Control | 1:10 |

### 4.3 Schedule Breakdown by Activity

| Size | Architecture | Construction | System Test |
|------|-------------|--------------|-------------|
| 1 KLOC | 15–25% | 50–65% | 15–20% |
| 25 KLOC | 15–30% | 50–60% | 20–25% |
| 125 KLOC | 20–35% | 45–55% | 20–30% |
| 500 KLOC | 20–40% | 40–55% | 20–35% |

Requirements schedule add-on: 10–30% depending on project size.

**Tip #99:** Consider size, type, and development approach in allocating schedule.

### 4.4 Ideal Effort → Planned Effort

Effort estimates are in *ideal* staff months. Conversion factors:
- Cocomo II assumes **152 hours/month** of focused project time
- Capers Jones reports average **132 hours/month** (6 hrs/day)
- Entrepreneurial orgs: 40–50 hrs/week possible
- Large established orgs: 20–30 hrs/week of focused time, often across multiple projects

Dilution factors: vacation, holidays, sick days, training, corporate overhead, multi-project assignments.

### 4.5 Cost Estimates

Key complicating factors:
- **Overtime:** Uncompensated OT reduces cost; contractor OT increases it
- **Direct vs. Burdened Cost:** Burden (corporate overhead) can range from 30% to 125%+ of salary
- **Other direct costs:** Travel, specialized tools, hardware

### 4.6 Defect Production & Removal

#### Defect Production (Jones, per Function Point)

| Activity | Avg Defects/FP |
|----------|---------------|
| Requirements | 1.00 |
| Architecture | 1.25 |
| Construction | 1.75 |
| Documentation | 0.60 |
| Bad fixes | 0.40 |
| **TOTAL** | **5.00** |

≈ 50 defects per 1,000 LOC (language-dependent).

#### Defect Density by Project Size

| Project Size (LOC) | Typical Error Density |
|-------------------|----------------------|
| <2K | 0–25 errors/KLOC |
| 2K–16K | 0–40 errors/KLOC |
| 16K–64K | 0.5–50 errors/KLOC |
| 64K–512K | 2–70 errors/KLOC |
| ≥512K | 4–100 errors/KLOC |

> Larger projects produce more defects per LOC — key contributor to diseconomy of scale. **Tip #100.**

#### Defect Removal Rates

| Removal Step | Lowest | Modal | Highest |
|-------------|--------|-------|---------|
| Informal design reviews | 25% | 35% | 40% |
| Formal design inspections | 45% | 55% | 65% |
| Informal code reviews | 20% | 25% | 35% |
| Formal code inspections | 45% | 60% | 70% |
| Modeling/prototyping | 35% | 65% | 80% |
| Personal desk checking | 20% | 40% | 60% |
| Unit test | 15% | 30% | 50% |
| Integration test | 25% | 35% | 40% |
| System test | 25% | 40% | 55% |
| Low-volume beta (<10 sites) | 25% | 35% | 40% |
| High-volume beta (>1,000 sites) | 60% | 75% | 85% |

> **Industry average:** ~84% pre-release defect removal (792 remaining out of 5,000 in a 1,000-FP system).
> **Best-in-class:** ~95% removal (226 remaining).

**Reliability improvement costs (Putnam):**
- 95% → 99%: add 25% to main-build schedule
- 99% → 99.9%: add another 25%

**Tip #101:** Use defect-removal-rate data to estimate how many defects your QA practices will remove.

### 4.7 Risk & Contingency Buffers

**Risk Exposure (RE)** = Probability × Severity (impact)

- Total RE is the starting point for quantitative buffer planning
- 50% chance you'll add more than RE, 50% chance less
- For higher certainty: plan buffer > RE; for risk tolerance: plan buffer < RE
- Separate buffers needed for schedule, effort, cost, features, quality

**Tip #102:** Use total RE as starting point; review specific risks to decide if buffer should be larger or smaller than RE.

### 4.8 Other Rules of Thumb

| Factor | Adjustment |
|--------|-----------|
| Admin/clerical support | +5–10% effort |
| IT support (lab setup, etc.) | +2–4% effort |
| Configuration management/build | +2–8% effort |
| Requirements growth | +1–4% per month |
| Multi-company, multi-city dev | +25% effort |
| International outsourced dev | +40% effort |
| New language/tools (first time) | +20–40% effort |
| New environment (first time) | +20–40% effort |

---

## 5. Estimate Presentation Styles (Ch 22)

### 5.1 Communicating Assumptions

Document and communicate:
- Which features are required / NOT required
- How elaborate features need to be
- Availability of key resources
- Dependencies on third-party performance
- Major unknowns
- Major influences and sensitivities
- How good the estimate is
- What the estimate can be used for

**Tip #104:** Document and communicate the assumptions embedded in your estimate.

### 5.2 Expressing Uncertainty

#### Plus-or-Minus Qualifiers

- "6 months, ±2 months" or "$600K, +$200K, –$100K"
- Typically sized for ±1 SD (68% confidence); 16% chance above, 16% chance below
- Minus qualifier often smaller than plus (asymmetric distribution)
- **Weakness:** Gets stripped to the core single-point number as it propagates up the org

#### Risk Quantification

Attach specific impacts to specific risks:

| Impact | Risk Description |
|--------|-----------------|
| +1.5 months | >20% new features vs. prior version |
| +1 month | Graphics subsystem delivered late |
| +1 month | New tools don't work as planned |
| +1 month | Can't reuse 80% of DB code |

> **Purpose:** Communicate to nontechnical stakeholders that the project presents risks — not to deluge them with details.

**Tip #105:** Know whether you're presenting uncertainty in an *estimate* or uncertainty affecting your ability to meet a *commitment*.

#### Confidence Factors

| Delivery Date | Probability of Delivering On or Before |
|--------------|---------------------------------------|
| January 15 | 20% |
| March 1 | 50% |
| November 1 | 80% |

- Approximate using multipliers from the Cone of Uncertainty
- Avoid "90% confident" without quantitative basis
- Consider whether you need to present low-probability options at all
- **Tip #106:** Don't present outcomes that are only remotely possible.
- **Tip #107:** Consider graphic presentation over tabular.

#### Case-Based Estimates

| Case | Date / Feature Set |
|------|-------------------|
| Best case (estimate) | January 15 |
| Planned case (commitment) | March 1 |
| Current case (estimate) | April 1 |
| Worst case (estimate) | November 1 |

> If planned case = best case and current case = worst case: your project is in trouble!

Can also be expressed in terms of feature delivery levels (Level 1/2/3 features).

#### Coarse Dates & Time Periods

Match presentation units to underlying accuracy:
- Early Cone: quarters, years
- Mid-project: months, weeks
- Late project: weeks, days

> "Second quarter" is immune to simplification; "6 months, +3/−1" gets simplified to "6 months."

### 5.3 Using Ranges

Key questions:
1. **What probability level should the range include?** ±1 SD (68%)? Wider?
2. **How do company budgeting/reporting processes handle ranges?** Many systems won't accept them.
3. **Can you live with the midpoint?** Managers often average the range or grab the low end.
4. **Present full range or only nominal-to-high?** Projects rarely shrink; estimates tend to err low.
5. **Combine ranges with other techniques?** List assumptions or quantified risks alongside.

> "It isn't the presentation that makes the estimate useless; it's the uncertainty in the estimate itself."

**IEEE-CS/ACM Code of Ethics, Item 3.09:** Software engineers shall "ensure realistic quantitative estimates of cost, scheduling, personnel, quality and outcomes ... and **provide an uncertainty assessment** of these estimates."

**Tip #108:** Use a presentation style that reinforces the message about your estimate's accuracy.

### 5.4 Ranges vs. Commitments

- Wide estimation range ≠ useless — it accurately conveys that the *estimate* is uncertain
- After uncertainty is reduced enough to support a commitment, **don't express the commitment as a range**
- A commitment needs to be a single-point number; the estimation range illustrates how risky that commitment is

**Tip #109:** Don't try to express a commitment as a range. A commitment needs to be specific.

---

## 6. Key Tips Summary

| # | Topic | Tip |
|---|-------|-----|
| 80 | Size | Use LOC; remember its limitations |
| 81 | Size | Count function points for accurate early size estimate |
| 82 | Size | Use Dutch Method for low-cost ballpark |
| 83 | Size | Use GUI elements for ballpark in wide Cone |
| 84 | Size | Size is the largest cost driver — use multiple techniques |
| 85 | Effort | Use estimation science tools for accurate effort from size |
| 86 | Effort | Use industry-average graphs for rough early estimates |
| 87 | Effort | Use ISBSG method; combine with others |
| 88 | Effort | Weight techniques by accuracy |
| 89 | Schedule | Use Basic Schedule Equation for early medium-to-large projects |
| 90 | Schedule | Use Informal Comparison to Past Projects formula |
| 91 | Schedule | Use Jones's First-Order for quick reality check |
| 92 | Schedule | Don't shorten schedule without increasing effort |
| 93 | Schedule | Don't compress schedule >25% (Impossible Zone) |
| 94 | Schedule | Reduce costs by lengthening schedule with smaller team |
| 95 | Schedule | Avoid >7 people for medium business-systems projects |
| 96 | Schedule | Use estimation for plausibility, detailed planning for final schedule |
| 97 | Schedule | Remove generic techniques before looking for convergence |
| 98 | Planning | Consider size, type, calibration data for effort allocation |
| 99 | Planning | Consider size, type, approach for schedule allocation |
| 100 | Planning | Estimate defect production numbers |
| 101 | Planning | Estimate defect removal rates for QA planning |
| 102 | Planning | Use total Risk Exposure as starting point for buffer |
| 103 | Planning | Read the planning literature — it's bigger than estimation |
| 104 | Presentation | Document and communicate estimate assumptions |
| 105 | Presentation | Distinguish estimate uncertainty from commitment uncertainty |
| 106 | Presentation | Don't present remotely possible outcomes |
| 107 | Presentation | Consider graphic presentation |
| 108 | Presentation | Match presentation style to accuracy message |
| 109 | Presentation | Commitments should be single-point, not ranges |

---

## 7. Chapter 23 — Estimation Sanity Checks & Tips

> ⚠️ Chapter 23 was not included in the source extract. Based on McConnell's framework, sanity checks typically include:
> - Compare effort/schedule against industry averages for the project type
> - Verify that schedule is within cube-root relationship of effort
> - Check that team size doesn't exceed optimal thresholds
> - Ensure estimate ranges narrow appropriately through project phases
> - Validate that contingency buffers account for quantified risks
> - Cross-check with multiple estimation methods and investigate divergence

---

## Related Notes

- [[05_Estimation_Techniques]] — McConnell Part II: fundamental estimation techniques
- [[04_Estimation_Concepts]] — McConnell Part I: critical estimation concepts
- [[01_Software_Engineering_Economics_Overview]] — Boehm's COCOMO and economic foundations
