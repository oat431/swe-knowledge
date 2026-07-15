---
title: "35 Approximation Algorithms"
tags:
  - approximation
  - vertex-cover
  - tsp
  - set-cover
  - clrs
source: CLRS
---

# 35 · Approximation Algorithms

> When exact solutions are intractable (NP-hard), approximation algorithms provide near-optimal solutions with provable guarantees.

> NP-complete problems are unlikely to have polynomial-time exact solutions. **Approximation algorithms** run in polynomial time and guarantee solutions within a known factor of optimal.

### Approximation Ratio

For a minimization problem with optimal value $\text{OPT}$ and algorithm output $C$:

$$\rho(n) = \max\left(\frac{C}{\text{OPT}},\; \frac{\text{OPT}}{C}\right)$$

- $\rho(n) = 1$: optimal solution
- $\rho(n) \leq \alpha$: algorithm is an **$\alpha$-approximation**
- Closer to 1 is better

An approximation scheme takes an additional parameter $\epsilon > 0$ and produces a $(1 + \epsilon)$-approximation. A **polynomial-time approximation scheme (PTAS)** runs in polynomial time for fixed $\epsilon$.

---

### 35.1 Vertex-Cover — 2-Approximation

**Problem:** Find a minimum-size vertex cover in graph $G = (V, E)$.

```
APPROX-VERTEX-COVER(G):
    C = ∅
    E' = E.copy()
    while E' ≠ ∅:
        pick any edge (u, v) ∈ E'
        C = C ∪ {u, v}
        remove all edges incident to u or v from E'
    return C
```

**Analysis:**

- The algorithm picks a set of **edges** that form a **matching** (no two share an endpoint, since we remove all incident edges).
- Let $A$ = set of edges picked. $|A| = |C|/2$ (each edge contributes 2 vertices to $C$).
- Any vertex cover must include at least one endpoint of each edge in $A$.
- Since edges in $A$ are disjoint (matching): $|\text{OPT}| \geq |A| = |C|/2$.
- Therefore: $|C| \leq 2 \cdot |\text{OPT}|$.

**Approximation ratio: 2** — the cover is at most twice the optimal size.

**Example of tightness:** A star graph $K_{1,n}$ — the algorithm picks the center + one leaf (picking the edge), while the optimal is just the center. Ratio → 2.

---

### 35.2 Traveling Salesman Problem — 2-Approximation (Metric TSP)

**Metric TSP:** Distances satisfy the triangle inequality: $d(u, w) \leq d(u, v) + d(v, w)$.

```
APPROX-TSP(G, w):
    select root vertex r
    compute MST T of G rooted at r      // Prim or Kruskal: O(E log V)
    let H = preorder walk of T           // DFS traversal
    return H
```

**Analysis:**

1. **MST cost ≤ OPT:** Removing one edge from an optimal tour gives a spanning tree, so $\text{cost}(T) \leq \text{cost}(\text{OPT})$.
2. **Tour cost ≤ 2 × MST cost:** The preorder walk traverses each MST edge exactly twice (down and up), so $\text{cost}(H) \leq 2 \cdot \text{cost}(T)$.
3. **Shortcutting:** By triangle inequality, skipping already-visited vertices can only decrease or maintain cost.

$$\text{cost}(H) \leq 2 \cdot \text{cost}(T) \leq 2 \cdot \text{cost}(\text{OPT})$$

**Approximation ratio: 2** for metric TSP.

**Note:** General TSP (no triangle inequality) has **no** constant-factor approximation unless P = NP.

---

### 35.3 Set Cover — Greedy $O(\ln n)$-Approximation

**Problem:** Given a universe $U$ and a family $\mathcal{F}$ of subsets, find the minimum number of sets from $\mathcal{F}$ that cover $U$.

```
GREEDY-SET-COVER(U, F):
    C = ∅; U' = U
    while U' ≠ ∅:
        select S ∈ F that maximizes |S ∩ U'|
        U' = U' \ S
        C = C ∪ {S}
    return C
```

**Analysis:**

- Assign costs to elements: when set $S$ covers $k$ new elements, each gets cost $|S|/k$ (paid equally).
- Total cost = $\sum_{x \in U} \text{cost}(x) = \sum_{S \in C_{\text{greedy}}} |S|$, which equals the total cost of the greedy solution.
- The $i$-th set chosen covers $\geq$ the average number of uncovered elements per remaining set. If $k$ sets remain and $|U'|$ elements are uncovered, the chosen set covers $\geq |U'|/k$ elements.
- The harmonic sum argument gives:

$$\text{cost}(C_{\text{greedy}}) \leq H(\max_i |S_i|) \cdot |\text{OPT}| \leq (\ln n + 1) \cdot |\text{OPT}|$$

where $H(d) = 1 + 1/2 + 1/3 + \cdots + 1/d$ is the harmonic number and $n = |U|$.

**Approximation ratio: $H(n) = O(\ln n)$** — essentially tight, since set cover is hard to approximate within $c \ln n$ for some constant $c > 0$.

---

### 35.4 Subset Sum — Fully Polynomial-Time Approximation Scheme (FPTAS)

**Problem:** Given set $S = \{w_1, \ldots, w_n\}$ and target $t$, find the largest subset sum ≤ $t$.

**Exact solution:** Dynamic programming in $O(nt)$ time (pseudo-polynomial).

**FPTAS idea:** Round/truncate large values to reduce the range.

