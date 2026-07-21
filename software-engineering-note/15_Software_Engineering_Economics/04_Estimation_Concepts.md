---
tags:
  - estimation
  - cone-of-uncertainty
  - targets
  - error
  - software-engineering-economics
  - mcconnell
source: "McConnell, Steve. *Software Estimation: Demystifying the Black Art*. Microsoft Press, 2006. Part I, Chapters 1–5."
created: 2026-07-21
---

# 04 — Estimation Concepts (McConnell Part I)

## Overview

Part I of McConnell's *Software Estimation* establishes the foundational concepts that underpin all effective software estimation. It covers the nature of estimates (vs. targets vs. commitments), the probabilistic character of project outcomes, the Cone of Uncertainty, sources of estimation error, the value of accurate estimates, and the key factors that influence project effort and schedule.

---

## 1. What Is an "Estimate"?

### 1.1 Estimates, Targets, and Commitments

| Term | Definition |
|------|------------|
| **Estimate** | A prediction of how long a project will take or how much it will cost. An unbiased, analytical assessment. |
| **Target** | A statement of a desirable business objective (e.g., "We need Version 2.1 ready for a trade show in May"). Desirable ≠ achievable. |
| **Commitment** | A promise to deliver defined functionality at a specific level of quality by a certain date. Can be more aggressive or more conservative than the estimate. |

> **Tip #1:** Distinguish between estimates, targets, and commitments.

### 1.2 Estimation vs. Planning

- **Estimation** = unbiased, analytical process. Goal: accuracy.
- **Planning** = biased, goal-seeking process. Goal: achieve a specific outcome.
- Estimates form the *foundation* for plans, but plans don't have to equal estimates.
- Combining the two tends to produce poor estimates *and* poor plans — the target substitutes for the analytically derived estimate.

> **Tip #2:** When asked for an "estimate," determine whether you're supposed to be estimating or figuring out how to hit a target.

### 1.3 Estimates as Probability Statements

- Single-point estimates ("14 weeks") are **meaningless** — they imply 100% probability of that exact outcome.
- Software project outcomes follow a **skewed probability distribution**:
  - There is a limit to how efficiently a project can go (truncated left tail).
  - There is **no limit** to how poorly a project can go (long right tail).
  - The distribution is NOT a symmetric bell curve.

> **Tip #3:** When you see a single-point "estimate," ask whether it's an estimate or really a target.
> **Tip #4:** Ask what probability is associated with a single-point number — it's not 100%.

- All estimates include a probability, whether stated or implied. Good estimates state it explicitly.
- Express estimates as ranges with confidence levels: "18 to 24 weeks" or "90% confident in 24 weeks."

### 1.4 Definitions of a "Good" Estimate

- **Capers Jones:** ±10% accuracy possible on well-controlled projects.
- **Conte, Dunsmore, and Shen (1986):** Estimates within 25% of actual, 75% of the time — the most common evaluation standard.
- Organizations at higher CMM levels show dramatically better estimation accuracy (U.S. Air Force, Boeing, Schlumberger case studies).
- Accurate estimation requires **effective project control**, not just good estimation practices.

### 1.5 Estimation's Real Purpose

> The primary purpose of software estimation is **not to predict** a project's outcome; it is to **determine whether a project's targets are realistic enough** to allow the project to be controlled to meet them.

- If the gap between target and estimate is ≤ ~20%, a skilled project manager can navigate to success through control.
- If the gap is larger, the targets must be reconsidered — no amount of "packing" will make things fit.

**Working definition:** A good estimate provides a clear enough view of project reality to allow leadership to make good decisions about how to control the project to hit its targets.

---

## 2. How Good an Estimator Are You?

### 2.1 The Estimation Quiz

McConnell provides a 10-question quiz (surface temperature of the Sun, latitude of Shanghai, etc.) asking for 90%-confidence ranges.

**Results from ~600 people:**
- Average score: **2.8 out of 10 correct**.
- Only 2% scored 8+.
- **No one has ever gotten 10 correct.**

> Most people's intuitive sense of "90% confident" is comparable to **~30% confident**.

### 2.2 Key Lessons

- **Pressure for narrow ranges is self-induced.** The instructions didn't encourage narrow ranges — professional pride did.
- Narrow ranges make estimators *appear* more knowledgeable but are usually *less* accurate.
- Customers' expectations exert strong, often unconscious, influence on estimates (Jørgensen & Sjøberg 2002).

