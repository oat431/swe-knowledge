---
tags: [statistics, hypothesis-testing, confidence-intervals, engineering-foundations, swebok]
---

# Statistical Inference

> *Source: SWEBOK v4 Engineering Foundations KA; NIST/SEMATECH e-Handbook of Statistical Methods*

## Purpose

Statistical inference lets engineers generalize from samples to populations — making decisions under uncertainty. Without inference, you can only describe what you've observed; with it, you can predict, compare, and validate.

## Populations and Samples

| Concept | Definition | Example |
|---|---|---|
| **Population** | The entire set of items of interest | All defects in the product lifetime |
| **Sample** | A subset of the population | This month's 150 defects |
| **Parameter** | A numerical characteristic of the population (μ, σ) | True mean defect density |
| **Statistic** | A numerical characteristic of a sample (x̄, s) | Observed mean defect density |
| **Random variable** | A variable whose value is determined by chance | Defect count per module |

**Key insight:** We almost never know the population parameters. We estimate them from sample statistics.

## Common Distributions

| Distribution | Use Case | Parameters | Shape |
|---|---|---|---|
| **Binomial** | Count of successes in n trials | n (trials), p (probability) | Symmetric if p=0.5 |
| **Poisson** | Count of events over time/space | λ (rate) | Right-skewed |
| **Normal** | Large number of values, many small effects | μ (mean), σ (std dev) | Symmetric bell curve |
| **t-distribution** | Small samples, unknown σ | df (degrees of freedom) | Like normal, heavier tails |
| **Chi-squared** | Variance testing, goodness of fit | df | Right-skewed |

### When to Use Which

| Situation | Distribution |
|---|---|
| Count of defects per module | Poisson |
| Pass/fail test results | Binomial |
| Measurement values (time, size) | Normal |
| Small sample, unknown population σ | t-distribution |
| Testing if variance equals a value | Chi-squared |

## Point and Interval Estimates

### Point Estimates

A single value as the "best guess" for a population parameter:

| Parameter | Point Estimator |
|---|---|
| Mean (μ) | Sample mean x̄ |
| Variance (σ²) | Sample variance s² |
| Proportion (p) | Sample proportion p̂ |

**Problem:** Point estimates don't tell you how uncertain you are.

### Confidence Intervals

A range that likely contains the true parameter, with a specified confidence level.

**General form:**
$$\text{Estimate} \pm \text{Margin of Error}$$

**For the mean (known σ):**
$$\bar{x} \pm z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}}$$

**For the mean (unknown σ, small sample):**
$$\bar{x} \pm t_{\alpha/2, n-1} \cdot \frac{s}{\sqrt{n}}$$

**For a proportion:**
$$\hat{p} \pm z_{\alpha/2} \cdot \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$$

| Confidence Level | z-value | Meaning |
|---|---|---|
| 90% | 1.645 | 90% of such intervals contain the true mean |
| 95% | 1.960 | 95% of such intervals contain the true mean |
| 99% | 2.576 | 99% of such intervals contain the true mean |

**Interpretation:** "We are 95% confident that the true mean defect density is between 2.1 and 3.4 defects/KLOC."

> **Common mistake:** "There's a 95% probability the mean is in this interval." Wrong — the interval either contains the mean or it doesn't. The 95% refers to the method, not the specific interval.

## Hypothesis Testing

### Structure

1. **Null hypothesis (H₀):** The status quo — no effect, no difference
2. **Alternative hypothesis (H₁):** The claim we're testing
3. **Test statistic:** Computed from sample data
4. **p-value:** Probability of observing results as extreme as ours, assuming H₀ is true
5. **Decision:** Reject H₀ if p-value < α

### Types of Errors

| | H₀ is true | H₀ is false |
|---|---|---|
| **Reject H₀** | Type I error (α) — false positive | Correct — true positive |
| **Fail to reject H₀** | Correct — true negative | Type II error (β) — false negative |

- **α (significance level):** Probability of Type I error — typically 0.05 (5%)
- **β:** Probability of Type II error — typically 0.20 (20%)
- **Power (1 − β):** Probability of correctly rejecting a false H₀ — typically 0.80 (80%)

