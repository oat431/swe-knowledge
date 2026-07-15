---
title: "Geometry, NP-Completeness & Approximation"
tags:
  - computational-geometry
  - np-completeness
  - approximation
  - clrs
  - algorithms
source: CLRS Chapters 33–35
---

# Geometry, NP-Completeness & Approximation

> Three pillars of advanced algorithm design: **computational geometry** gives efficient algorithms for spatial problems; **NP-completeness theory** identifies problems where efficient algorithms are unlikely to exist; and **approximation algorithms** provide near-optimal solutions when exact ones are intractable.

---

## Chapter 33 — Computational Geometry

Computational geometry studies algorithms for geometric problems in the plane (and higher dimensions). Applications span computer graphics, robotics, VLSI design, CAD, molecular modeling, and statistics.

**Representation:** Each point $p_i = (x_i, y_i) \in \mathbb{R}^2$. An $n$-vertex polygon is $P = \langle p_0, p_1, \ldots, p_{n-1} \rangle$ in counterclockwise order.

---

### 33.1 Line-Segment Properties

Three fundamental $O(1)$-time questions, all based on **cross products** (no division or trigonometry needed):

#### Cross Product

For vectors $p_1 = (x_1, y_1)$ and $p_2 = (x_2, y_2)$:

$$p_1 \times p_2 = x_1 y_2 - x_2 y_1 = \det \begin{pmatrix} x_1 & x_2 \\ y_1 & y_2 \end{pmatrix}$$

- **Positive** → $p_1$ is clockwise from $p_2$ relative to origin
- **Negative** → $p_1$ is counterclockwise from $p_2$
- **Zero** → vectors are colinear

To compare segments $\overrightarrow{p_0 p_1}$ and $\overrightarrow{p_0 p_2}$, translate $p_0$ to origin:

$$(p_1 - p_0) \times (p_2 - p_0) = (x_1 - x_0)(y_2 - y_0) - (x_2 - x_0)(y_1 - y_0)$$

#### Left/Right Turn

Given consecutive segments $p_0p_1$ and $p_1p_2$, compute $(p_2 - p_0) \times (p_1 - p_0)$:
- **Negative** → counterclockwise → **left turn**
- **Positive** → clockwise → **right turn**
- **Zero** → colinear

#### Segment Intersection

Segments $p_1p_2$ and $p_3p_4$ intersect iff:
1. Each segment **straddles** the line containing the other, OR
2. An endpoint of one segment lies on the other (boundary case)

```
SEGMENTS-INTERSECT(p1, p2, p3, p4):
    d1 = DIRECTION(p3, p4, p1)    // (p1−p3) × (p4−p3)
    d2 = DIRECTION(p3, p4, p2)    // (p2−p3) × (p4−p3)
    d3 = DIRECTION(p1, p2, p3)    // (p3−p1) × (p2−p1)
    d4 = DIRECTION(p1, p2, p4)    // (p4−p1) × (p2−p1)

    if ((d1 > 0 and d2 < 0) or (d1 < 0 and d2 > 0)) and
       ((d3 > 0 and d4 < 0) or (d3 < 0 and d4 > 0)):
        return TRUE                          // straddle each other

    // Boundary cases: endpoint colinear with other segment
    if d1 == 0 and ON-SEGMENT(p3, p4, p1): return TRUE
    if d2 == 0 and ON-SEGMENT(p3, p4, p2): return TRUE
    if d3 == 0 and ON-SEGMENT(p1, p2, p3): return TRUE
    if d4 == 0 and ON-SEGMENT(p1, p2, p4): return TRUE

    return FALSE
```

`ON-SEGMENT(p_i, p_j, p_k)` checks if $p_k$ lies between $p_i$ and $p_j$ using only comparisons (assumes colinearity).

---

### 33.2 Any-Segment-Intersection — Sweep Line

**Problem:** Given $n$ line segments, determine if **any** pair intersects.

**Technique:** Sweep line moving left to right, maintaining a **total preorder** $T$ of segments currently crossing the sweep line, ordered by $y$-coordinate at the current $x$.

**Event points:** Segment endpoints (sorted lexicographically — left before right, bottom before top).

**Data structure:** Red-black tree for $T$ (ordered by $<_x$ relation); event queue (sorted by $x$).

```
ANY-SEGMENTS-INTERSECT(S):
    T = ∅
    Sort endpoints by x-coordinate (left before right; ties: bottom before top)
    for each event point p (in sorted order):
        if p is a left endpoint:
            insert segment into T
            check intersection with above/below neighbors → return TRUE if found
        if p is a right endpoint:
            find above/below neighbors (now consecutive after deletion)
            delete segment from T
            check if new neighbors intersect → return TRUE if found
    return FALSE
```