> **Tip #5:** Don't provide "percentage confident" estimates (especially "90% confident") without quantitative basis.
> **Tip #6:** Avoid artificially narrow ranges — they misrepresent your confidence.
> **Tip #7:** Verify that pressure to narrow ranges is external, not self-induced.

---

## 3. Value of Accurate Estimates

### 3.1 Overestimation vs. Underestimation

| | Overestimation | Underestimation |
|---|---|---|
| **Mechanism** | Parkinson's Law — work expands to fill available time | Planning errors, shortchanged upstream activities, more defects, destructive late-project dynamics |
| **Penalty shape** | **Linear and bounded** | **Nonlinear and unbounded** |
| **Net effect** | Manageable through planning/control | Far more damaging |

> **Tip #8:** Don't intentionally underestimate. Address overestimation concerns through planning and control, not by biasing estimates.

### 3.2 Industry Track Record

- **Standish Group Chaos Report (2004):** 54% late, 18% failed outright, 28% on time/on budget.
- There isn't even a category for "early delivery" in the Standish data.
- **Jones:** Larger projects have dramatically worse outcomes. At 10,000 FP (~1M LOC): <1% early, 28% on time, 24% late, 48% canceled.
- The software industry has an **underestimation problem**, not a neutral estimation problem.
- Average schedule overrun: ~120%; average cost overrun: ~100% (and these numbers understate the problem because late projects routinely throw out functionality).

### 3.3 Benefits of Accurate Estimates

1. **Improved status visibility** — realistic plans enable meaningful progress tracking.
2. **Higher quality** — ~40% of defects are stress-induced; schedule pressure causes ~4× more released defects.
3. **Better coordination with non-software functions** (testing, docs, marketing, training, support).
4. **Better budgeting** and cost forecasting.
5. **Increased credibility** for the development team.
6. **Early risk information** — a mismatch between target and estimate is *valuable risk data*, not a reason to negotiate the estimate down.

> **Tip #9:** Recognize estimate-target mismatches as valuable early risk information. Act early.

### 3.4 Predictability vs. Other Attributes

- When asked "shorter average schedule with high variability vs. longer average with low variability," most executives choose the longer but predictable option.
- **Businesses value predictability more than development time, cost, or flexibility** (at least 8/10 executives).

> **Tip #10:** Understand what your business values most — don't assume it's speed.

### 3.5 Common (Ineffective) Techniques

- Most used: comparing to a similar past project based on **personal memory** — NOT correlated with accuracy.
- "Intuition" and "guessing" are correlated with cost and schedule **overruns**.
- ~60–85% of all estimates use these informal methods.

---

## 4. Where Does Estimation Error Come From?

Four generic sources of estimation error:

1. Inaccurate information about the project being estimated
2. Inaccurate information about the organization's capabilities
3. Too much chaos (estimating a moving target)
4. Inaccuracies from the estimation process itself

### 4.1 The Cone of Uncertainty

The Cone of Uncertainty represents the **best-case accuracy** possible at different project stages — created by *skilled* estimators. It's easily possible to do worse; it's not possible to do better (only luckier).

| Phase | Low-Side Error | High-Side Error | Range (High ÷ Low) |
|-------|---------------|-----------------|-------------------|
| Initial Concept | 0.25× (−75%) | 4.0× (+300%) | **16×** |
| Approved Product Definition | 0.50× (−50%) | 2.0× (+100%) | **4×** |
| Requirements Complete | 0.67× (−33%) | 1.5× (+50%) | **2.25×** |
| User Interface Design Complete | 0.80× (−20%) | 1.25× (+25%) | **1.6×** |
| Detailed Design Complete (sequential) | 0.90× (−10%) | 1.10× (+10%) | **1.2×** |

#### Key Properties of the Cone

- **Estimation accuracy depends on the level of refinement of the software's definition** (Laranjeira 1990), not on time spent estimating.
- On a **calendar-time basis**, the Cone narrows rapidly — most uncertainty is eliminated in the first ~30% of the project.
- **The Cone doesn't narrow itself.** You must force it to narrow by making decisions that remove variability (defining the product, including what you will *not* do). If decisions change later, the Cone widens.
- Without active narrowing, you get a **Cloud of Uncertainty** that persists to project end.

> **Tip #11:** Your estimate cannot exceed the accuracy possible at your project's current Cone position.
> **Tip #12:** Force the Cone to narrow by removing sources of variability.
> **Tip #13:** Use predefined uncertainty ranges from the Cone in your estimates.
> **Tip #14:** Have one person estimate "how much" and another estimate "how uncertain."

