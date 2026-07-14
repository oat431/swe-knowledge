---
tags:
- algorithms
- data-structures
- programming
---

# 02 Greedy Algorithms

Make the locally optimal choice at each step, hoping it leads to a globally optimal solution. Greedy algorithms are fast but don't always work — you must prove correctness.

---

## The Greedy Pattern

```java
Result greedy(Problem p) {
    Result result = new Result();
    while (/* not done */) {
        Choice c = makeBestLocalChoice(p);   // Greedy step
        if (isValid(c)) {
            result.add(c);
            p = updateProblem(p, c);
        }
    }
    return result;
}
```

---

## Classic Examples

### Activity Selection

Pick maximum number of non-overlapping activities.

```
Strategy: Always pick the activity that ENDS earliest

Activities sorted by end time:
[1,3] [2,5] [3,9] [5,8] [6,9] [8,10]
  ✓     ✗     ✗     ✓     ✗     ✓
→ Max: 3 activities
```

```java
int maxActivities(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[1] - b[1]);  // Sort by end time
    int count = 1;
    int lastEnd = intervals[0][1];
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= lastEnd) {
            count++;
            lastEnd = intervals[i][1];
        }
    }
    return count;
}
```

---

### Fractional Knapsack

Take fractions of items to maximize value within weight limit.

```
Strategy: Take items with highest value/weight ratio first

Items: [(60,10), (100,20), (120,30)]  — (value, weight)
Ratios: [6, 5, 4]
Capacity: 50

→ Take all of (60,10) + all of (100,20) + 2/3 of (120,30)
→ Total value: 60 + 100 + 80 = 240
```

> ⚠️ Greedy works for **fractional** knapsack. For **0/1 knapsack** (can't take fractions), use Dynamic Programming.

---

### Huffman Coding

Build optimal prefix-free binary codes for data compression.

```
Frequency:  a=45, b=13, c=12, d=16, e=9, f=5

Strategy: repeatedly merge two nodes with smallest frequency
```

---

### Dijkstra's Algorithm

Shortest path from source to all nodes (non-negative weights).

```
Strategy: always expand the unvisited node with smallest distance
```

---

## When Greedy Works

| Criteria | Why |
|----------|-----|
| **Optimal substructure** | Optimal solution contains optimal solutions to subproblems |
| **Greedy choice property** | Locally optimal choice leads to globally optimal solution |

> **You must PROVE the greedy choice property holds.** Not all problems with optimal substructure can be solved greedily — some require DP.

---

## Greedy vs DP

| | Greedy | Dynamic Programming |
|---|:---:|:---:|
| **Choices** | One choice, never reconsider | Consider all choices |
| **Speed** | O(n log n) typical | O(n²) or O(n·W) typical |
| **Proof needed?** | Yes (correctness) | No (exhaustive) |
| **Examples** | Activity selection, Dijkstra, Huffman | Knapsack 0/1, LCS, Edit distance |

---

## Sources

- CLRS — Chapter 16
- LeetCode — Greedy problem sets


---

## Hands-On Exercises

### Exercise 1: Activity Selection — Implement from Note
Implement the `maxActivities` method from the note. Test with intervals `[[1,3],[2,5],[3,9],[5,8],[6,9],[8,10]]` → expect `3`.

```java
int maxActivities(int[][] intervals) {
    // TODO: Sort by end time
    // Greedily pick activities that don't overlap with last picked
}
```

---

### Exercise 2: Jump Game — Greedy Reach
Given an array where each element is the maximum jump length from that position, determine if you can reach the last index.

```java
boolean canJump(int[] nums) {
    int maxReach = 0;
    // TODO: For each index i, update maxReach = max(maxReach, i + nums[i])
    // If maxReach >= last index, return true
    // If i > maxReach, stuck — return false
}
```

**Hint:** This is a greedy approach — always track the farthest you can reach.

---

### Exercise 3: Greedy vs DP — When Does Greedy Fail?
Consider the **0/1 Knapsack** problem: items `[(60,10), (100,20), (120,30)]`, capacity = 50.

1. Solve with **greedy** (highest value/weight ratio first). What value do you get?
2. Solve with **DP** (from the DP note). What value do you get?
3. Why does greedy fail here but works for **fractional** knapsack?

Write your analysis as comments in your code.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) (LC 121) | 🟢 Easy | Greedy (track min) |
| 2 | [Jump Game](https://leetcode.com/problems/jump-game/) (LC 55) | 🟡 Medium | Greedy Reach |
| 3 | [Jump Game II](https://leetcode.com/problems/jump-game-ii/) (LC 45) | 🟡 Medium | Greedy (BFS-like) |
| 4 | [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) (LC 435) | 🟡 Medium | Activity Selection |
| 5 | [Partition Labels](https://leetcode.com/problems/partition-labels/) (LC 763) | 🟡 Medium | Greedy (last occurrence) |
| 6 | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) (LC 621) | 🟡 Medium | Greedy (frequency) |
| 7 | [Minimum Number of Arrows](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) (LC 452) | 🟡 Medium | Interval Greedy |
| 8 | [Candy](https://leetcode.com/problems/candy/) (LC 135) | 🔴 Hard | Greedy (two passes) |

### Assignment Guidelines
- **Start** with 1 (Easy) — simple greedy tracking.
- **Then** 2–7 (Medium) — each applies greedy in a different way.
- **Problem 8** (Candy) is a tricky greedy problem requiring two passes.
- **Key insight:** Greedy works when the **greedy choice property** holds — always prove it first!
- **Target time:** 10 min per Easy, 20 min per Medium, 30 min per Hard.