**Running time:** $O(n \log n)$ — sorting is $O(n \log n)$, each of the $2n$ event points does $O(\log n)$ red-black tree operations plus $O(1)$ intersection tests.

**Key insight:** We never need to check non-consecutive segments — if two segments intersect, they must become consecutive at the sweep line at some point before the leftmost intersection.

---

### 33.3 Convex Hull

The **convex hull** $\text{CH}(Q)$ is the smallest convex polygon enclosing all points in $Q$. Think: rubber band around nails.

#### Graham's Scan — $O(n \log n)$

**Idea:** Maintain a stack $S$ of candidate hull vertices, processing points in counterclockwise polar angle order relative to the lowest point $p_0$.

```
GRAHAM-SCAN(Q):
    let p₀ = point with minimum y (leftmost on tie)
    sort remaining points by polar angle around p₀ (CCW)
    remove all but farthest point at each duplicate angle
    S = empty stack
    PUSH(p₀, S); PUSH(p₁, S); PUSH(p₂, S)
    for i = 3 to m:
        while angle(NEXT-TO-TOP(S), TOP(S), pᵢ) makes a non-left turn:
            POP(S)          // remove concave vertex
        PUSH(pᵢ, S)
    return S
```

**Correctness (loop invariant):** At the start of each iteration, stack $S$ contains exactly the vertices of $\text{CH}(\{p_0, \ldots, p_{i-1}\})$ in counterclockwise order.

**Running time:** $O(n \log n)$ — sorting dominates. Each point is pushed once and popped at most once (aggregate analysis), so the while loop totals $O(n)$.

#### Jarvis's March — $O(nh)$

**Idea:** Gift wrapping. Start at lowest point $p_0$, repeatedly find the point with the smallest polar angle relative to the current hull vertex. Build the right chain up to the highest point, then the left chain back down.

**Running time:** $O(nh)$ where $h = |\text{CH}(Q)|$. Each of the $h$ hull vertices requires scanning all $n$ points. When $h = o(\log n)$, Jarvis's march beats Graham's scan.

#### Other Methods

| Method | Running Time |
|---|---|
| **Incremental** | $O(n \log n)$ — sort left-to-right, update hull incrementally |
| **Divide-and-conquer** | $O(n \log n)$ — split, recurse, merge hulls in $O(n)$ |
| **Prune-and-search** | $O(n \log h)$ — repeatedly discard constant fraction of points |

---

### 33.4 Closest Pair of Points

**Problem:** Find the two points with minimum Euclidean distance among $n$ points.

**Brute force:** $O(n^2)$ — check all $\binom{n}{2}$ pairs.

**Divide-and-conquer:** $O(n \log n)$

```
CLOSEST-PAIR(P, X, Y):    // X sorted by x, Y sorted by y
    if |P| ≤ 3: brute-force all pairs

    // DIVIDE
    Split P into P_L and P_R by vertical line l
    Split X into X_L, X_R; split Y into Y_L, Y_R

    // CONQUER
    (p_L, q_L, δ_L) = CLOSEST-PAIR(P_L, X_L, Y_L)
    (p_R, q_R, δ_R) = CLOSEST-PAIR(P_R, X_R, Y_R)
    δ = min(δ_L, δ_R)

    // COMBINE: check strip of width 2δ around l
    Y' = points in Y within δ of l
    for each point p in Y':
        compare p with next 7 points in Y'  // key insight!
        update closest pair if distance < δ

    return closest pair and distance
```

**Key lemma:** Within the $\delta \times 2\delta$ rectangle centered on $l$, at most **8 points** can reside (4 from each half, since points within each half are ≥ $\delta$ apart). Therefore, for each point in $Y'$, we need only check the **next 7 points** in $Y'$-order.

**Running time recurrence:** $T(n) = 2T(n/2) + O(n)$ → $T(n) = O(n \log n)$.

**Implementation trick:** Pre-sort by $x$ and $y$ once ($O(n \log n)$), then maintain sorted arrays through the recursion to avoid re-sorting at each level.

---

## Chapter 34 — NP-Completeness

### 34.1–34.3 The Classes P, NP, and NP-Complete

#### Class P

**P** = the class of **decision problems** solvable in **polynomial time** by a deterministic Turing machine. Informally: problems with efficient algorithms.

