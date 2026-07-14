---
tags:
- algorithms
- data-structures
- programming
---

# 03 Complexity Analysis & Problem-Solving Patterns

Most interview problems fit a known pattern. Recognizing the pattern unlocks the optimal solution. This file covers Big-O analysis and the 10 most common problem-solving patterns.

---

## Big-O Complexity

How runtime/space grows as input size grows.

| Notation | Name | Example |
|----------|------|---------|
| **O(1)** | Constant | Array access, hash table lookup |
| **O(log n)** | Logarithmic | Binary search |
| **O(n)** | Linear | Linear search, single loop |
| **O(n log n)** | Linearithmic | Merge sort, quick sort (avg) |
| **O(n²)** | Quadratic | Nested loops, bubble sort |
| **O(2ⁿ)** | Exponential | Subsets, brute-force combinatorics |
| **O(n!)** | Factorial | Permutations |

### Quick Estimation

```
n = 10        → O(n!), O(2ⁿ), O(n³) all fine
n = 100       → O(n³) max
n = 1,000     → O(n²) max
n = 10,000    → O(n²) borderline
n = 100,000   → O(n log n) target
n = 1,000,000 → O(n) or O(log n)
```

---

## 10 Essential Patterns

### 1. Two Pointers

Two indices moving through a sorted array. Opposite ends or same direction.

```java
// Pair sum in sorted array — O(n)
int[] pairSum(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == target) return new int[]{left, right};
        if (sum < target) left++;
        else right--;
    }
    return new int[]{-1, -1};
}
```

### 2. Sliding Window

Maintain a window that slides across the array.

```java
// Max sum subarray of size k — O(n)
int maxSum(int[] arr, int k) {
    int windowSum = 0, maxSum = 0;
    for (int i = 0; i < k; i++) windowSum += arr[i];
    maxSum = windowSum;
    for (int i = k; i < arr.length; i++) {
        windowSum += arr[i] - arr[i - k];
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```

### 3. Fast & Slow Pointers

One pointer moves twice as fast. Cycle detection, middle of list.

### 4. Merge Intervals

Sort by start, then merge overlapping.

```java
// Merge overlapping intervals — O(n log n)
int[][] merge(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    List<int[]> result = new ArrayList<>();
    int[] curr = intervals[0];
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] <= curr[1]) {
            curr[1] = Math.max(curr[1], intervals[i][1]);
        } else {
            result.add(curr);
            curr = intervals[i];
        }
    }
    result.add(curr);
    return result.toArray(new int[0][]);
}
```

### 5. Modified Binary Search

Search an answer space, not an array.

### 6. BFS / DFS on Matrix

Grid traversal. Number of islands, shortest path in maze.

### 7. Topological Sort

Dependency ordering. Course schedule, build systems.

### 8. Prefix Sum

Precompute cumulative sums for O(1) range queries.

```java
// Range sum queries — build O(n), query O(1)
int[] prefix = new int[nums.length + 1];
for (int i = 0; i < nums.length; i++) {
    prefix[i + 1] = prefix[i] + nums[i];
}
// Sum of nums[L..R] = prefix[R+1] - prefix[L]
```

### 9. Union-Find (Disjoint Set)

Dynamic connectivity. Friend circles, redundant connections.

```java
class UnionFind {
    int[] parent, rank;
    UnionFind(int n) {
        parent = new int[n]; rank = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);  // Path compression
        return parent[x];
    }
    void union(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return;
        if (rank[px] < rank[py]) parent[px] = py;
        else if (rank[px] > rank[py]) parent[py] = px;
        else { parent[py] = px; rank[px]++; }
    }
}
```

### 10. Trie (Prefix Tree)

Efficient string search. Autocomplete, spell checker.

---

## Problem-Solving Framework

```
1. Clarify the problem (inputs, outputs, constraints)
2. Brute force first (prove it works, establish baseline)
3. Identify the pattern (two pointers? DP? sliding window?)
4. Optimize (data structure choice, eliminate loops)
5. Code clean solution
6. Test edge cases (empty, single element, duplicates, overflow)
```

