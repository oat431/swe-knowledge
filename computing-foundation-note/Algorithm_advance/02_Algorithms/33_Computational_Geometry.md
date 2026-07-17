---
title: "33 Computational Geometry"
tags:
  - computational-geometry
  - convex-hull
  - sweep-line
  - clrs
source: CLRS
---

# 33 · Computational Geometry

> Algorithms for spatial problems: cross products for O(1) orientation, sweep line for O(n log n) intersection, Graham's scan for convex hull.

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

## Hands-On Exercises

### Exercise 1: Cross Product — Orientation Test
Given three points: `p₀ = (0,0)`, `p₁ = (4,4)`, `p₂ = (1,2)`.

1. Compute `(p₁ - p₀) × (p₂ - p₀)`.
2. Is the turn from `p₀→p₁` to `p₀→p₂` clockwise, counterclockwise, or colinear?
3. Now try `p₂ = (2,1)`. What's the orientation?
4. And `p₂ = (2,2)`? (Colinear case.)

```java
int cross(int[] p0, int[] p1, int[] p2) {
    return (p1[0] - p0[0]) * (p2[1] - p0[1])
         - (p2[0] - p0[0]) * (p1[1] - p0[1]);
}
// Positive → counterclockwise (left turn)
// Negative → clockwise (right turn)
// Zero → colinear
```

---


### Exercise 2: Segment Intersection
Given segments `p₁p₂ = (0,0)→(4,4)` and `p₃p₄ = (0,4)→(4,0)`:

1. Compute `d₁ = direction(p₃, p₄, p₁)` and `d₂ = direction(p₃, p₄, p₂)`.
2. Compute `d₃ = direction(p₁, p₂, p₃)` and `d₄ = direction(p₁, p₂, p₄)`.
3. Apply the straddling test: do `(d₁, d₂)` have opposite signs? Do `(d₃, d₄)`?
4. Do the segments intersect?

Now try `p₃p₄ = (0,2)→(2,0)`. Does this intersect `p₁p₂`?

---


### Exercise 3: Graham's Scan — Trace Convex Hull
Given points: `(0,0), (1,1), (2,2), (3,1), (3,3), (4,0), (2,0)`.

1. Find `p₀` — the point with minimum y (leftmost on tie).
2. Sort remaining points by polar angle around `p₀`.
3. Trace Graham's scan:
   - Push `p₀`, `p₁`, `p₂` onto stack.
   - For each subsequent point: while top two + new point make a non-left turn, pop.
   - Push new point.
4. What points are on the convex hull?

```java
// Draw the points on graph paper and trace visually.
// The hull should be: (0,0) → (4,0) → (3,3) → ... → back to (0,0)
```

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Convex Hull](https://leetcode.com/problems/erect-the-fence/) (LC 587) | 🔴 Hard | Graham's Scan / Jarvis March |
| 2 | [Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/) (LC 149) | 🟡 Medium | Cross product / Slope |
| 3 | [Rectangle Overlap](https://leetcode.com/problems/rectangle-overlap/) (LC 836) | 🟢 Easy | Geometry |
| 4 | [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) (LC 973) | 🟡 Medium | Distance / Heap |
| 7 | **Implement Graham's Scan** | 🔴 Code | Convex hull |

### Assignment Guidelines
- **Start** with 3 (Easy), then 2+4 (Medium), then 1 (Hard — Graham's scan).
- **Target time:** 10 min per Easy, 20 min per Medium, 30 min per Hard.