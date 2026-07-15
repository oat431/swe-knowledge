---
title: Amortized Analysis
tags:
  - amortized-analysis
  - algorithms
  - clrs
source: CLRS Chapter 17
---

# Amortized Analysis

> In an amortized analysis, we average the time required to perform a sequence of data-structure operations over all the operations performed. The average cost per operation is small even though a single operation within the sequence might be expensive. Unlike average-case analysis, **probability is not involved** — an amortized analysis guarantees the average performance of each operation **in the worst case**.

## Key Distinction: Amortized vs Average-Case

| Aspect | Amortized Analysis | Average-Case Analysis |
|---|---|---|
| Probability involved? | No | Yes |
| Guarantee | Worst-case bound over sequence | Expected bound assuming distribution |
| Insight | Structural: exploits how operations interact | Statistical: relies on input distribution |

---

## 17.1 Aggregate Analysis

**Idea:** Determine an upper bound $T(n)$ on the total cost of a sequence of $n$ operations. The amortized cost per operation is $T(n)/n$ — the same for every operation, regardless of type.

### Example 1: Stack with MULTIPOP

Standard stack operations (each $O(1)$):
- `PUSH(S, x)` — push object onto stack
- `POP(S)` — pop top object

Augmented with:
- `MULTIPOP(S, k)` — pop up to $k$ objects (or until empty)

```
MULTIPOP(S, k):
    while not STACK-EMPTY(S) and k > 0
        POP(S)
        k = k - 1
```

**Naive worst-case:** Each `MULTIPOP` costs $O(n)$, so $n$ operations could cost $O(n^2)$.

**Aggregate analysis insight:** Each object can be popped **at most once** per push. For any sequence of $n$ `PUSH`, `POP`, and `MULTIPOP` operations on an initially empty stack, the total number of pops ≤ number of pushes ≤ $n$.

$$T(n) = O(n) \implies \text{amortized cost} = O(n)/n = O(1) \text{ per operation}$$

### Example 2: Incrementing a Binary Counter

A $k$-bit counter stored in array $A[0..k-1]$. `INCREMENT` flips trailing 1s to 0 and the first 0 to 1.

```
INCREMENT(A):
    i = 0
    while i < A.length and A[i] == 1
        A[i] = 0
        i = i + 1
    if i < A.length
        A[i] = 1
```

**Naive worst-case:** Single `INCREMENT` flips $O(k)$ bits → $n$ operations cost $O(nk)$.

**Aggregate analysis insight:** Bit $A[0]$ flips every time, $A[1]$ flips every 2nd time, $A[i]$ flips every $2^i$-th time. Total flips across $n$ increments:

$$\sum_{i=0}^{k-1} \left\lfloor \frac{n}{2^i} \right\rfloor < n \sum_{i=0}^{\infty} \frac{1}{2^i} = 2n$$

$$T(n) = O(n) \implies \text{amortized cost} = O(1) \text{ per INCREMENT}$$

---

## 17.2 Accounting Method

**Idea:** Assign different **amortized costs** to different operation types. Overcharged operations store **prepaid credit** on objects in the data structure. Undercharged operations draw from that credit. The invariant: **total credit never goes negative**.

### Rules

1. Assign an amortized cost $\hat{c}_i$ to each operation $i$ (may differ from actual cost $c_i$).
2. Credit after $n$ operations = $\sum_{i=1}^n (\hat{c}_i - c_i) \geq 0$.
3. If credit is always ≥ 0, then $\sum \hat{c}_i \geq \sum c_i$ — the amortized cost upper-bounds the actual total cost.

### Stack Example

| Operation | Amortized Cost | Actual Cost | Credit Change |
|---|---|---|---|
| `PUSH` | 2 | 1 | +1 (store 1 credit on pushed object) |
| `POP` | 0 | 1 | −1 (credit from object pays for pop) |
| `MULTIPOP` | 0 | min(s, k) | −min(s, k) (credits pay for pops) |

**Invariant:** Each object on the stack carries 1 credit. Total credit ≥ 0 at all times.

Since $\hat{c} = O(1)$ per operation, total cost of $n$ operations = $O(n)$.

### Binary Counter Example

| Operation | Amortized Cost | Actual Cost | Credit Change |
|---|---|---|---|
| `INCREMENT` | 2 | # of 1-bits flipped + 1 | +1 for each 0→1 flip, −1 for each 1→0 flip |

Each 0→1 flip deposits 1 credit on that bit. Each 1→0 flip spends it. The final set bit pays for itself. Credit never goes negative.

---

## 17.3 Potential Method

**Idea:** Instead of associating credit with individual objects, represent the total credit as a **potential function** $\Phi$ of the entire data structure.

### Formal Definition