Examples: shortest path, MST, sorting, linear programming, primality testing.

#### Class NP

**NP** = the class of decision problems where a **"yes"** instance has a **polynomial-time verifiable certificate**.

Formally: $L \in \text{NP}$ iff there exists a polynomial-time algorithm $A$ and a polynomial $p$ such that:

$$x \in L \iff \exists\, y \text{ with } |y| \leq p(|x|) \text{ such that } A(x, y) = \text{TRUE}$$

The string $y$ is the **certificate** (or witness). The verifier $A$ checks it in polynomial time.

Examples:
- **HAM-CYCLE:** certificate is the vertex sequence; verifier checks it visits all vertices and forms a cycle — $O(n)$.
- **CLIQUE:** certificate is the subset of $k$ vertices; verifier checks all $\binom{k}{2}$ edges exist — $O(k^2)$.
- **SAT:** certificate is a truth assignment; verifier evaluates the formula — $O(|\phi|)$.

**Key relationship:** $\text{P} \subseteq \text{NP}$. If you can solve a problem in polynomial time, you can certainly verify a solution in polynomial time (ignore the certificate and recompute).

**Open question:** Does $\text{P} = \text{NP}$? This is one of the seven Millennium Prize Problems. Most computer scientists believe $\text{P} \neq \text{NP}$.

#### NP-Hard and NP-Complete

- **NP-hard:** A problem $H$ is NP-hard if every problem in NP is polynomial-time reducible to $H$. (At least as hard as any NP problem, but not necessarily in NP itself.)
- **NP-complete:** A problem that is both **NP-hard** and **in NP** ($\text{NPC} = \text{NP-hard} \cap \text{NP}$).

**Implication:** If any NP-complete problem can be solved in polynomial time, then **P = NP** and every problem in NP has a polynomial-time solution.

#### Polynomial-Time Reductions

**Notation:** $A \leq_P B$ ("$A$ polynomial-time reduces to $B$") means there exists a polynomial-time computable function $f$ such that:

$$x \in A \iff f(x) \in B$$

**To show $B$ is NP-hard:** Pick a known NP-hard problem $A$ and show $A \leq_P B$.

**To show $B$ is NP-complete:** Show (1) $B \in \text{NP}$, and (2) some known NP-complete problem $A \leq_P B$.

**Transitivity:** If $A \leq_P B$ and $B \leq_P C$, then $A \leq_P C$.

---

### 34.4 NP-Completeness Proofs — The Reduction Chain

The foundational chain of reductions:

```
CIRCUIT-SAT ≤_P SAT ≤_P 3-CNF-SAT ≤_P CLIQUE
                                          ↓
3-CNF-SAT ≤_P SUBSET-SUM    CLIQUE ≤_P VERTEX-COVER
                                          ↓
                                VERTEX-COVER ≤_P HAM-CYCLE
                                          ↓
                                HAM-CYCLE ≤_P TSP
```

#### CIRCUIT-SAT ≤_P SAT (Theorem 34.8)

Transform a boolean circuit into an equivalent boolean formula. For each gate, create a variable and a clause encoding the gate's logic. The formula is satisfiable iff the circuit has a satisfying assignment.

#### SAT ≤_P 3-CNF-SAT (Theorem 34.10)

Three steps:
1. **Circuit → Formula:** Introduce variables for each gate, build a conjunction of sub-formulas.
2. **Formula → CNF:** Apply DeMorgan's laws and distributive law to push OR inward (may cause exponential blow-up, but we use the structure of step 1 to avoid this).
3. **CNF → 3-CNF:** Split long clauses using auxiliary variables $p, q$:
   - Clause with ≥ 4 literals → split into multiple 3-literal clauses
   - Clause with 2 literals $(l_1 \lor l_2)$ → $(l_1 \lor l_2 \lor p) \land (l_1 \lor l_2 \lor \bar{p})$
   - Clause with 1 literal $l$ → four clauses using $p, q$ that are satisfiable iff $l$ is true

Each step is polynomial-time computable.

---

### 34.5 NP-Complete Problems

#### CLIQUE (Theorem 34.11)

**3-CNF-SAT $\leq_P$ CLIQUE**

Given a 3-CNF formula $\phi = C_1 \land C_2 \land \cdots \land C_k$:

**Construction:** For each clause $C_r = (l_1^r \lor l_2^r \lor l_3^r)$, create three vertices (one per literal). Add edge between $v_i^r$ and $v_j^s$ iff:
- They are in **different** clauses ($r \neq s$), AND
- Their literals are **consistent** ($l_i^r$ is not the negation of $l_j^s$)