```
APPROX-SUBSET-SUM(S, t, ε):
    n = |S|
    L₀ = {0}
    for i = 1 to n:
        Lᵢ = MERGE-LISTS(Lᵢ₋₁, Lᵢ₋₁ + wᵢ)
        trim Lᵢ to remove values within factor (1 + ε/n) of each other
        remove all values > t
    return largest value in Lₙ
```

**Trimming:** After merging, keep at most one representative from each group of values in $[(1+\epsilon/n)^j,\; (1+\epsilon/n)^{j+1})$. This bounds $|L_i| = O(n/\epsilon \cdot \log t)$.

**Analysis:**

- After trimming, the largest remaining value $\hat{z}$ satisfies: $z^*/(1+\epsilon) \leq \hat{z} \leq z^*$ where $z^*$ is the exact answer.
- **Running time:** $O(n^3/\epsilon)$ — polynomial in $n$ and $1/\epsilon$.

**Approximation ratio: $1 + \epsilon$** for any $\epsilon > 0$.

This is a **FPTAS** (fully polynomial-time approximation scheme): polynomial in both $n$ and $1/\epsilon$. Stronger than a PTAS (which need only be polynomial in $n$ for fixed $\epsilon$).

---

## Summary: The Big Picture

| Category | Problem | Algorithm | Time | Quality |
|---|---|---|---|---|
| **Geometry** | Segment intersection | Cross-product test | $O(1)$ | Exact |
| | Any-segment-intersection | Sweep line + RB-tree | $O(n \log n)$ | Exact |
| | Convex hull | Graham's scan | $O(n \log n)$ | Exact |
| | Convex hull | Jarvis's march | $O(nh)$ | Exact |
| | Closest pair | Divide & conquer | $O(n \log n)$ | Exact |
| **NP-Complete** | SAT, 3-CNF-SAT | Reductions | — | NP-complete |
| | CLIQUE | 3-CNF-SAT reduction | — | NP-complete |
| | VERTEX-COVER | CLIQUE reduction | — | NP-complete |
| | HAM-CYCLE | VC reduction | — | NP-complete |
| | TSP | HAM-CYCLE reduction | — | NP-complete |
| | SUBSET-SUM | 3-CNF-SAT reduction | — | NP-complete |
| **Approximation** | Vertex cover | Maximal matching | $O(V + E)$ | 2-approx |
| | Metric TSP | MST + preorder walk | $O(E \log V)$ | 2-approx |
| | Set cover | Greedy (max coverage) | $O(|\mathcal{F}| \cdot |U|)$ | $\ln n$-approx |
| | Subset sum | Trimmed DP | $O(n^3/\epsilon)$ | $(1+\epsilon)$-approx (FPTAS) |

---

## Key Takeaways

1. **Computational geometry** relies on cross products for $O(1)$ orientation tests — no division or trig needed. Sweep-line techniques reduce many problems to $O(n \log n)$.

2. **NP-completeness** is established via **polynomial-time reductions**. The chain CIRCUIT-SAT → SAT → 3-CNF-SAT → many problems forms the backbone. If any NP-complete problem is in P, then P = NP.

3. **Approximation algorithms** provide practical guarantees for NP-hard problems:
   - Simple greedy often gives logarithmic factors (set cover)
   - Structural properties (triangle inequality, matching) give constant factors
   - Trimming/packing techniques yield FPTAS for subset sum

4. **The trade-off:** Exact algorithms for geometric problems are efficient ($O(n \log n)$). For NP-complete problems, we settle for approximation — the quality of the approximation depends on problem structure.

---

## See Also

- [[01_Amortized_Analysis]] — amortized analysis used in convex hull correctness proof
- [[01 Heaps & Priority Queues]] — heap-based sorting for convex hull vertex ordering
- [[03_Advanced_Graph_Algorithms]] — Prim's algorithm for MST in TSP approximation


---

## Hands-On Exercises

### Exercise 5: Approximation Algorithm — Vertex Cover
Given graph:
```
A -- B -- C
|         |
D -- E -- F
```

Edges: (A,B), (B,C), (C,F), (F,E), (E,D), (D,A).

1. Run the 2-approximation algorithm:
   - Pick an edge, add both endpoints, remove all incident edges.
   - Repeat until no edges remain.
2. What is your vertex cover? How many vertices?
3. What is the optimal vertex cover? (Try to find a smaller one.)
4. Verify: `|your cover| ≤ 2 × |optimal cover|`.

---


### Exercise 6: Approximation — Metric TSP
Given complete graph with 4 cities and distances satisfying triangle inequality:

```
     A ---3--- B
     |         |
     4         2
     |         |
     D ---5--- C
```

Plus diagonal: A-C = 4, B-D = 6.

1. Find the MST using Prim's (start from A).
2. Do a preorder walk of the MST.
3. Apply shortcutting (skip already-visited vertices) to get a tour.
4. What is the tour cost? Compare to the optimal tour cost.
5. Verify: `tour cost ≤ 2 × MST cost ≤ 2 × OPT`.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 6 | **Implement 2-Approx Vertex Cover** | 🟡 Code | Approximation |
| 8 | **Prove Set Cover ≤_P Dominating Set** | 🔴 Theory | NP reduction |

### Assignment Guidelines
- **Problem 1**: Implement the 2-approximation vertex cover from the note.
- **Problem 2**: Implement TSP approximation using MST.
- **Problem 3**: Prove the ln(n) bound using the harmonic sum argument.
- **Target time:** 30 min per code, 40 min per theory.