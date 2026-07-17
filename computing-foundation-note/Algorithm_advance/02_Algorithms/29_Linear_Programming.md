---
tags: [linear-programming, optimization, clrs, simplex, duality, algorithms]
---

# 04 — Linear Programming

> *Source: CLRS Chapter 29 — Linear Programming*

## Purpose

Linear programming is the cornerstone of operations research. Many real-world problems — from political campaign strategy to airline scheduling, from network flow to combinatorial optimization — can be modeled as linear programs.

## Overview & Applications

| Domain | Problem |
|---|---|
| Political campaigns | Minimize ad spend to win enough votes under budget constraints |
| Airline scheduling | Cover all flights with minimum crew under labor regulations |
| Oil exploration | Maximize expected oil yield under budget constraints |
| Network flow | Max flow, min-cost flow, multicommodity flow |
| Shortest paths | Can be modeled as LP (see §29.2) |

### Algorithm Comparison

| Algorithm | Time Complexity | Practical Efficiency |
|---|---|---|
| **Simplex** | Exponential worst case | Usually fast in practice |
| **Ellipsoid** | Polynomial | Slow in practice |
| **Interior-point** | Polynomial | Competitive with Simplex for large-scale problems |

> **Note:** Integer Linear Programming (ILP) is NP-hard — no known polynomial-time algorithm.

---

## 1. Standard Form

Standard form is the canonical representation of linear programs.

### Mathematical Definition

Given:
- $n$ real numbers $c_1, c_2, \ldots, c_n$ (objective function coefficients)
- $m$ real numbers $b_1, b_2, \ldots, b_m$ (constraint right-hand sides)
- $mn$ real numbers $a_{ij}$ (constraint coefficient matrix)

Find $n$ real numbers $x_1, x_2, \ldots, x_n$:

$$\text{maximize} \quad \sum_{j=1}^{n} c_j x_j$$

$$\text{subject to} \quad \sum_{j=1}^{n} a_{ij} x_j \le b_i, \quad i = 1, 2, \ldots, m$$

$$x_j \ge 0, \quad j = 1, 2, \ldots, n$$

### Matrix Form

$$\text{maximize} \quad \mathbf{c}^T \mathbf{x}$$
$$\text{subject to} \quad A\mathbf{x} \le \mathbf{b}, \quad \mathbf{x} \ge \mathbf{0}$$

A linear program is fully specified by the triple $(A, \mathbf{b}, \mathbf{c})$.

### Terminology

| Term | Definition |
|---|---|
| **Feasible solution** | Variable assignment satisfying all constraints |
| **Infeasible solution** | Violates at least one constraint |
| **Objective value** | $\mathbf{c}^T \hat{\mathbf{x}}$ |
| **Optimal solution** | Feasible solution with maximum objective value |
| **Infeasible** | No feasible solution exists |
| **Unbounded** | Feasible solutions exist, but objective can increase without bound |

### Converting to Standard Form

Any LP can be converted to standard form:

1. **Minimization → Maximization:** Negate the objective coefficients
   - $\min f(\mathbf{x})$ is equivalent to $\max -f(\mathbf{x})$

2. **Variables without non-negativity constraints:** Replace $x_j = x_j' - x_j''$ where $x_j', x_j'' \ge 0$

3. **Equality constraints:** $f(\mathbf{x}) = b$ splits into $f(\mathbf{x}) \le b$ and $f(\mathbf{x}) \ge b$

4. **Greater-than-or-equal constraints:** $\sum a_{ij} x_j \ge b_i$ multiply by $-1$ to get $\sum (-a_{ij}) x_j \le -b_i$

---

## 2. Slack Form

Slack form converts inequality constraints to equalities — the working form for the Simplex algorithm.

### From Standard to Slack Form

For each inequality constraint, introduce a **slack variable** $x_{n+i}$:

$$\sum_{j=1}^{n} a_{ij} x_j \le b_i \quad \Longleftrightarrow \quad x_{n+i} = b_i - \sum_{j=1}^{n} a_{ij} x_j, \quad x_{n+i} \ge 0$$

### Slack Form Structure

$$z = \nu + \sum_{j \in N} c_j x_j$$
$$x_i = b_i - \sum_{j \in N} a_{ij} x_j, \quad \text{for } i \in B$$

Where:
- $N$ — index set of **nonbasic variables**
- $B$ — index set of **basic variables**
- $|N| = n, \; |B| = m, \; N \cup B = \{1, 2, \ldots, n+m\}$
- $\nu$ — constant term of the objective function

> **Key note:** $a_{ij}$ in slack form are the **negatives** of the actual coefficients in the constraint.

