---
title: "23 Minimum Spanning Trees"
tags:
  - mst
  - kruskal
  - prim
  - greedy
  - graph-algorithms
  - clrs
source: CLRS
---

# 23 · Minimum Spanning Trees

> Find the spanning tree of minimum total weight. Kruskal's and Prim's are the two classic greedy algorithms — one edge-centric, one vertex-centric.

### Problem Definition

Given a connected, undirected graph $G = (V, E)$ with a weight function $w: E \to \mathbb{R}$, find a **minimum spanning tree (MST)** — a spanning tree of minimum total weight.

**Key property:** A spanning tree connects all $|V|$ vertices with exactly $|V| - 1$ edges.

### Generic MST Algorithm

```
GENERIC-MST(G, w)
1  A = ∅
2  while A does not form a spanning tree
3      find an edge (u, v) that is safe for A
4      A = A ∪ {(u, v)}
5  return A
```

An edge $(u, v)$ is **safe** for $A$ if adding it to $A$ maintains the invariant that $A$ is a subset of some MST.

### Cut Property

A **cut** $(S, V - S)$ is a partition of vertices. An edge $(u, v)$ **crosses** the cut if one endpoint is in $S$ and the other in $V - S$.

> **Theorem (Cut Property):** For any cut $(S, V - S)$ of $G$, if edge $(u, v)$ is a light edge crossing the cut, then $(u, v)$ is safe for $A$.

A **light edge** crossing a cut is one with minimum weight among all edges crossing that cut.

### Kruskal's Algorithm

**Strategy:** Greedy — process edges in order of increasing weight; add each edge if it connects two different components.

```
KRUSKAL(G, w)
1  A = ∅
2  for each vertex v ∈ V
3      MAKE-SET(v)
4  sort edges of E by weight w (nondecreasing)
5  for each edge (u, v) ∈ E, in sorted order
6      if FIND-SET(u) ≠ FIND-SET(v)
7          A = A ∪ {(u, v)}
8          UNION(u, v)
9  return A
```

**Data structure:** Disjoint-set (Union-Find) with union by rank and path compression.

| Complexity | Value |
|---|---|
| **Time** | $O(E \log E)$ = $O(E \log V)$ |
| **Bottleneck** | Sorting edges |

### Prim's Algorithm

**Strategy:** Greedy — grow a single tree from an arbitrary start vertex, always adding the cheapest edge connecting the tree to a new vertex. (Similar to Dijkstra's algorithm.)

```
PRIM(G, w, r)
1  for each u ∈ V
2      u.key = ∞
3      u.π = NIL
4  r.key = 0
5  Q = V                           // min-priority queue, keyed on key
6  while Q ≠ ∅
7      u = EXTRACT-MIN(Q)
8      for each v ∈ Adj[u]
9          if v ∈ Q and w(u, v) < v.key
10             v.π = u
11             v.key = w(u, v)
```

| Implementation | Time |
|---|---|
| Binary min-heap | $O(E \log V)$ |
| Fibonacci heap | $O(E + V \log V)$ |

### Kruskal vs. Prim

| Feature | Kruskal | Prim |
|---|---|---|
| Strategy | Edge-centric (sort all edges) | Vertex-centric (grow from root) |
| Data structure | Union-Find | Priority queue |
| Best for | Sparse graphs | Dense graphs |
| Graph type | Undirected, connected | Undirected, connected |

---

## Hands-On Exercises

### Exercise 1: Kruskal's MST — Trace by Hand
Given graph with edges (sorted by weight):

| Edge | Weight |
|------|--------|
| (B,C) | 1 |
| (D,E) | 2 |
| (A,B) | 3 |
| (B,D) | 4 |
| (A,D) | 5 |
| (C,E) | 6 |
| (B,E) | 7 |

Vertices: {A, B, C, D, E}. Trace Kruskal's algorithm:
1. Start with empty MST and each vertex in its own set.
2. Process edges in order. For each edge, check if endpoints are in different sets (FIND-SET).
3. If yes, add to MST and UNION. If no, skip.
4. Draw the final MST. What is its total weight?

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/) (LC 1584) | 🟡 Medium | Prim's / Kruskal's MST |

### Assignment Guidelines
- **Start** with the LeetCode problem — implement Prim's or Kruskal's.
- **Target time:** 20 min per Medium.