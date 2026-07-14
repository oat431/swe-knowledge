---
tags:
- algorithms
- data-structures
- programming
---

# 02 Searching

Finding an element in a collection. From simple linear scan to binary search and beyond.

---

## Linear Search

Check every element. Works on unsorted data. O(n).

```java
int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}
```

---

## Binary Search

Divide and conquer on **sorted** data. O(log n).

```java
int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;  // Avoid overflow
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

### Binary Search Patterns

| Pattern | What It Finds | Condition |
|---------|--------------|-----------|
| **Exact match** | `arr[mid] == target` | Standard |
| **First occurrence** | `arr[mid] >= target`, then `right = mid - 1` | Lower bound |
| **Last occurrence** | `arr[mid] <= target`, then `left = mid + 1` | Upper bound |
| **Insertion point** | `left` after loop | Where target should go |

```java
// Find first position ≥ target (lower bound)
int lowerBound(int[] arr, int target) {
    int left = 0, right = arr.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] >= target) right = mid;
        else left = mid + 1;
    }
    return left;  // First index where arr[i] >= target
}
```

---

## Binary Search on Answer

Not searching an array — searching a **value range** for the answer that satisfies a condition.

```java
// Find square root (floor) — O(log n)
int sqrt(int x) {
    if (x < 2) return x;
    int left = 1, right = x / 2;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        long sq = (long) mid * mid;
        if (sq == x) return mid;
        if (sq < x) left = mid + 1;
        else right = mid - 1;
    }
    return right;
}
```

### When to Use

| Problem Pattern | Example |
|----------------|---------|
| "Find the minimum/maximum X such that..." | Min capacity to ship packages |
| Monotonic condition | "If X works, X+1 also works" |
| K-th smallest/largest | K-th smallest in matrix |

---

## Search in Rotated Sorted Array

```java
int searchRotated(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) return mid;
        
        // Left half is sorted
        if (nums[left] <= nums[mid]) {
            if (nums[left] <= target && target < nums[mid]) right = mid - 1;
            else left = mid + 1;
        }
        // Right half is sorted
        else {
            if (nums[mid] < target && target <= nums[right]) left = mid + 1;
            else right = mid - 1;
        }
    }
    return -1;
}
```

---

## Search Complexity

| Algorithm | Time (Worst) | Space | Precondition |
|-----------|:----------:|:-----:|-------------|
| Linear | O(n) | O(1) | None |
| Binary | O(log n) | O(1) | Sorted |
| BFS | O(V + E) | O(V) | Graph |
| DFS | O(V + E) | O(V) | Graph |

---

## Sources

- CLRS — Chapter 2, 12
- LeetCode — Binary Search problem sets


---

## Hands-On Exercises

### Exercise 1: Binary Search — Standard Implementation
Implement binary search from the note. Test with `arr = [1,3,5,7,9,11]`, `target = 7` → expect `3`.

```java
int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    // TODO: while left <= right, compute mid, compare, narrow range
}
```

---

### Exercise 2: Lower Bound — First Position ≥ Target
Implement `lowerBound` from the note. Test with `arr = [1,3,3,5,7]`, `target = 3` → expect `1` (first index where `arr[i] >= 3`).

```java
int lowerBound(int[] arr, int target) {
    int left = 0, right = arr.length;
    // TODO: Find first index where arr[i] >= target
}
```

---

### Exercise 3: Binary Search on Answer — Koko Eating Bananas
Koko has `h` hours to eat `n` piles of bananas. She eats at speed `k` bananas/hour (one pile per hour, finishes pile moves to next). Find the minimum `k` such that she finishes all piles in `h` hours.

```java
int minEatingSpeed(int[] piles, int h) {
    int left = 1, right = Arrays.stream(piles).max().orElse(0);
    // TODO: Binary search on k
    // For each k, compute total hours = sum of ceil(pile / k)
    // If total <= h, try smaller k; else try larger k
}
```

**Hint:** This is "binary search on answer" — the condition is monotonic: if speed `k` works, then `k+1` also works.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Binary Search](https://leetcode.com/problems/binary-search/) (LC 704) | 🟢 Easy | Standard Binary Search |
| 2 | [Search Insert Position](https://leetcode.com/problems/search-insert-position/) (LC 35) | 🟢 Easy | Lower Bound |
| 3 | [First Bad Version](https://leetcode.com/problems/first-bad-version/) (LC 278) | 🟢 Easy | Lower Bound |
| 4 | [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) (LC 33) | 🟡 Medium | Modified Binary Search |
| 5 | [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) (LC 153) | 🟡 Medium | Binary Search |
| 6 | [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) (LC 875) | 🟡 Medium | Binary Search on Answer |
| 7 | [Find Peak Element](https://leetcode.com/problems/find-peak-element/) (LC 162) | 🟡 Medium | Binary Search |
| 8 | [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) (LC 74) | 🟡 Medium | Binary Search |

### Assignment Guidelines
- **Start** with 1–3 (Easy) — standard binary search and lower bound.
- **Then** 4–8 (Medium) — modified binary search and "binary search on answer" pattern.
- **Problem 6** (Koko) is the key "binary search on answer" pattern from this note.
- **Target time:** 10 min per Easy, 20 min per Medium.