---

## Sources

- LeetCode Explore cards — Patterns
- Grokking the Coding Interview — https://www.educative.io/courses/grokking-the-coding-interview


---

## Hands-On Exercises

### Exercise 1: Two Pointers — Container With Most Water
Given `n` non-negative integers representing heights, find two lines that together with the x-axis form a container that holds the most water.

```java
int maxArea(int[] height) {
    int left = 0, right = height.length - 1;
    int maxArea = 0;
    // TODO: Move the shorter pointer inward, update maxArea
}
```

**Hint:** Area = `min(height[left], height[right]) * (right - left)`.

---

### Exercise 2: Sliding Window — Longest Substring Without Repeating Characters
Given a string, find the length of the longest substring without repeating characters.

```java
int lengthOfLongestSubstring(String s) {
    // TODO: Use a HashSet or HashMap to track characters in window
    // Expand right, shrink left when duplicate found
}
```

---

### Exercise 3: Merge Intervals — Implement from Note
Implement the `merge` method from the note. Test with `[[1,3],[2,6],[8,10],[15,18]]` → expect `[[1,6],[8,10],[15,18]]`.

```java
int[][] merge(int[][] intervals) {
    // TODO: Sort by start, then merge overlapping
}
```

---

### Exercise 4: Prefix Sum — Subarray Sum Equals K
Given an array of integers and an integer `k`, find the total number of continuous subarrays whose sum equals `k`.

```java
int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> prefixCount = new HashMap<>();
    prefixCount.put(0, 1);  // Empty prefix
    // TODO: Build prefix sum, check if (prefixSum - k) exists in map
}
```

**Hint:** This combines prefix sum with the HashMap pattern.

---

### Exercise 5: Big-O Estimation — Practice
For each code snippet, determine the Big-O complexity:

```java
// Snippet A
for (int i = 0; i < n; i++)
    for (int j = i; j < n; j++)
        arr[j]++;                    // Answer: ?

// Snippet B
int i = 1;
while (i < n) i *= 2;               // Answer: ?

// Snippet C
for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        for (int k = 0; k < n; k++)  // Answer: ?
            arr[k]++;
```

Write your answers as comments, then verify with the quick estimation table in the note.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Two Sum](https://leetcode.com/problems/two-sum/) (LC 1) | 🟢 Easy | HashMap |
| 2 | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) (LC 53) | 🟡 Medium | Kadane's / DP |
| 3 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) (LC 56) | 🟡 Medium | Sort + Merge |
| 4 | [Longest Substring Without Repeating](https://leetcode.com/problems/longest-substring-without-repeating-characters/) (LC 3) | 🟡 Medium | Sliding Window |
| 5 | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) (LC 11) | 🟡 Medium | Two Pointers |
| 6 | [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) (LC 560) | 🟡 Medium | Prefix Sum + HashMap |
| 7 | [Number of Islands](https://leetcode.com/problems/number-of-islands/) (LC 200) | 🟡 Medium | BFS/DFS |
| 8 | [Course Schedule](https://leetcode.com/problems/course-schedule/) (LC 207) | 🟡 Medium | Topological Sort |
| 9 | [LRU Cache](https://leetcode.com/problems/lru-cache/) (LC 146) | 🟡 Medium | HashMap + DLL |
| 10 | [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) (LC 4) | 🔴 Hard | Binary Search |

### Assignment Guidelines
- **These problems cover all 10 patterns** from this note. Each problem maps to a specific pattern.
- **Start** with 1 (Easy).
- **Then** 2–9 (Medium) — try to identify which pattern each problem uses before solving.
- **Problem 10** (Hard) — the ultimate binary search challenge.
- **After completing all:** Review which patterns you found easiest/hardest. Focus more practice on weak patterns.
- **Target time:** 10 min per Easy, 20 min per Medium, 40 min for Hard.
