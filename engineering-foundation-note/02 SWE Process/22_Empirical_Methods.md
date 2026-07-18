---
tags: [empirical-methods, experimentation, research, engineering-foundations, swebok]
---

# Empirical Methods

> *Source: SWEBOK v4 Engineering Foundations KA; Wohlin et al. Experimentation in Software Engineering*

## Purpose

Engineers must validate their claims with evidence. Empirical methods provide the framework for gathering, analyzing, and interpreting evidence systematically. In software engineering, this means testing hypotheses about tools, techniques, processes, and products — not just relying on intuition or authority.

## Three Study Types

### 1. Designed Experiments

**What:** Manipulate independent variables to test a clear hypothesis about cause-effect relationships.

**Key characteristics:**
- **Controlled environment** — Researchers control the conditions
- **Random assignment** — Subjects randomly assigned to treatment/control groups
- **Hypothesis-driven** — Clear null and alternative hypotheses
- **Cause-effect** — Can establish causation (not just correlation)

**Structure:**
```
Hypothesis: "Using TDD reduces defect density"
Independent variable: Development method (TDD vs traditional)
Dependent variable: Defect density (defects/KLOC)
Control variables: Team experience, problem complexity, language
Subjects: 20 developers, randomly assigned to 2 groups
```

**Example:**
| Group | Method | Defects/KLOC | Time |
|---|---|---|---|
| A (n=10) | TDD | 2.3 | 15% longer |
| B (n=10) | Traditional | 4.7 | baseline |

**Threats to validity:**
- **Internal** — Did the treatment cause the effect? (maturation, selection bias, instrumentation)
- **External** — Does it generalize? (ecological validity, population validity)
- **Construct** — Are we measuring what we think? (mono-operation bias, mono-method bias)
- **Conclusion** — Is the statistical analysis valid? (violated assumptions, fishing)

### 2. Observational / Case Studies

**What:** Observe phenomena in real-world context without manipulating variables.

**Key characteristics:**
- **Natural setting** — Study real projects, not lab experiments
- **No manipulation** — Observe what happens, don't control it
- **Context-rich** — Capture the "how" and "why" behind the numbers
- **Exploratory** — Often used to generate hypotheses for later experiments

**Types:**
- **Ethnography** — Embed in a team, observe their practices
- **Action research** — Intervene, observe, reflect, iterate
- **Case study** — In-depth analysis of one or few projects

**When to use:**
- Understanding why a practice works (not just whether it works)
- Studying phenomena that can't be controlled (organizational change, adoption)
- Generating hypotheses for controlled experiments
- Validating lab findings in real-world context

**Example:**
> "We observed 5 teams adopting microservices over 18 months. Teams with dedicated DevOps support deployed 3x more frequently. Key insight: the bottleneck wasn't code — it was environment provisioning."

### 3. Retrospective Studies

**What:** Analyze archived historical data to find relationships, predict outcomes, or identify trends.

**Key characteristics:**
- **Existing data** — Uses version control history, bug trackers, CI/CD logs
- **No intervention** — Purely analytical
- **Large samples** — Can analyze thousands of commits/issues
- **Limited by data quality** — "Garbage in, garbage out"

**Common data sources:**
- Git history (commits, authors, branches, merges)
- Issue trackers (Jira, GitHub Issues — defects, stories, priorities)
- CI/CD pipelines (build times, failure rates, deployment frequency)
- Code review systems (comments, approval times, rejection rates)

**Example:**
> "Analysis of 10,000 commits across 50 repositories showed that files with >500 LOC had 3x more post-release defects. However, the relationship was mediated by code review coverage."

**Limitations:**
- **Correlation ≠ causation** — High LOC correlates with defects, but reducing LOC won't reduce defects
- **Selection bias** — Historical data reflects past decisions, not controlled conditions
- **Missing context** — Why a decision was made is rarely recorded
- **Data quality** — Inconsistent labeling, missing fields, outdated records

## Choosing the Right Method

| Question | Best Method |
|---|---|
| "Does X cause Y?" | Designed experiment |
| "How/why does X work?" | Observational study |
| "What happened in our past projects?" | Retrospective study |
| "What should we test next?" | Retrospective → hypothesis → experiment |
| "Does this work in practice?" | Case study (after lab experiment) |

## Experimental Design Basics

### Hypothesis Testing

**Null hypothesis (H₀):** No effect / no difference
**Alternative hypothesis (H₁):** There is an effect / difference

**Example:**
- H₀: TDD has no effect on defect density
- H₁: TDD reduces defect density

**Possible outcomes:**

| | H₀ is true | H₀ is false |
|---|---|---|
| **Reject H₀** | Type I error (α) | Correct (power) |
| **Fail to reject H₀** | Correct | Type II error (β) |

- **Type I error (α):** False positive — conclude there's an effect when there isn't (typically α = 5%)
- **Type II error (β):** False negative — miss a real effect (typically β = 20%)
- **Power (1 − β):** Probability of detecting a real effect (typically 80%)

### Sample Size

**Depends on:**
- Effect size (how big a difference you want to detect)
- Significance level (α)
- Desired power (1 − β)
- Variance in the population

**Rule of thumb:** Smaller effects need larger samples. If you expect a small effect, you need many subjects.

### Variables

| Type | Definition | Example |
|---|---|---|
| **Independent** | What you manipulate | Development method (TDD vs traditional) |
| **Dependent** | What you measure | Defect density, productivity |
| **Controlled** | What you hold constant | Team experience, problem complexity |
| **Confounding** | Uncontrolled variables that affect results | Developer motivation, learning effect |

## Practical Guidelines

### Do:
- State hypotheses before collecting data
- Use random assignment when possible
- Control for confounding variables
- Report effect sizes, not just p-values
- Replicate studies to build confidence
- Publish negative results (they're still valuable)

### Don't:
- Cherry-pick data to support a hypothesis
- Confuse correlation with causation
- Generalize from a single study
- Ignore threats to validity
- Use p-hacking (running many tests until one is significant)

## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[02 SWE Process/21_Measurement_Theory|21_Measurement_Theory]] — Measurement scales and GQM
- [[01 Physics and Math/09_Math_Stats_and_Economics|09_Math_Stats_and_Economics]] — Statistical foundations (distributions, hypothesis testing)
- [[02 SWE Process/18_Evaluation_and_Improvement|18_Evaluation_and_Improvement]] — Applying empirical methods to process improvement