- Let $D_i$ be the state of the data structure after operation $i$.
- Potential function: $\Phi(D_i) \geq 0$ for all $i$, with $\Phi(D_0) = 0$ (initial state).
- Amortized cost of operation $i$:

$$\hat{c}_i = c_i + \Phi(D_i) - \Phi(D_{i-1})$$

- Summing over $n$ operations (telescoping):

$$\sum_{i=1}^n \hat{c}_i = \sum_{i=1}^n c_i + \Phi(D_n) - \Phi(D_0) \geq \sum_{i=1}^n c_i$$

since $\Phi(D_n) \geq 0 = \Phi(D_0)$. The amortized cost upper-bounds the actual total cost.

### Stack Example

Let $\Phi$ = number of objects on the stack.

| Operation | Actual $c_i$ | $\Phi(D_i) - \Phi(D_{i-1})$ | Amortized $\hat{c}_i$ |
|---|---|---|---|
| `PUSH` | 1 | +1 | 2 |
| `POP` | 1 | −1 | 0 |
| `MULTIPOP(S, k)` | $k' = \min(s, k)$ | $-k'$ | 0 |

### Binary Counter Example

Let $\Phi$ = number of 1-bits in the counter ($b_i$ = number of 1s after $i$-th increment).

For `INCREMENT` flipping $t$ bits (all trailing 1s plus one 0):
- Actual cost: $c_i = t + 1$ (but we count $t$ bit flips)
- $t-1$ bits go 1→0, 1 bit goes 0→1, so $\Phi(D_i) - \Phi(D_{i-1}) = 1 - (t-1) = 2 - t$

$$\hat{c}_i = t + (2 - t) = 2 = O(1)$$

---

## 17.4 Dynamic Tables

**Problem:** A hash table (or any array-backed structure) that must grow and shrink as items are inserted and deleted. A single resize costs $O(n)$ (copy all elements), but we want $O(1)$ amortized per operation.

### Table Expansion

**Strategy:** When inserting into a full table of size $m$, allocate a new table of size $2m$ and copy all $m$ elements.

**Potential function:**

$$\Phi(T) = 2 \cdot T.\text{num} - T.\text{size}$$

where `num` = number of elements, `size` = table capacity.

**Analysis of INSERT:**

- **Non-full table** (no expansion): $c_i = 1$, $\Phi$ increases by 2 → $\hat{c}_i = 3$.
- **Full table** (triggers expansion from $m$ to $2m$, $m$ elements copied): $c_i = m + 1$, $\Phi$ changes from $2m - m = m$ to $2(m+1) - 2m = 2$:

$$\hat{c}_i = (m+1) + (2 - m) = 3$$

Amortized cost per INSERT = **3** (constant!).

### Table Expansion and Contraction

**Strategy:** When a deletion drops `num` to `size/4`, shrink the table to half size. (The load factor stays between 1/4 and 1, with the threshold at 1/4 — not 1/2 — to avoid thrashing on alternating insert/delete near the boundary.)

**Potential function:**

$$\Phi(T) = \begin{cases} 2 \cdot T.\text{num} - T.\text{size} & \text{if } T.\text{num} \geq T.\text{size}/2 \\ T.\text{size}/2 - T.\text{num} & \text{if } T.\text{num} < T.\text{size}/2 \end{cases}$$

This ensures $\Phi \geq 0$ and that contraction (like expansion) has $O(1)$ amortized cost.

---

## Comparison of Methods

| Feature | Aggregate Analysis | Accounting Method | Potential Method |
|---|---|---|---|
| **Approach** | Bound total cost $T(n)$, divide by $n$ | Assign amortized cost per op type; track credit | Define potential function $\Phi$ on data structure state |
| **Per-operation cost** | Same for all operations | Can differ by operation type | Can differ by operation type |
| **Credit model** | None (implicit) | Prepaid credit on individual objects | Potential energy of whole structure |
| **Flexibility** | Simplest, least flexible | More flexible, intuitive | Most flexible and powerful |
| **Key invariant** | $T(n)/n$ is the bound | Total credit ≥ 0 | $\Phi(D) \geq 0$ always |
| **Math** | Summation / counting | Bookkeeping of deposits & withdrawals | Telescoping sum of $\Delta\Phi$ |
| **Best for** | Uniform operations | Intuitive per-object reasoning | Complex state-dependent costs |

All three methods yield the **same amortized bounds** when applied to the same problem — they are different lenses on the same phenomenon.

---

## Summary of Key Results

| Data Structure / Operation | Worst-Case (single) | Amortized |
|---|---|---|
| Stack `PUSH` | $O(1)$ | $O(1)$ |
| Stack `POP` | $O(1)$ | $O(1)$ |
| Stack `MULTIPOP` | $O(n)$ | $O(1)$ |
| Binary counter `INCREMENT` | $O(k)$ | $O(1)$ |
| Dynamic table `INSERT` (expand) | $O(n)$ | $O(1)$ |
| Dynamic table `DELETE` (contract) | $O(n)$ | $O(1)$ |

