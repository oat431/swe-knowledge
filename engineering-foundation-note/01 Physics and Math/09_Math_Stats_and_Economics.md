---
title: Mathematics, Statistics, and Engineering Economics
tags:
  - mathematics
  - statistics
  - economics
  - engineering
source: Engineering Fundamentals Ch 18-20
---

# Mathematics, Statistics, and Engineering Economics

## 1. Mathematics in Engineering (Ch 18)

Engineering problems are mathematical models of physical situations. They may take the form of linear models, nonlinear models, differential equations, integrals, or systems of algebraic equations solved simultaneously.

### 1.1 Mathematical Symbols and Greek Alphabet

Mathematics has its own symbols and terminology. Greek alphabetic characters are commonly used to express angles, dimensions, and physical variables in drawings and equations (e.g., α, β, γ, θ, π, σ, ω). Familiarity with the Greek alphabet is essential for effective engineering communication.

### 1.2 Linear Models

Linear models are the simplest form of equations describing engineering situations. The general form of a linear equation:

$$y = ax + b$$

where *a* is the slope (Δy/Δx) and *b* is the y-intercept.

**Examples of linear models in engineering:**

- **Hooke's Law (Spring):** F = kx — spring force is directly proportional to deformation. The slope *k* is the spring constant (N/mm or lb/in).
- **Temperature Distribution Across a Plane Wall:** T(x) = T₂ + [(T₁ - T₂)/L] × x — temperature varies linearly through the wall thickness.
- **Temperature Scale Conversion:** T(°F) = (9/5)T(°C) + 32 — the relationship between Fahrenheit and Celsius scales.
- **Electrical Resistance:** R = ρl/A — for constant resistivity and cross-section, resistance is linear with length.

**Key characteristics of linear models:**
- The slope is always constant
- A horizontal line has slope = 0
- A vertical line has undefined slope

**Linear Interpolation:** Used to estimate values from a table when exact entries are not available. Assumes linear change between known data points.

**Systems of Linear Equations:** Formulations that lead to sets of linear equations solved simultaneously, e.g., 2x + 4y = 10 and 4x + y = 6.

### 1.3 Nonlinear Models

Nonlinear models predict actual engineering relationships more accurately than linear models. They have variable slopes — the change in the dependent variable depends on where in the range the change is introduced.

**Examples:**

- **Laminar Fluid Velocity in a Pipe:** u(r) = V_c[1 - (r/R)²] — a second-order polynomial (parabolic velocity profile)
- **Stopping Sight Distance (AASHTO):** S = V²/[2g(f ± G)] + TV — a second-order polynomial model used by civil engineers for roadway design
- **Beam Deflection:** y = (-wx²/24EI)(x² - 4Lx + 6L²) — a fourth-order polynomial for cantilever beam deflection

**General polynomial function:**
$$y = f(x) = a_0 + a_1x + a_2x^2 + a_3x^3 + \cdots + a_nx^n$$

**Key characteristics:**
- Variable slopes (draw tangent line to visualize slope at a point)
- Real roots are x-axis intersections
- Functions may have imaginary roots (no real x-axis intersection)

**Other nonlinear models:**
- **Projectile trajectory** under constant deceleration
- **Power consumption** for resistive elements
- **Drag force** and air resistance

### 1.4 Exponential and Logarithmic Models

**Exponential functions** are critical for modeling growth, decay, and transient processes.

**Simple exponential form:** f(x) = e^x, where e ≈ 2.718281

**Key characteristic:** The value of the dependent variable begins to level off as the independent variable increases.

**Applications:**
- **Cooling of Steel Plates (Annealing):** (T - T_env)/(T_initial - T_env) = exp[-2h/(ρcL)]t — models exponential temperature decay
- **Probability distributions:** f(x) = e^(-x²) produces a bell-shaped curve

**Growth form:** f(x) = f₀ + a₀·e^(a₁·x) — exponential growth
**Decay form:** f(x) = f₀ - a₀·e^(-a₁·x) — exponential decay (levels off toward f₀)