#### Cone of Uncertainty and Commitment

- Making commitments in the early, wide part of the Cone is organizational self-sabotage (2×–4× error).
- Meaningful commitments are possible ~30% into the project (after Requirements Complete / UI Design Complete).

#### Cone of Uncertainty and Iterative Development

- Short iterations go through a miniature Cone each iteration (from 4× to 1.6× in days).
- Trade-off: long-range predictability of cost/schedule/features is reduced.
- Middle ground: define majority of requirements upfront (~30% into schedule), then iterate design/construction/test. Achieves ±25% variability while retaining iterative benefits.

### 4.2 Chaotic Development Processes

Examples: poorly investigated requirements, lack of end-user involvement, poor design/coding practices, inexperienced personnel, incomplete planning, prima donnas, abandoning planning under pressure, gold-plating, lack of source control.

> **Tip #15:** You can't accurately estimate an out-of-control process. Fix the chaos first.

### 4.3 Unstable Requirements

- Requirements volatility keeps the Cone from narrowing.
- Changes are often not tracked; projects are not re-estimated.
- Projects are perceived as late even when everyone agreed to feature additions.

> **Tip #16:** For unstable requirements, consider project control strategies (short iterations, Scrum, XP, DSDM, timeboxing) rather than estimation strategies alone.
- Some organizations plan for growth: NASA SEL plans 40% increase; Cocomo II includes "requirements breakage."

### 4.4 Omitted Activities

Developers tend to overlook **20–30%** of necessary tasks (van Genuchten 1991).

Three categories of commonly omitted activities:

1. **Missing requirements:** setup/installation, data conversion, help system, interfaces, nonfunctional requirements (security, performance, usability, scalability, etc.)
2. **Missing software activities:** ramp-up time, mentoring, maintenance work, management, defect correction, cutover/deployment, integration, reviews, change control, beta management, demos, documentation
3. **Missing non-software activities:** vacations, holidays, sick days, training, company/department meetings

> **Tip #17:** Include *all* requirements — stated, implied, and nonfunctional.
> **Tip #18:** Include all software-development activities, not just coding and testing.
> **Tip #19:** On projects > a few weeks, include overhead (vacations, sick days, training, meetings).

### 4.5 Unfounded Optimism

- Developer estimates contain a built-in optimism factor of **20–30%** (van Genuchten 1991).
- Developers don't sandbag — their estimates tend to be too low.
- DoD study found a "fantasy factor" of ~1.33 in management estimates (Boehm 1981).
- The **Collusion of Optimists:** developers present optimistic estimates → executives like them → managers support them → no one checks whether estimates are well-founded.

> **Tip #20:** Don't reduce developer estimates — they're probably already too optimistic.

### 4.6 Subjectivity and Bias

- **More "control knobs" = more error**, not more accuracy. Each adjustment factor introduces a chance for subjectivity.
- Cocomo II (17 Effort Multipliers + 5 Scaling Factors = 22 control knobs): average intra-session variation of **1.7×**.
- A technique with only 1 control knob: average intra-session variation of only **1.1×**.
- As forecasting researcher J. Scott Armstrong states: "Simple methods are generally as accurate as complex methods."

> **Tip #21:** Avoid having many "control knobs" on estimates — they introduce subjectivity and degrade accuracy.

### 4.7 Off-the-Cuff Estimates

- Off-the-cuff estimates: **MMRE of 67%**.
- Reviewed estimates: **MMRE of 30%** — less than half the error.
- People often remember their past *estimate* rather than the past *actual outcome*, baking prior overruns into new estimates.
- Use of "documented facts" is negatively correlated with overruns.

> **Tip #22:** Don't give off-the-cuff estimates. Even a 15-minute review will be more accurate.

### 4.8 Unwarranted Precision

| Term | Definition |
|------|------------|
| **Accuracy** | How close to the real value a number is |
| **Precision** | How many significant digits a number has (exactness) |

- A number can be precise without being accurate, and vice versa.
- "395.7 days" implies accuracy to 4 significant digits — but "1 year" or "13 months" is usually more honest.

> **Tip #23:** Match the number of significant digits in your estimate to its accuracy.

### 4.9 Other Sources of Error

- Unfamiliar business or technology area
- Incorrect conversion from estimated time to project time
- Misunderstanding of statistical concepts (adding "best case" or "worst case" estimates)
- Budgeting processes requiring final approval in the wide part of the Cone
- Conversion errors: size → effort, or effort → schedule
- Overstated savings from new tools/methods
- Simplification of estimates as they travel up management layers