### Example

Standard form:
```
maximize  2x₁ - x₂ + x₃
subject to
  x₁ + x₂ + x₃ ≤ 7
  x₁ - x₂ + x₃ ≤ 7
  -x₁ + 2x₂ - 2x₃ ≤ 4
  x₁, x₂, x₃ ≥ 0
```

Slack form:
```
z = 2x₁ - x₂ + x₃
x₄ = 7 - x₁ - x₂ - x₃
x₅ = 7 - x₁ + x₂ - x₃
x₆ = 4 + x₁ - 2x₂ + 2x₃
```

$B = \{4, 5, 6\}$, $N = \{1, 2, 3\}$

---

## 3. Simplex Algorithm

### Core Idea

The Simplex algorithm is the classic method for solving LPs. It moves between **vertices** of the feasible region, improving the objective value at each iteration until reaching the optimum.

### Geometric Intuition

- The feasible region is a **convex polytope (simplex)**
- The optimal solution is always at some vertex
- Simplex moves along edges from one vertex to a better neighbor
- When all neighbors are worse, the current vertex is **globally optimal** (because the feasible region is convex)

### Basic Solution

Given a slack form, the **basic solution** is computed by:
- Setting all nonbasic variables to 0
- Basic variables: $x_i = b_i$

If all $x_i \ge 0$, it's a **basic feasible solution**.

### Iteration: The Pivot Operation

Each iteration performs one **pivot**:

1. **Choose entering variable** $x_e$: select a nonbasic variable with $c_e > 0$
2. **Choose leaving variable** $x_l$: select the basic variable that most tightly limits the increase of $x_e$
3. **Swap roles:** $x_e$ becomes basic, $x_l$ becomes nonbasic

#### Entering Variable Selection

Choose a nonbasic variable with $c_e > 0$ (if none exists, current solution is optimal).

#### Leaving Variable Selection (Minimum Ratio Test)

For each $i \in B$, if $a_{ie} > 0$, compute $\Delta_i = b_i / a_{ie}$.

Choose $l \in B$ minimizing $\Delta_l$. If all $a_{ie} \le 0$, the problem is **unbounded**.

### PIVOT Procedure

```
PIVOT(N, B, A, b, c, ν, l, e):
  // Compute new basic variable x_e's equation
  b̂_e = b_l / a_le
  for each j ∈ N - {e}:
    â_{ej} = a_{lj} / a_le
  â_{ee} = 1 / a_le

  // Update other constraints
  for each i ∈ B - {l}:
    b̂_i = b_i - a_{ie} * b̂_e
    for each j ∈ N - {e}:
      â_{ij} = a_{ij} - a_{ie} * â_{ej}
    â_{il} = -a_{ie} * â_{ee}

  // Update objective function
  ν̂ = ν + c_e * b̂_e
  for each j ∈ N - {e}:
    ĉ_j = c_j - c_e * â_{ej}
  ĉ_l = -c_e * â_{ee}

  // Update basic/nonbasic variable sets
  N̂ = (N - {e}) ∪ {l}
  B̂ = (B - {l}) ∪ {e}

  return (N̂, B̂, Â, b̂, ĉ, ν̂)
```

### SIMPLEX Main Procedure

```
SIMPLEX(A, b, c):
  (N, B, A, b, c, ν) = INITIALIZE-SIMPLEX(A, b, c)
  while ∃j ∈ N with c_j > 0:
    choose e ∈ N with c_e > 0
    for each i ∈ B:
      if a_{ie} > 0: Δ_i = b_i / a_{ie}
      else: Δ_i = ∞
    choose l ∈ B that minimizes Δ_l
    if Δ_l == ∞:
      return "unbounded"
    else: (N, B, A, b, c, ν) = PIVOT(N, B, A, b, c, ν, l, e)

  // Compute solution
  for i = 1 to n:
    if i ∈ B: x̂_i = b_i
    else: x̂_i = 0
  return (x̂_1, x̂_2, ..., x̂_n)
```

### Example Execution

Initial problem (standard form):
```
maximize  3x₁ + x₂ + 2x₃
subject to
  x₁ + x₂ + 3x₃ ≤ 30
  2x₁ + 2x₂ + 5x₃ ≤ 24
  4x₁ + x₂ + 2x₃ ≤ 36
  x₁, x₂, x₃ ≥ 0
```