**Logarithmic functions** are the inverse of exponential functions:
- If 10^x = y, then log y = x (common logarithm, base 10)
- If e^x = y, then ln y = x (natural logarithm, base e)
- Relationship: ln x = 2.302585 × log x

**Logarithmic identities:**
- log(xy) = log x + log y
- log(x/y) = log x - log y
- log(x^n) = n·log x

**Decibel Scale:** Sound loudness expressed as dB = 20 log(I/20 μPa), where I is pressure change in μPa.

### 1.5 Matrix Algebra

Matrix algebra is essential for formulating and solving systems of linear equations from engineering problems (vibrations, structural deflections, circuit analysis, pipe networks).

**Basic definitions:**
- **Scalar:** A quantity with a single value (e.g., temperature, time)
- **Vector:** A quantity with both magnitude and direction (e.g., velocity)
- **Matrix:** An array of numbers, variables, or mathematical terms; size defined by rows × columns

**Types of matrices:**
- **Square matrix:** Same number of rows and columns
- **Diagonal matrix:** Elements only along the principal diagonal; zeros elsewhere
- **Identity (unit) matrix:** Diagonal matrix with all diagonal elements = 1
- **Column matrix:** One column, multiple rows
- **Row matrix:** One row, multiple columns

**Matrix operations:**
- **Addition/Subtraction:** Only for matrices of the same size; element-wise operation
- **Scalar multiplication:** Multiply each element by the scalar
- **Matrix multiplication:** [A]_(m×n) × [B]_(n×p) = [C]_(m×p); number of columns in first matrix must equal number of rows in second

### 1.6 Calculus

Calculus is divided into two areas: differential and integral calculus.

#### Differential Calculus

Deals with the rate of change — how a variable changes with respect to another.

**Definition of derivative:**
$$f'(x) = \frac{df}{dx} = \lim_{h \to 0} \frac{f(x+h) - f(x)}{h}$$

**Key differentiation rules:**

| Rule | Formula |
|------|---------|
| Constant | If f(x) = constant, f'(x) = 0 |
| Power Rule | If f(x) = x^n, f'(x) = nx^(n-1) |
| Constant × Function | f'(x) = a·g'(x) |
| Sum/Difference | f'(x) = g'(x) ± h'(x) |
| Product Rule | f'(x) = g'(x)h(x) + g(x)h'(x) |
| Quotient Rule | f'(x) = [h(x)g'(x) - g(x)h'(x)] / [h(x)]² |
| Chain Rule | f'(x) = (df/du)(du/dx) |
| Power Rule (general) | f'(x) = n·u^(n-1)·(du/dx) |
| Natural Log | If f(x) = ln|g(x)|, f'(x) = g'(x)/g(x) |
| Exponential | If f(x) = e^(g(x)), f'(x) = g'(x)·e^(g(x)) |

#### Integral Calculus

Related to summation or addition of things. The integral sign ∫ is a big "S" for summation.

**Applications:**
- **Second Moment of Area (Area Moment of Inertia):** I_y-y = ∫x²dA — measures resistance to bending. For a rectangle: I = wh³/12
- **Hydrostatic Force on a Dam:** Net Force = ∫dF = ∫ρgy·dA = ½ρgwH² — integrating pressure over area to find total force

**Basic integral rules:**

| Rule | Formula |
|------|---------|
| Constant | ∫a·dx = ax + C |
| Power | ∫x^n dx = x^(n+1)/(n+1) + C (n ≠ -1) |
| Reciprocal | ∫(a/x)dx = a·ln\|x\| + C |
| Exponential | ∫e^(ax)dx = (1/a)e^(ax) + C |
| Constant multiple | ∫a·f(x)dx = a·∫f(x)dx |
| Sum/Difference | ∫[f(x) ± g(x)]dx = ∫f(x)dx ± ∫g(x)dx |
| Substitution (power) | ∫[u(x)]^n · u'(x)dx = [u(x)]^(n+1)/(n+1) + C |
| Substitution (exp) | ∫e^(u(x))·u'(x)dx = e^(u(x)) + C |

### 1.7 Differential Equations

Differential equations contain derivatives of functions and represent the balance of mass, force, energy, etc.