**Claim:** $\phi$ is satisfiable $\iff$ $G$ has a clique of size $k$.

- **→:** Pick one "true" literal per clause → $k$ vertices forming a clique (all consistent).
- **←:** A $k$-clique has exactly one vertex per triple (no intra-triple edges). Set those literals to 1 — no conflicts (no edges between inconsistent literals).

#### VERTEX-COVER (Theorem 34.12)

**CLIQUE $\leq_P$ VERTEX-COVER**

Given graph $G = (V, E)$ and integer $k$, construct $\bar{G} = (V, \bar{E})$ (complement graph). Output $(\bar{G},\; |V| - k)$.

**Claim:** $G$ has a $k$-clique $\iff$ $\bar{G}$ has a vertex cover of size $|V| - k$.

- **→:** If $V' \subseteq V$ is a $k$-clique in $G$, then $V - V'$ is a vertex cover in $\bar{G}$: every edge in $\bar{E}$ connects two vertices not both in $V'$ (since $V'$ is a clique in $G$, all pairs within it are edges of $G$, not $\bar{G}$).
- **←:** If $V - V'$ is a vertex cover in $\bar{G}$ of size $|V| - k$, then $V'$ is a clique in $G$: for any $u, v \in V'$, the edge $(u, v) \in \bar{E}$ would not be covered, so $(u, v) \in E$.

#### HAMILTONIAN-CYCLE (Theorem 34.13)

**VERTEX-COVER $\leq_P$ HAM-CYCLE**

Uses a **widget** construction: for each edge $(u, v) \in E$, create a 12-vertex widget $W_{uv}$ with constrained traversal paths. Selector vertices $s_1, \ldots, s_k$ choose the $k$ vertices of the cover. The construction ensures a Hamiltonian cycle exists iff $G$ has a vertex cover of size $k$.

**Widget properties:** Any Hamiltonian cycle must traverse the widget in one of three specific patterns (visiting all 12 vertices), corresponding to whether $u$, $v$, or both are in the vertex cover.

#### TSP (Theorem 34.14)

**HAM-CYCLE $\leq_P$ TSP**

Given graph $G = (V, E)$, construct a complete graph $G' = (V, V \times V)$ with edge weights:

$$w(u, v) = \begin{cases} 0 & \text{if } (u, v) \in E \\ 1 & \text{if } (u, v) \notin E \end{cases}$$

$G$ has a Hamiltonian cycle $\iff$ TSP instance $(G', 0)$ has a tour of cost 0.

#### SUBSET-SUM (Theorem 34.15)

**3-CNF-SAT $\leq_P$ SUBSET-SUM**

Construct integers in base 10 (or any base ≥ 7) encoding the formula structure:
- Variables $x_i$ → two values $v_i$ and $v_i'$ (corresponding to $x_i = 1$ and $x_i = 0$)
- Clauses $C_j$ → two slack values $s_j$ and $s_j'$ (to pad each clause-digit to target value 4)
- Target $t$: each variable-digit is 1, each clause-digit is 4

No carries between digit positions (base ≥ 7, max digit sum is 6), so digits are independent. Each variable-digit contributes 1 to the target. Each clause-digit needs 3 (from true literals) + 1 or 2 (from slack) = 4.

**Claim:** $\phi$ is satisfiable $\iff$ subset sums to $t$.

---

### 34.5 Summary of NP-Complete Problems

| Problem | Domain | Input | Question |
|---|---|---|---|
| **CIRCUIT-SAT** | Boolean logic | Boolean circuit | Satisfiable? |
| **SAT** | Boolean logic | Boolean formula | Satisfiable? |
| **3-CNF-SAT** | Boolean logic | 3-CNF formula | Satisfiable? |
| **CLIQUE** | Graphs | Graph $G$, integer $k$ | $k$-clique exists? |
| **VERTEX-COVER** | Graphs | Graph $G$, integer $k$ | Size-$k$ vertex cover? |
| **HAM-CYCLE** | Graphs | Graph $G$ | Hamiltonian cycle? |
| **TSP** | Graphs | Complete graph, bound $b$ | Tour of cost ≤ $b$? |
| **SUBSET-SUM** | Arithmetic | Set $S$, target $t$ | Subset sums to $t$? |

---

## Chapter 35 — Approximation Algorithms

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
- [[04_Binary_Heaps]] — heap-based sorting for convex hull vertex ordering
- [[05_Fibonacci_Heaps]] — Prim's algorithm for MST in TSP approximation
