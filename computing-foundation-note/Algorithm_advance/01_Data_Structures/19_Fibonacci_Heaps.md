---
title: "19 Fibonacci Heaps"
tags:
  - fibonacci-heap
  - priority-queue
  - amortized-analysis
  - clrs
source: CLRS
---

# 19 · Fibonacci Heaps

> Fibonacci heaps achieve **optimal amortized bounds** — especially `DECREASE-KEY` in O(1). This is the key to Dijkstra's and Prim's optimal complexity.

### Motivation

Fibonacci heaps achieve **optimal amortized bounds** for priority queue operations, especially **`DECREASE-KEY` in $O(1)$** — critical for graph algorithms like Dijkstra's and Prim's.

| Operation | Binary Heap | Fibonacci Heap |
|---|---|---|
| `MAKE-HEAP` | $\Theta(1)$ | $\Theta(1)$ |
| `INSERT` | $\Theta(\lg n)$ | $\Theta(1)$ |
| `MINIMUM` | $\Theta(1)$ | $\Theta(1)$ |
| `EXTRACT-MIN` | $\Theta(\lg n)$ | $O(\lg n)$ |
| `UNION` | $\Theta(n)$ | $\Theta(1)$ |
| `DECREASE-KEY` | $\Theta(\lg n)$ | $\Theta(1)$ |
| `DELETE` | $\Theta(\lg n)$ | $O(\lg n)$ |

> Fibonacci heaps are predominantly of **theoretical interest** — constant factors and complexity make binary/k-ary heaps preferable in practice.

### Structure

A Fibonacci heap is a **collection of min-heap-ordered rooted trees** with a lazy structure.

**Node attributes:**
- `key`, `degree` (number of children)
- `parent`, `child` (pointer to any child)
- `left`, `right` (circular doubly-linked sibling list)
- `mark` — TRUE if node has lost a child **since becoming a child of its current parent**

**Heap attributes:**
- `min` — pointer to minimum node
- `n` — total number of nodes

### Potential Function

$$\Phi(H) = t(H) + 2m(H)$$

where $t(H)$ = number of trees, $m(H)$ = number of marked nodes.

### Mergeable Heap Operations

#### `FIB-HEAP-INSERT(H, x)` — $O(1)$ amortized
- Create single-node tree, add to root list
- Update `min` if needed
- Amortized cost: $O(1) + 1 = O(1)$

#### `FIB-HEAP-UNION(H₁, H₂)` — $O(1)$ amortized
- Concatenate root lists, update `min`
- $\Delta\Phi = 0$, amortized cost = actual cost = $O(1)$

#### `FIB-HEAP-EXTRACT-MIN(H)` — $O(\lg n)$ amortized
1. Remove min node, add its children to root list
2. **CONSOLIDATE:** Link trees of equal degree until at most one tree per degree

**Consolidation:**
- Use auxiliary array $A[0..D(n)]$ indexed by degree
- For each root $w$ in root list:
  - While another root $y$ has same degree as $x$ (starting at $w$):
    - Link $y$ to $x$ (smaller key becomes parent)
  - Place $x$ in $A[\text{degree}]$
- Rebuild root list from array

**Amortized cost:** $O(D(n)) = O(\lg n)$

### Decrease Key and Delete

#### `FIB-HEAP-DECREASE-KEY(H, x, k)` — $O(1)$ amortized
If min-heap order violated:
1. **CUT** $x$ from its parent → make $x$ a root
2. **CASCADING-CUT** up the tree:
   - If parent is unmarked → mark it (first child lost)
   - If parent is already marked → cut parent, recurse (second child lost)

**The mark bit** enforces: a node can lose **at most one child** before being cut from its parent. This limits tree degradation.

**Amortized analysis:**
- Each cascading cut: unmarks a node (−2 potential) and creates a new root (+1)
- Net: each cut "pays for itself" through potential reduction
- Amortized cost: $O(1)$

#### `FIB-HEAP-DELETE(H, x)` — $O(\lg n)$ amortized
1. Decrease key to $-\infty$ → node becomes minimum
2. Extract minimum

### Maximum Degree Bound

**Theorem:** $D(n) = O(\lg n)$ where $D(n)$ is the maximum degree of any node in an $n$-node Fibonacci heap.

**Key insight:** If node $x$ has degree $k$, then $\text{size}(x) \geq F_{k+2}$ (Fibonacci number).

**Proof sketch:**
- Children linked in order $y_1, y_2, \ldots, y_k$
- $y_i.\text{degree} \geq i - 2$ (each child lost at most one subtree before being cut)
- Size bound follows from Fibonacci recurrence: $\text{size}(x) \geq 2 + \sum_{i=2}^{k} F_i = F_{k+2}$

Since $F_k \approx \phi^k / \sqrt{5}$ where $\phi = (1+\sqrt{5})/2$:

$$n \geq F_{k+2} \geq \phi^k \implies k \leq \log_\phi n$$

This is why the structure is called a **Fibonacci** heap.

### When to Use Fibonacci Heaps

- **Graph algorithms** with many `DECREASE-KEY` calls: Dijkstra's ($O((V+E)\lg V)$ → $O(V \lg V + E)$), Prim's MST
- **Dense graphs** where $|E| \gg |V|$
- Not practical for small inputs or when `EXTRACT-MIN` dominates

---

## Hands-On Exercises

### Exercise 3: Fibonacci Heap — Trace Operations
Start with an empty Fibonacci heap. Perform:
1. Insert keys: 7, 3, 17, 24, 1, 5
2. Extract-min. What is the new minimum? How many trees are in the root list after consolidation?
3. Decrease-key of node with key 24 to 2. Does cascading cut occur?
4. Decrease-key of node with key 17 to 0. What happens?

For each step, draw the root list and mark any marked nodes. Track the potential `Φ = t(H) + 2m(H)`.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 8 | **Analyze Fibonacci Heap DECREASE-KEY** | 🟡 Theory | Potential method |

### Assignment Guidelines
- **Problem 1**: Prove O(1) amortized using Φ = t(H) + 2m(H).
- **Problem 2**: Major project — implement full Fibonacci heap.
- **Target time:** 30 min per theory, 90 min for implementation.