### One-Tailed vs Two-Tailed Tests

| Test | H₁ | Use When |
|---|---|---|
| Two-tailed | μ ≠ μ₀ | Testing if there's any difference |
| One-tailed (right) | μ > μ₀ | Testing if something is greater |
| One-tailed (left) | μ < μ₀ | Testing if something is less |

### Example: Comparing Two Methods

**Question:** Does TDD reduce defect density compared to traditional development?

```
H₀: μ_TDD = μ_traditional (no difference)
H₁: μ_TDD < μ_traditional (TDD reduces defects)

Sample: TDD group mean = 2.3, Traditional group mean = 4.7
t-statistic = (4.7 - 2.3) / SE = 3.45
p-value = 0.002

Since p < 0.05, reject H₀. TDD significantly reduces defect density.
```

### p-value Interpretation

| p-value | Evidence Against H₀ |
|---|---|
| p > 0.10 | No evidence |
| 0.05 < p < 0.10 | Weak evidence |
| 0.01 < p < 0.05 | Moderate evidence |
| 0.001 < p < 0.01 | Strong evidence |
| p < 0.001 | Very strong evidence |

> **Warning:** p < 0.05 does NOT mean the effect is large or important. A tiny effect with a large sample can have p < 0.001. Always report effect sizes alongside p-values.

## Correlation and Regression

### Correlation

**Pearson correlation coefficient (r):** Measures the strength of a linear relationship between two variables.

$$r = \frac{\sum(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum(x_i - \bar{x})^2 \cdot \sum(y_i - \bar{y})^2}}$$

| r value | Interpretation |
|---|---|
| 0.0 – 0.3 | Weak or no correlation |
| 0.3 – 0.7 | Moderate correlation |
| 0.7 – 1.0 | Strong correlation |
| Negative | Inverse relationship |
| Positive | Direct relationship |

> **Correlation ≠ causation.** Two variables can be highly correlated without one causing the other. A confounding variable may be driving both.

### Linear Regression

**Model:** $y = \beta_0 + \beta_1 x + \varepsilon$

- $\beta_0$ — intercept (value of y when x=0)
- $\beta_1$ — slope (change in y per unit change in x)
- $\varepsilon$ — error term (unexplained variation)

**R² (coefficient of determination):** Proportion of variance in y explained by x.

| R² | Interpretation |
|---|---|
| 0.0 | x explains none of the variance in y |
| 0.5 | x explains 50% of the variance |
| 1.0 | x explains all of the variance |

**Example:**
```
Defects = 1.2 + 0.003 × LOC
R² = 0.72

Interpretation: Each additional 1000 LOC adds ~3 defects.
LOC explains 72% of the variance in defect count.
```

### Correlation vs Regression

| Aspect | Correlation | Regression |
|---|---|---|
| **Purpose** | Measure relationship strength | Predict y from x |
| **Output** | r (−1 to +1) | Equation (y = a + bx) |
| **Direction** | Symmetric (x↔y same r) | Asymmetric (x predicts y) |
| **Causation** | No | No (despite the equation) |

## Essential Concepts

- **Sample must be representative** — drawn using probability sampling (known selection probabilities)
- **Larger samples → narrower confidence intervals** — more data → more precision
- **Power depends on effect size, α, and sample size** — to detect small effects, you need large samples
- **Always report effect sizes** — p-values tell you if an effect exists; effect sizes tell you how big it is
- **Check assumptions** — normality, independence, equal variance (for t-tests)
- **Multiple testing correction** — running many tests inflates Type I error; use Bonferroni or FDR correction

## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[02 SWE Process/21_Measurement_Theory|21_Measurement_Theory]] — Measurement scales and GQM
- [[02 SWE Process/22_Empirical_Methods|22_Empirical_Methods]] — Designed experiments using these statistical methods
- [[01 Physics and Math/09_Math_Stats_and_Economics|09_Math_Stats_and_Economics]] — Basic probability and statistics from Moaveni