- **Boundary conditions:** Information about what happens at the boundaries of a problem
- **Initial conditions:** The state of a system at time t = 0, before a disturbance

**Examples of engineering differential equations:**

1. **Beam Deflection:** EI·(d²Y/dX²) = (wX/2)(L - X), with boundary conditions Y(0) = 0 and Y(L) = 0
2. **Oscillation of an Elastic System:** d²y/dt² + ω²_n·y = 0, with initial conditions y(0) = y₀ and dy/dt(0) = 0
3. **Temperature Distribution Along a Fin:** d²T/dX² - (h_p/k_A)(T - T_air) = 0, with boundary conditions at base and tip

---

## 2. Probability and Statistics in Engineering (Ch 19)

Statistical models are used by engineers to address quality control, reliability, failure analysis, signal processing, and experimental design.

### 2.1 Probability — Basic Ideas

- **Trial:** Each repetition of an experiment
- **Outcome:** The result of an experiment
- **Random experiment:** One with outcomes that cannot be predicted exactly
- **Probability:** p = m/n, where m is the number of times an outcome occurs in n trials

**Odds:** If probability of an event is p, the odds in favor = p/(1-p). If odds are x to y, then p = x/(x+y).

### 2.2 Statistics — Basic Ideas

Statistics deals with collection, organization, analysis, and interpretation of data.

- **Population:** All data pertaining to a situation or problem
- **Sample:** A subset of the population selected for analysis
- A sample must represent the characteristics of the population

### 2.3 Frequency Distributions

**Grouped frequency distribution:** Organizing data into equal intervals or ranges.

**Histogram (bar graph):** Bar height shows frequency of data within ranges.

**Cumulative frequency:** Shows the cumulative number of observations up to and including those in a given range. Can be displayed as a cumulative frequency histogram or polygon.

### 2.4 Measures of Central Tendency and Variation

#### Mean (Arithmetic Average)

For raw data:
$$\bar{x} = \frac{1}{n}\sum_{i=1}^{n} x_i$$

For grouped distribution:
$$\bar{x} = \frac{\sum xf}{n}$$

where x = midpoints of ranges, f = frequency, n = Σf = total data points.

#### Deviation from Mean

$$d_i = x_i - \bar{x}$$

The sum of deviations from the mean is always zero: Σd_i = 0.

#### Variance

$$s^2 = \frac{\sum(x_i - \bar{x})^2}{n - 1}$$

Uses n-1 (instead of n) to obtain conservative values for limited experimental trials.

#### Standard Deviation

$$s = \sqrt{\frac{\sum(x_i - \bar{x})^2}{n - 1}}$$

For grouped distribution:
$$s = \sqrt{\frac{\sum(x - \bar{x})^2 f}{n - 1}}$$

**Interpretation:**
- Small standard deviation → data bunched near the mean
- Large standard deviation → data more spread out
- For normal distribution: ~68% of data falls within ±1σ, ~95% within ±2σ, ~99.7% within ±3σ

### 2.5 Normal Distribution

A probability distribution with a bell-shaped curve. The shape is determined by its mean and standard deviation.

**Standard normal distribution:** Mean = 0, standard deviation = 1.

**Z-score (standardization):**
$$z = \frac{x - \bar{x}}{s}$$

z represents the number of standard deviations from the mean.

**Using the standard normal table (Table 19.11):**
- z = 1 → A = 0.3413 (34.13% of area between mean and z=1)
- z = 2 → A = 0.4772 (47.72% of area between mean and z=2)
- z = 3 → A = 0.4987 (49.87% of area between mean and z=3)

**Probability calculations:**
- Probability between two values: find z-scores, look up areas, add them
- Probability greater than a value: find z-score, look up area, subtract from 0.5
- Probability less than a value: find z-score, look up area, add to 0.5

**Observation errors:**
- **Systematic (fixed) errors:** From inaccurate instruments; detected by calibration
- **Random errors:** From unpredictable variations (vibrations, voltage fluctuations, humidity)

---

## 3. Engineering Economics (Ch 20)

Economic factors play vital roles in engineering design decision-making. Companies design products not only to improve lives but also to be profitable.

