---
title: "20 van Emde Boas Trees"
tags:
  - veb-tree
  - priority-queue
  - integer-keys
  - clrs
source: CLRS
---

# 20 · van Emde Boas Trees

> Priority queue operations in **O(lg lg u)** time when keys are integers in {0, ..., u-1}. Beats the Ω(lg n) comparison-based lower bound.

### Motivation

Priority queue operations in $O(\lg \lg u)$ time when keys are **integers in range $\{0, 1, \ldots, u-1\}$**. This beats the $\Omega(\lg n)$ comparison-based lower bound by exploiting integer structure.

> **Prerequisite:** Universe size $u$ must be known upfront and be an exact power of 2.

### Key Idea: Recursive Square-Root Decomposition

Split the $u$-bit universe into clusters:
- $\lceil\sqrt{u}\rceil$ clusters, each of size $\lfloor\sqrt{u}\rfloor$
- **Summary** structure tracks which clusters are non-empty
- Recurse on both clusters and summary

### Helper Functions

```
high(x) = ⌊x / ⌊√u⌋⌋     → which cluster
low(x)  = x mod ⌊√u⌋      → position within cluster
index(x, y) = x · ⌊√u⌋ + y  → reconstruct value
```

### Proto van Emde Boas Structure

**Base case:** $u = 2$ — a 2-bit array $A[0..1]$

**Recursive case ($u > 2$):**
- `summary`: proto-vEB($\lceil\sqrt{u}\rceil$) — tracks non-empty clusters
- `cluster[0..⌈√u⌉−1]`: array of proto-vEB($\lfloor\sqrt{u}\rfloor$) pointers

**Problem:** Most operations make **two** recursive calls → $T(u) = 2T(\sqrt{u}) + O(1) = O(\lg u)$, not $O(\lg \lg u)$.

### van Emde Boas Tree (Full Version)

**Key enhancement:** Store `min` and `max` explicitly in each node.

**Rules:**
- `min` stores the minimum element but it is **NOT** stored in any cluster
- `max` stores the maximum (may also appear in a cluster if $|S| \geq 2$)

**This breaks the two-call pattern:**
- `MINIMUM`/`MAXIMUM`: Return `min`/`max` in $O(1)$ — no recursion
- `SUCCESSOR`: Check cluster's `max` to decide if successor is local → one recursive call
- `INSERT` into empty tree: Just set `min` and `max` — no recursion
- `DELETE` from single-element tree: Clear `min` and `max` — no recursion

**Result:** $T(u) = T(\lceil\sqrt{u}\rceil) + O(1) = O(\lg \lg u)$

### Operations Summary

| Operation | Time | Technique |
|---|---|---|
| `MINIMUM` / `MAXIMUM` | $O(1)$ | Direct attribute access |
| `MEMBER` | $O(\lg \lg u)$ | Single recursive descent |
| `SUCCESSOR` / `PREDECESSOR` | $O(\lg \lg u)$ | One recursive call (cluster or summary) |
| `INSERT` | $O(\lg \lg u)$ | One recursive call (only if cluster non-empty) |
| `DELETE` | $O(\lg \lg u)$ | One recursive call (mutually exclusive cases) |

### INSERT Detail

```
VEB-TREE-INSERT(V, x):
    if V.min == NIL:
        VEB-EMPTY-TREE-INSERT(V, x)   // O(1)
    else if x < V.min:
        swap x with V.min              // new min not stored in clusters
    if V.u > 2:
        if cluster[high(x)] is empty:
            INSERT into V.summary       // mark cluster non-empty
            EMPTY-TREE-INSERT(cluster[high(x)], low(x))
        else:
            INSERT into cluster[high(x)]  // single recursive call
    if x > V.max: V.max = x
```

### DELETE Detail

The key trick: when deleting `min`, find the actual minimum in the clusters to replace it.

```
VEB-TREE-DELETE(V, x):
    if V.min == V.max:               // single element
        V.min = V.max = NIL
    elif V.u == 2:                    // base case
        set remaining element as min/max
    elif x == V.min:
        first_cluster = MINIMUM(V.summary)
        x = index(first_cluster, MINIMUM(cluster[first_cluster]))
        V.min = x                     // new min from clusters
        // fall through to delete x from its cluster
    DELETE from cluster[high(x)]
    if cluster now empty:
        DELETE from V.summary
    update V.max if needed
```

**Critical observation:** The two recursive calls (lines 13 and 15) are **mutually exclusive** — if the cluster becomes empty (triggering summary update), the first call was $O(1)$ because it deleted the sole element.

### Space and Construction

- **Space:** $O(u)$ — pre-allocated for all possible universe values
- **Construction:** $O(u)$ time (vs. $O(1)$ for red-black trees)
- Trade-off: expensive initialization pays off only with many operations

### Reduced-Space Variant (RS-vEB)

Replace the `cluster` array with a **hash table** (dynamic table):
- Only stores pointers to non-empty clusters
- **Space:** $O(n)$ (where $n$ = actual elements stored)
- Operations remain $O(\lg \lg u)$ expected amortized time
- **y-fast tries** achieve $O(n)$ space with $O(\lg \lg u)$ worst-case queries

---

## Hands-On Exercises

### Exercise 4: vEB Tree — Conceptual Understanding
For a vEB tree with universe size `u = 16`:

1. How many clusters are there? What is each cluster's size?
2. What does the `summary` structure track?
3. Trace `INSERT(2)`, `INSERT(3)`, `INSERT(7)`, `INSERT(10)`.
4. What does `SUCCESSOR(3)` return? Trace the recursive calls.

**Hint:** `high(x) = ⌊x / ⌊√u⌋⌋`, `low(x) = x mod ⌊√u⌋`. For `u = 16`: `√u = 4`.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | **Trace vEB INSERT and SUCCESSOR** | 🟡 Theory | Recursive decomposition |
| 2 | **Implement proto-vEB tree** | 🟡 Code | Basic recursive structure |
| 3 | **Why does explicit min/max break the two-call pattern?** | 🟡 Theory | Recursion depth |

### Assignment Guidelines
- **Problem 1**: Trace on paper with u=16.
- **Problem 2**: Implement proto-vEB (without min/max optimization).
- **Target time:** 20 min per theory, 45 min for code.