| Iteration | Entering | Leaving | Basic Solution | Objective |
|---|---|---|---|---|
| 0 | $x_1$ | $x_6$ | $(0,0,0,30,24,36)$ | 0 |
| 1 | $x_3$ | $x_5$ | $(9,0,0,21,6,0)$ | 27 |
| 2 | $x_2$ | $x_4$ | $(33/4,0,3/2,69/4,0,0)$ | 111/4 |
| 3 | — | — | $(8,4,0,18,0,0)$ | **28** ✓ |

### Algorithm Analysis

- **Correctness:** If Simplex terminates and initial solution is feasible, returns feasible solution or reports unbounded
- **Objective monotonicity:** Each pivot does not decrease the objective value
- **Degeneracy:** If $b_l = 0$, objective value may not change
- **Cycling:** Degeneracy can cause infinite loops (rare, avoidable with Bland's rule)
- **Worst-case complexity:** Exponential (Klee-Minty cube construction)

---

## 4. Initialization: Finding Initial Feasible Solution

If the initial basic solution is infeasible (some $b_i < 0$), use an **auxiliary LP**.

### Auxiliary Linear Program

Given LP $L$, construct auxiliary problem $L_{aux}$:

$$\text{maximize} \quad -x_0$$
$$\text{subject to} \quad x_i = b_i - \sum_{j \in N} a_{ij} x_j - x_0, \quad i \in B$$

where $x_0$ is an artificial variable.

### INITIALIZE-SIMPLEX Procedure

```
INITIALIZE-SIMPLEX(A, b, c):
  let k be the index of the minimum b_i
  if b_k >= 0:
    return ({1,...,n}, {n+1,...,n+m}, A, b, c, 0)  // Initial solution feasible

  // Construct auxiliary LP
  Set N̂ = {0, 1, ..., n}, B̂ = {n+1, ..., n+m}
  For all i ∈ B̂: set â_{i0} = -1
  Pivot with x_0 entering, x_k leaving

  // Run SIMPLEX on auxiliary LP
  Repeat pivot until optimal

  if x̂_0 ≠ 0:
    return "infeasible"

  // Restore to original slack form
  If x_0 is basic, pivot to make it nonbasic
  Delete x_0 equations, restore original objective
  return (N, B, A, b, c, ν)
```

---

## 5. Duality

Duality is one of the deepest concepts in LP theory — every LP is associated with a "dual" program.

### Primal and Dual Problems

**Primal:**

$$\text{maximize} \quad \sum_{j=1}^{n} c_j x_j$$
$$\text{subject to} \quad \sum_{j=1}^{n} a_{ij} x_j \le b_i, \quad i = 1, \ldots, m$$
$$x_j \ge 0, \quad j = 1, \ldots, n$$

**Dual:**

$$\text{minimize} \quad \sum_{i=1}^{m} b_i y_i$$
$$\text{subject to} \quad \sum_{i=1}^{m} a_{ij} y_i \ge c_j, \quad j = 1, \ldots, n$$
$$y_i \ge 0, \quad i = 1, \ldots, m$$

Matrix form:
- Primal: $\max \mathbf{c}^T\mathbf{x}$, s.t. $A\mathbf{x} \le \mathbf{b}$, $\mathbf{x} \ge \mathbf{0}$
- Dual: $\min \mathbf{b}^T\mathbf{y}$, s.t. $A^T\mathbf{y} \ge \mathbf{c}$, $\mathbf{y} \ge \mathbf{0}$

### Fundamental Properties

#### Weak Duality Theorem

If $\hat{\mathbf{x}}$ is primal feasible and $\hat{\mathbf{y}}$ is dual feasible:

$$\sum_{j=1}^{n} c_j \hat{x}_j \le \sum_{i=1}^{m} b_i \hat{y}_i$$

**Corollaries:**
- If objective values are equal, both are optimal
- If primal is unbounded, dual is infeasible
- If dual is unbounded, primal is infeasible

#### Strong Duality Theorem

If the primal has optimal solution $\hat{\mathbf{x}}$, the dual also has optimal solution $\hat{\mathbf{y}}$, and:

$$\sum_{j=1}^{n} c_j \hat{x}_j = \sum_{i=1}^{m} b_i \hat{y}_i$$

### Complementary Slackness

Let $\hat{\mathbf{x}}$ and $\hat{\mathbf{y}}$ be primal and dual feasible solutions. The following are equivalent:

1. $\hat{\mathbf{x}}$ and $\hat{\mathbf{y}}$ are both optimal
2. For all $j$: $c_j = \sum_i a_{ij} \hat{y}_i$ or $\hat{x}_j = 0$ (or both)
3. For all $i$: $\sum_j a_{ij} \hat{x}_j = b_i$ or $\hat{y}_i = 0$ (or both)

> Intuition: If a primal variable is positive, the corresponding dual constraint must be tight. If a dual variable is positive, the corresponding primal constraint must be tight.

---

## 6. Problem Modeling Examples

### Shortest Paths

Single-source shortest paths can be modeled as LP:

$$\text{maximize} \quad d_t$$
$$\text{subject to} \quad d_v \le d_u + w(u,v), \quad \forall (u,v) \in E$$
$$d_s = 0$$

where $d_v$ is the shortest-path weight from source $s$ to $v$. Maximization is used because constraints force $d_v \le \min\{d_u + w(u,v)\}$.

### Maximum Flow

$$\text{maximize} \quad \sum_{v \in V} f_{sv} - \sum_{v \in V} f_{vs}$$
$$\text{subject to}$$
$$f_{uv} \le c(u,v), \quad \forall u, v \in V$$
$$\sum_{v \in V} f_{vu} = \sum_{v \in V} f_{uv}, \quad \forall u \in V - \{s, t\}$$
$$f_{uv} \ge 0, \quad \forall u, v \in V$$

### Minimum-Cost Flow

Adds cost objective to max flow:

$$\text{minimize} \quad \sum_{(u,v) \in E} a(u,v) f_{uv}$$
$$\text{subject to flow conservation + capacity constraints}$$
$$\sum_{v} f_{sv} - \sum_{v} f_{vs} = d \quad \text{(deliver exactly d units)}$$

### Multicommodity Flow

$k$ commodities share the same network, each with its own source $s_i$, sink $t_i$, and demand $d_i$:

$$\sum_{i=1}^{k} f_{i,uv} \le c(u,v), \quad \forall (u,v) \in E$$

Each commodity satisfies its own flow conservation.

---

## 7. Key Theorems Summary

| Result | Statement |
|---|---|
| **Vertex optimality** | If an LP has an optimal solution, some vertex is optimal |
| **Simplex correctness** | If initial feasible and terminates, returns optimal solution |
| **Weak duality** | Primal feasible ≤ Dual feasible |
| **Strong duality** | Optimal values are equal: $\max = \min$ |
| **Complementary slackness** | Optimality ⟺ complementary slackness conditions hold |
| **LP ∈ P** | Solvable in polynomial time via ellipsoid or interior-point methods |
| **ILP is NP-hard** | No known polynomial-time algorithm for integer LP |

---

## 8. Complexity & Practice

### Simplex Efficiency

- **Average case:** Very efficient in practice, iterations far fewer than constraints
- **Worst case:** Exponential (Klee-Minty cube, $2^n$ iterations)
- **Degeneracy handling:** Bland's rule (smallest index selection) prevents cycling

### When to Use LP

- Problem objective and constraints are linear
- Variables can be continuous reals
- Problem size is moderate (thousands to millions of variables/constraints)
- Exact optimal solution needed (not approximation)

### LP Relationship to Other Techniques

```
LP (continuous) ──→ ILP (integer)
  │                │
  ├─ Network flow    ├─ Scheduling
  ├─ Shortest paths  ├─ Knapsack
  ├─ Matching        ├─ Facility location
  └─ Approximation   └─ Combinatorial optimization
```

---

## 9. Core Formula Quick Reference

| Concept | Formula |
|---|---|
| Standard form | $\max \mathbf{c}^T\mathbf{x}$, s.t. $A\mathbf{x} \le \mathbf{b}$, $\mathbf{x} \ge \mathbf{0}$ |
| Slack variables | $x_{n+i} = b_i - \sum_j a_{ij} x_j$ |
| Pivot update | $\hat{b}_e = b_l / a_{le}$, $\hat{b}_i = b_i - a_{ie} \hat{b}_e$ |
| Objective update | $\hat{\nu} = \nu + c_e \hat{b}_e$ |
| Minimum ratio test | $\Delta_l = \min_{i \in B, a_{ie}>0} \{b_i / a_{ie}\}$ |
| Dual objective | $\min \mathbf{b}^T\mathbf{y}$, s.t. $A^T\mathbf{y} \ge \mathbf{c}$ |
| Weak duality | $\mathbf{c}^T\hat{\mathbf{x}} \le \mathbf{b}^T\hat{\mathbf{y}}$ |
| Strong duality | $\max = \min$ (when both have optimal solutions) |

## Related

- [[Algorithm Overview]] — Algorithm design paradigms
- [[06_Geometry_NP_and_Approximation]] — LP relaxation for approximation algorithms
- [[02 Greedy Algorithms]] — Greedy approach relates to LP duality


---

## Hands-On Exercises

### Exercise 1: Convert to Standard Form
Convert the following LP to standard form (maximize, ≤ constraints, non-negative variables):

```
minimize  2x₁ + 3x₂
subject to
  x₁ + x₂ ≥ 5
  x₁ - x₂ = 3
  x₁ ≥ 0, x₂ unrestricted
```

**Steps:**
1. Negate objective to convert min → max.
2. Replace unrestricted `x₂ = x₂' - x₂''` where `x₂', x₂'' ≥ 0`.
3. Convert ≥ constraint to ≤ by multiplying by -1.
4. Split equality into two inequalities.

Write the final standard form explicitly.

---

### Exercise 2: Convert to Slack Form
Convert the standard form LP from the note's example:

```
maximize  2x₁ - x₂ + x₃
subject to
  x₁ + x₂ + x₃ ≤ 7
  x₁ - x₂ + x₃ ≤ 7
  -x₁ + 2x₂ - 2x₃ ≤ 4
  x₁, x₂, x₃ ≥ 0
```

1. Introduce slack variables x₄, x₅, x₆.
2. Write out the slack form (objective + constraints).
3. Identify B (basic variables) and N (nonbasic variables).
4. Compute the initial basic solution.

---

### Exercise 3: Trace Simplex Iterations
For the LP:
```
maximize  3x₁ + x₂ + 2x₃
subject to
  x₁ + x₂ + 3x₃ ≤ 30
  2x₁ + 2x₂ + 5x₃ ≤ 24
  4x₁ + x₂ + 2x₃ ≤ 36
  x₁, x₂, x₃ ≥ 0
```

Trace the first pivot:
1. Convert to slack form. Identify B, N, b, c, ν.
2. **Entering variable:** Which nonbasic variable has `c_e > 0`? (Choose any.)
3. **Leaving variable:** Apply the minimum ratio test — compute `Δᵢ = bᵢ / aᵢₑ` for each basic variable where `aᵢₑ > 0`.
4. Perform the pivot. Write the new slack form.
5. What is the new basic solution and objective value?

---

### Exercise 4: Construct the Dual
For each primal LP, write the dual:

**Primal A:**
```
maximize  5x₁ + 3x₂
subject to
  x₁ + x₂ ≤ 4
  2x₁ + x₂ ≤ 6
  x₁, x₂ ≥ 0
```

**Primal B:**
```
minimize  4y₁ + 6y₂
subject to
  y₁ + 2y₂ ≥ 5
  y₁ + y₂ ≥ 3
  y₁, y₂ ≥ 0
```

For each: write the dual, then verify **weak duality** — plug in any feasible primal solution and any feasible dual solution, and confirm primal objective ≤ dual objective.

---

### Exercise 5: Model a Real Problem as LP
A bakery makes cakes and cookies. Each cake requires 2 hours of baking and 1 hour of decoration, earning $15 profit. Each cookie batch requires 1 hour of baking and 1 hour of decoration, earning $8 profit. The bakery has 8 hours of baking time and 6 hours of decoration time available per day.

1. Define variables.
2. Write the objective function.
3. Write the constraints.
4. Convert to standard form.
5. **Bonus:** Solve graphically (plot the feasible region) or via Simplex.

---

## Assignments

| # | Problem | Type | Key Technique |
|---|---------|------|---------------|
| 1 | **Convert LP to Standard/Slack Form** (3 problems) | 🟡 Theory | Form conversion |
| 2 | **Trace Simplex on a 3-variable LP** | 🟡 Theory | Pivot operations |
| 3 | **Construct and Verify Duals** (3 problems) | 🟡 Theory | Duality |
| 4 | **Model Diet Problem as LP** | 🟡 Theory | LP modeling |
| 5 | **Model Network Flow as LP** | 🟡 Theory | LP modeling |
| 6 | **Prove Complementary Slackness** | 🔴 Theory | Duality theory |
| 7 | [Optimize Maximum Profit](https://leetcode.com/problems/maximum-profit-in-job-scheduling/) (LC 1235) | 🔴 Hard | DP (LP-adjacent) |
| 8 | **Implement Simplex in Java** | 🔴 Code | Full Simplex algorithm |

### Assignment Guidelines
- **Problems 1–3** are core — standard form, slack form, Simplex, duality.
- **Problems 4–5** develop LP modeling skills — the most valuable practical skill.
- **Problem 6** (Complementary Slackness) is a theoretical deep-dive.
- **Problem 8** (Implement Simplex) is a substantial project — handle pivot, minimum ratio test, and initialization.
- **Target time:** 20 min per Theory, 60 min for Simplex implementation.
