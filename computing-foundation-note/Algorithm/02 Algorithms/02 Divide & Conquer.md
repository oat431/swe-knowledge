---
tags:
- algorithms
- data-structures
- programming
---

# 02 Divide & Conquer

Split the problem into smaller subproblems, solve them recursively, combine the results. Three steps: **Divide → Conquer → Combine**.

---

## The Pattern

```java
Result divideAndConquer(Problem p) {
    if (p is small enough) {
        return solveDirectly(p);           // Base case
    }
    SubProblem[] subs = divide(p);          // Divide
    Result[] results = new Result[subs.length];
    for (int i = 0; i < subs.length; i++) {
        results[i] = divideAndConquer(subs[i]);  // Conquer (recursively)
    }
    return combine(results);                // Combine
}
```

---

## Classic Examples

### Merge Sort

```
Divide:  split array in half
Conquer: recursively sort each half
Combine: merge two sorted halves

[38,27,43,3,9,82,10]
       ↙           ↘
[38,27,43,3]   [9,82,10]
    ↙    ↘        ↙    ↘
[38,27] [43,3] [9,82] [10]
```

### Quick Sort

```
Divide:  partition around pivot
Conquer: recursively sort left and right of pivot
Combine: nothing — sorting happens in-place during partition
```

### Binary Search

```
Divide:  compare target with middle element
Conquer: search in left OR right half (only one!)
Combine: nothing — result propagates up
```

---

## Master Theorem

For recurrences of the form: $T(n) = a \cdot T(n/b) + f(n)$

| Case | Condition | Result |
|------|-----------|--------|
| 1 | $f(n) = O(n^{\log_b a - \epsilon})$ | $T(n) = \Theta(n^{\log_b a})$ |
| 2 | $f(n) = \Theta(n^{\log_b a})$ | $T(n) = \Theta(n^{\log_b a} \log n)$ |
| 3 | $f(n) = \Omega(n^{\log_b a + \epsilon})$ | $T(n) = \Theta(f(n))$ |

### Applying to Common Algorithms

| Algorithm | Recurrence | a | b | f(n) | Result |
|-----------|-----------|---|---|------|--------|
| Binary Search | T(n) = T(n/2) + O(1) | 1 | 2 | O(1) | O(log n) |
| Merge Sort | T(n) = 2T(n/2) + O(n) | 2 | 2 | O(n) | O(n log n) |
| Quick Sort (avg) | T(n) = 2T(n/2) + O(n) | 2 | 2 | O(n) | O(n log n) |
| Strassen Matrix | T(n) = 7T(n/2) + O(n²) | 7 | 2 | O(n²) | O(n^2.81) |

---

## When to Use Divide & Conquer

| ✅ Good Fit | ❌ Not a Good Fit |
|------------|------------------|
| Problem can be split into independent subproblems | Subproblems overlap (use DP instead) |
| Combining solutions is efficient | Splitting is more expensive than solving directly |
| Parallelizable | Subproblems must be solved sequentially |

---

## Sources

- CLRS — Chapter 4
- LeetCode — Divide & Conquer problem sets


---

## Hands-On Exercises

### Exercise 1: Merge Sort — Full Implementation
Implement the complete merge sort from the note. Sort `[38,27,43,3,9,82,10]`.

```java
void mergeSort(int[] arr, int left, int right) {
    // TODO: Base case (left >= right)
    // Divide: compute mid
    // Conquer: recursively sort left and right halves
    // Combine: merge the two sorted halves
}
```

---

### Exercise 2: Maximum Subarray (Kadane's / D&C)
Given an integer array, find the contiguous subarray with the largest sum. Solve it two ways:

```java
// Way 1: Kadane's algorithm — O(n) (greedy/DP approach)
int maxSubArray(int[] nums) { /* TODO */ }

// Way 2: Divide & Conquer — O(n log n)
int maxSubArrayDC(int[] nums, int left, int right) {
    // TODO: Divide at mid
    // Max is either: (a) entirely in left half, (b) entirely in right half,
    //                (c) crossing the mid point
}
```

**Hint:** For D&C version, the cross-mid case is: max left suffix + max right prefix.

---

### Exercise 3: Master Theorem — Identify Complexity
For each recurrence, identify which Master Theorem case applies and state the result:

1. `T(n) = 2T(n/2) + O(n)` → Answer: ?
2. `T(n) = T(n/2) + O(1)` → Answer: ?
3. `T(n) = 4T(n/2) + O(n²)` → Answer: ?

Check your answers against the table in the note.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Sort an Array](https://leetcode.com/problems/sort-an-array/) (LC 912) | 🟡 Medium | Merge Sort |
| 2 | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) (LC 53) | 🟡 Medium | D&C / Kadane's |
| 3 | [Majority Element](https://leetcode.com/problems/majority-element/) (LC 169) | 🟢 Easy | D&C / Boyer-Moore |
| 4 | [Kth Largest Element](https://leetcode.com/problems/kth-largest-element-in-an-array/) (LC 215) | 🟡 Medium | Quickselect (D&C) |
| 5 | [Different Ways to Add Parentheses](https://leetcode.com/problems/different-ways-to-add-parentheses/) (LC 241) | 🟡 Medium | D&C Recursion |
| 6 | [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) (LC 4) | 🔴 Hard | Binary Search D&C |

### Assignment Guidelines
- **Start** with 1–3 — merge sort and D&C fundamentals.
- **Then** 4–5 — D&C applications.
- **Problem 6** (Median of Two Sorted Arrays) is one of the hardest classic interview problems. It requires binary search on partitioning.
- **Target time:** 15 min per Easy/Medium, 40 min for Hard.