---

## 5. Estimate Influences

### First-Tier Influences (in order of significance)

1. **Project Size** — the single largest driver. More variation in size than any other factor.
2. **Kind of Software** — life-critical vs. business systems can differ dramatically in effort per unit.
3. **Personnel Factors** — capability of the team.

Programming language/environment is a *first-tier influence on the estimate* but a second-tier influence on the project outcome.

### 5.1 Project Size and Diseconomies of Scale

- Larger projects require more coordination → **communication paths increase as n²** (n × (n−1) / 2).
- Software exhibits **diseconomies of scale**: the larger the system, the greater the cost per unit.
- A 1,000,000-LOC system can require ~160× the effort of a 10,000-LOC system (not 100×).
- **Worst-case** diseconomy: 1M LOC can require **300×** the effort of 10K LOC.

| Project Size (LOC) | Productivity Range (LOC/Staff-Year) |
|-------------------|-------------------------------------|
| 10K | 2,000–25,000 |
| 100K | 1,000–20,000 |
| 1M | 700–10,000 |
| 10M | 300–5,000 |

- Productivity on the largest projects can be **5–10× lower** than on the smallest.

> **Tip #24:** Invest effort in assessing software size — it's the single most significant contributor.
> **Tip #25:** Effort scales exponentially, not linearly, with project size.
> **Tip #26:** Use software estimation tools to compute diseconomy-of-scale impacts.

#### When Diseconomies of Scale Can Be Ignored

If your past projects are **within a factor of 3 in size** (largest ÷ smallest), ratio-based estimation (e.g., LOC per staff-month) is safe — error from diseconomies is ≤ ~10%.

> **Tip #27:** Use ratio-based estimating only when new and past projects are within 3× size of each other.

### 5.2 Kind of Software

After size, the type of software is the next biggest influence. Life-critical, embedded, and systems software require far more effort per unit than business systems.

### 5.3 Personnel Factors

Individual performance varies by at least a factor of 10 in debugging, coding speed, and design quality. Personnel capability affects every phase of the project.

---

## Summary of Key Tips (Chapters 1–5)

| # | Tip |
|---|-----|
| 1 | Distinguish between estimates, targets, and commitments. |
| 2 | Determine whether you're estimating or figuring out how to hit a target. |
| 3 | Single-point "estimates" are often targets — ask which it is. |
| 4 | Ask what probability a single-point number carries — it's not 100%. |
| 5 | Don't give percentage-confident estimates without quantitative basis. |
| 6 | Avoid artificially narrow ranges — they misrepresent your confidence. |
| 7 | Verify that pressure to narrow ranges is external, not self-induced. |
| 8 | Don't intentionally underestimate — the penalty is more severe. |
| 9 | Estimate-target mismatches are valuable early risk information. |
| 10 | Understand what your business values most (often predictability). |
| 11 | Your estimate can't exceed the accuracy your Cone position allows. |
| 12 | Force the Cone to narrow by removing variability. |
| 13 | Use predefined Cone uncertainty ranges in estimates. |
| 14 | Separate "how much" estimation from "how uncertain" estimation. |
| 15 | Fix project chaos before trying to improve estimates. |
| 16 | Use project control strategies for unstable requirements. |
| 17 | Include all requirements — stated, implied, and nonfunctional. |
| 18 | Include all software-development activities, not just coding. |
| 19 | Include overhead on projects lasting more than a few weeks. |
| 20 | Don't reduce developer estimates — they're already too optimistic. |
| 21 | Avoid many estimation "control knobs" — they degrade accuracy. |
| 22 | Don't give off-the-cuff estimates; even a 15-min review helps. |
| 23 | Match significant digits to accuracy; don't over-precise. |
| 24 | Invest in assessing software size — the #1 cost driver. |
| 25 | Effort scales exponentially with size; don't assume linearity. |
| 26 | Use estimation tools to account for diseconomies of scale. |
| 27 | Ratio-based estimation is safe within 3× size range of past projects. |

---

## Related Concepts

- [[01_Economics_Fundamentals]] — economic foundations of software engineering
- [[02_Decision_Making]] — decision-making under uncertainty
- [[03_Cost_Analysis_and_Tradeoffs]] — cost analysis and tradeoff decisions
- Cone of Uncertainty → see also Boehm et al., *Software Cost Estimation with Cocomo II* (2000)
- Parkinson's Law, Student Syndrome → see Goldratt, *Critical Chain* (1997)