### 3.1 Cash Flow Diagrams

Visual aids showing the flow of costs and revenues over time. They indicate:
- **Timing** of cash flow
- **Magnitude** of cash flow
- **Direction:** costs (downward arrows) vs. revenues (upward arrows)

### 3.2 Simple and Compound Interest

**Simple Interest:**
$$F = P(1 + ni)$$

Interest is paid only on the initial principal; accumulated interest does not earn interest.

**Compound Interest:**
The interest paid on the initial principal also collects interest itself. The balance grows exponentially.

| Year | Balance at Start | Interest (6%) | Balance at End |
|------|-----------------|---------------|----------------|
| 1 | $100.00 | $6.00 | $106.00 |
| 2 | $106.00 | $6.36 | $112.36 |
| 3 | $112.36 | $6.74 | $119.10 |

### 3.3 Future Worth of a Present Amount

**Annual compounding:**
$$F = P(1 + i)^n$$

**Compounding m times per year:**
$$F = P\left(1 + \frac{i}{m}\right)^{nm}$$

where P = principal, i = interest rate, n = number of years, m = compounding periods per year.

### 3.4 Effective Interest Rate

The stated (nominal) interest rate vs. the actual (effective) rate:

$$i_{eff} = \left(1 + \frac{i}{m}\right)^m - 1$$

| Compounding | Effective Rate (6% nominal) |
|-------------|---------------------------|
| Annually | 6.00% |
| Semiannually | 6.09% |
| Quarterly | 6.13% |
| Monthly | 6.16% |
| Daily | 6.18% |

### 3.5 Present Worth of a Future Amount

$$P = \frac{F}{(1 + i)^n}$$

Determines how much to deposit today to have a desired amount in the future.

### 3.6 Present Worth of Series Payment (Annuity)

**Annual payments:**
$$P = A\left[\frac{(1 + i)^n - 1}{i(1 + i)^n}\right]$$

Rearranged to solve for A:
$$A = P\left[\frac{i(1 + i)^n}{(1 + i)^n - 1}\right]$$

**Payments m times per year:**
$$P = A\left[\frac{(1 + i/m)^{nm} - 1}{(i/m)(1 + i/m)^{nm}}\right]$$

### 3.7 Future Worth of Series Payment

**Annual payments:**
$$F = A\left[\frac{(1 + i)^n - 1}{i}\right]$$

**Solving for A (sinking fund):**
$$A = F\left[\frac{i}{(1 + i)^n - 1}\right]$$

**Payments m times per year:**
$$F = A\left[\frac{(1 + i/m)^{nm} - 1}{i/m}\right]$$

When the frequency of uniform series differs from the compounding frequency, calculate i_eff to match the payment frequency first.

### 3.8 Key Equations Summary

| Relationship | Equation |
|-------------|----------|
| Simple interest future value | F = P(1 + ni) |
| Compound interest future value | F = P(1 + i)^n |
| Compound (m periods/year) | F = P(1 + i/m)^(nm) |
| Effective interest rate | i_eff = (1 + i/m)^m - 1 |
| Present worth of future | P = F/(1 + i)^n |
| Annuity → Present worth | P = A[(1+i)^n - 1] / [i(1+i)^n] |
| Annuity → Future worth | F = A[(1+i)^n - 1] / i |
| Sinking fund | A = F·i / [(1+i)^n - 1] |

---

## Summary

- **Mathematics** provides the language and tools for engineering: linear/nonlinear models, matrix algebra, calculus (differentiation and integration), and differential equations
- **Probability and statistics** enable engineers to analyze data, assess quality, predict outcomes, and make informed decisions under uncertainty
- **Engineering economics** provides the framework for evaluating costs, revenues, interest, and financial feasibility of engineering projects


## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[01 Physics and Math/01_Dimensions_and_Measurement|01_Dimensions_and_Measurement]] — Units and dimensional analysis
- [[01 Physics and Math/03_Force_and_Mechanics|03_Force_and_Mechanics]] — Applied mechanics calculations
- [[01 Physics and Math/06_Energy_and_Power|06_Energy_and_Power]] — Energy calculations
