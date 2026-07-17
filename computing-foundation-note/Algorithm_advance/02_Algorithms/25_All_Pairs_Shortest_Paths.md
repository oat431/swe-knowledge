---
title: "25 All Pairs Shortest Paths"
tags:
  - all-pairs
  - floyd-warshall
  - johnson
  - graph-algorithms
  - clrs
source: CLRS
---

# 25 · All-Pairs Shortest Paths

> Find shortest paths between **every pair** of vertices. Floyd-Warshall is O(V³) for dense graphs; Johnson's combines reweighting with Dijkstra for sparse graphs.

### Problem Definition

Find shortest paths between **every pair** of vertices. Output: $n \times n$ matrix $D = (d_{ij})$ where $d_{ij} = \delta(i, j)$.

**Naive approach:** Run single-source algorithm $|V|$ times.

| Single-Source Algorithm | Total Time |
|---|---|
| Dijkstra (Fibonacci heap) | $O(VE + V^2 \log V)$ |
| Bellman-Ford | $O(V^2 E)$ = $O(V^4)$ dense |

### Matrix Multiplication Approach (§25.1–25.2)

Shortest paths can be computed via repeated matrix multiplication in the **min-plus semiring**:

$$d_{ij}^{(m)} = \min_{1 \leq k \leq n} \{d_{ik}^{(m-1)} + w_{kj}\}$$

- After $m$ multiplications: shortest paths using at most $m$ edges
- After $|V| - 1$ multiplications: all shortest paths (no negative cycles)
- **Repeated squaring** variant: $O(V^3 \log V)$

### Floyd-Warshall Algorithm

Dynamic programming approach: considers **intermediate vertices** explicitly.

**Idea:** Let $d_{ij}^{(k)}$ = shortest path from $i$ to $j$ using only vertices $\{1, 2, \ldots, k\}$ as intermediate vertices.

**Recurrence:**
$$d_{ij}^{(k)} = \begin{cases} w_{ij} & \text{if } k = 0 \\ \min\left(d_{ij}^{(k-1)},\; d_{ik}^{(k-1)} + d_{kj}^{(k-1)}\right) & \text{if } k \geq 1 \end{cases}$$

```
FLOYD-WARSHALL(W)
1  n = W.rows
2  D⁰ = W
3  for k = 1 to n
4      let D^(k) = (d_ij^(k)) be a new n × n matrix
5      for i = 1 to n
6          for j = 1 to n
7              d_ij^(k) = min(d_ij^(k-1), d_ik^(k-1) + d_kj^(k-1))
8  return D^(n)
```

| Property | Value |
|---|---|
| **Time** | $\Theta(V^3)$ |
| **Space** | $\Theta(V^2)$ (in-place) |
| **Negative weights** | ✅ |
| **Negative cycle detection** | ✅ (check diagonal) |

**Key insight:** The $k$-th iteration asks "Is vertex $k$ useful as an intermediate on the path from $i$ to $j$?"

### Transitive Closure (§25.2)

Use Floyd-Warshall with boolean OR/AND instead of min/+ to compute reachability:

$$t_{ij}^{(k)} = t_{ij}^{(k-1)} \lor (t_{ik}^{(k-1)} \land t_{kj}^{(k-1)})$$

### Johnson's Algorithm (§25.3)

For **sparse graphs** — combines reweighting with Dijkstra.

**Steps:**
1. Create $G'$ by adding new vertex $s$ with zero-weight edges to all vertices
2. Run Bellman-Ford from $s$ to compute $h(v) = \delta(s, v)$ for all $v$
3. If negative cycle detected → report and stop
4. **Reweight** edges: $\hat{w}(u, v) = w(u, v) + h(u) - h(v)$ (all nonnegative!)
5. Run Dijkstra from each vertex with reweighted edges
6. Recover original shortest-path weights: $\delta(u, v) = \hat{\delta}(u, v) - h(u) + h(v)$

| Property | Value |
|---|---|
| **Time** | $O(V^2 \log V + VE)$ |
| **Best for** | Sparse graphs with negative weights |

### All-Pairs Algorithm Summary

| Algorithm | Time | Negative Weights? | Best For |
|---|---|---|---|
| Repeated Dijkstra | $O(VE + V^2 \log V)$ | ❌ | Nonnegative weights |
| Floyd-Warshall | $\Theta(V^3)$ | ✅ | Dense graphs |
| Johnson's | $O(V^2 \log V + VE)$ | ✅ | Sparse graphs |
| Matrix multiplication | $O(V^3 \log V)$ | ✅ (no neg. cycles) | Theoretical |

---

## Hands-On Exercises

### Exercise 3: Floyd-Warshall — Trace Matrix Updates
Given 4-vertex graph with adjacency matrix:

```
W = | 0   3   ∞   7  |
    | 8   0   2   ∞  |
    | 5   ∞   0   1  |
    | 2   ∞   ∞   0  |
```

Trace Floyd-Warshall for `k = 1, 2, 3, 4`:
1. `D⁽⁰⁾` = W (initial).
2. For `k = 1`: Update `D⁽¹⁾[i][j] = min(D⁽⁰⁾[i][j], D⁽⁰⁾[i][1] + D⁽⁰⁾[1][j])` for all i, j.
3. Continue for k = 2, 3, 4.
4. Final `D⁽⁴⁾` gives all-pairs shortest paths.

```java
// Show D^(0), D^(1), D^(2), D^(3), D^(4) as matrices
```

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 5 | [Find the City With Smallest Number of Neighbors](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/) (LC 1334) | 🟡 Medium | Floyd-Warshall |

### Assignment Guidelines
- **Problem 1**: Floyd-Warshall to find all-pairs shortest paths, then count cities within threshold.
- **Target time:** 20 min per Medium.