---

## See Also

- [[01 Stacks & Queues]] — underlying data structures for amortized examples
- [[01 Hash Tables]] — dynamic tables as hash table backing
- [[01 Heaps & Priority Queues]] — binary counter bit-flipping analysis
- [[02_Advanced_Data_Structures]] — Fibonacci heaps (advanced amortized application)
- [[02_Advanced_Data_Structures]] — Disjoint sets (union-by-rank / path compression)


---

## Hands-On Exercises

### Exercise 1: Aggregate Analysis — Stack with MULTIPOP
Consider a sequence of 10 operations on an initially empty stack: `PUSH, PUSH, PUSH, MULTIPOP(5), PUSH, PUSH, MULTIPOP(3), PUSH, MULTIPOP(10), PUSH`.

1. Count the **total number of actual pops** across all operations.
2. Count the **total number of pushes**.
3. Verify that total pops ≤ total pushes (the key invariant from the note).
4. What is the amortized cost per operation using aggregate analysis?

```java
// Trace through each operation. Record stack size, pops, and pushes.
// Operation      | Actual Pops | Actual Pushes | Stack Size After
// PUSH           |             |               |
// PUSH           |             |               |
// ... (fill in all 10)
```

---

### Exercise 2: Accounting Method — Binary Counter
A 4-bit counter starts at `0000`. Run 16 increments (`0000` → `1111`).

1. For each increment, count the number of bit flips (actual cost).
2. Assign amortized cost = 2 per increment (as in the note).
3. Track the **running credit** (cumulative overpayment). Verify credit never goes negative.
4. What is the total actual cost? Total amortized cost? Verify `∑ ĉᵢ ≥ ∑ cᵢ`.

```java
// Fill in a table:
// Counter | Binary | Bit Flips | Amortized Cost | Credit Change | Total Credit
// 0       | 0000   |     —     |       —        |       —       |     0
// 1       | 0001   |     ?     |       2        |       ?       |     ?
// ... (fill all 16)
```

---

### Exercise 3: Potential Method — Dynamic Table INSERT
A dynamic table starts with capacity 1. We insert 8 elements, doubling capacity each time it's full.

Potential function: `Φ(T) = 2·num − size`

1. Compute `Φ` before and after each of the 8 inserts.
2. For each insert, compute actual cost `cᵢ` (1 for normal, `m+1` for expansion).
3. Compute amortized cost: `ĉᵢ = cᵢ + Φ(Dᵢ) − Φ(Dᵢ₋₁)`.
4. Verify that every insert has amortized cost = 3 (the result from the note).

```java
// Table:
// Insert # | num (before→after) | size (before→after) | Φ(before) | Φ(after) | cᵢ | ĉᵢ
// 1        | 0 → 1              | 1 → 1              |    ?      |    ?     |  ? |  ?
// 2        | 1 → 2              | 1 → 2 (expand!)    |    ?      |    ?     |  ? |  ?
// ... (fill all 8)
```

---

### Exercise 4: Compare All Three Methods
For the MULTIPOP stack example (PUSH=1 credit, POP/MULTIPOP=0 amortized):

1. **Aggregate:** State the amortized cost per operation.
2. **Accounting:** State the credit on each stack element and verify invariant.
3. **Potential:** Let `Φ = |stack|`. Compute `ĉ` for PUSH, POP, MULTIPOP(S, k).

Confirm all three methods give the same O(1) amortized bound.

---

## Assignments

| # | Problem | Type | Key Technique |
|---|---------|------|---------------|
| 1 | **Implement a Dynamic Array (ArrayList)** | 🟡 Code | Potential method analysis |
| 2 | **Analyze Incrementing a Binary Counter** | 🟡 Theory | Aggregate analysis |
| 3 | **Accounting Method for Multi-Stack** | 🟡 Theory | Accounting method |
| 4 | **Potential Function for Splay Trees** | 🔴 Theory | Potential method (advanced) |
| 5 | **Amortized Analysis of Union-Find** | 🟡 Theory | Accounting / Ackermann |
| 6 | [Implement Dynamic Array](https://leetcode.com/problems/design-an-ordered-stream/) (LC 1656) | 🟢 Code | Dynamic table |

### Assignment Guidelines
- **Problems 1–3** are core exercises — they directly apply the three methods from this note.
- **Problem 4** (Splay Trees) is a classic but challenging potential method application. Don't attempt until comfortable with the stack and counter examples.
- **Problem 5** connects to [[02_Advanced_Data_Structures]] — the `O(m·α(n))` bound uses accounting-style arguments.
- **Target time:** 20 min per Theory problem, 30 min for code.
