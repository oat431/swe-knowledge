---
tags:
- algorithms
- data-structures
- programming
---

# 02 Dynamic Programming

DP solves problems by breaking them into **overlapping subproblems** and storing results to avoid recomputation. If you find yourself solving the same subproblem repeatedly, DP is the answer.

---

## Two Approaches

| | Top-Down (Memoization) | Bottom-Up (Tabulation) |
|---|:---:|:---:|
| **Direction** | Problem → subproblems | Base cases → problem |
| **Implementation** | Recursion + cache | Iteration + table |
| **Space** | O(n) call stack + cache | O(n) table only |
| **When** | Easier to write, natural from recurrence | Better performance, no recursion limit |

```java
// Fibonacci — Top-Down
long fib(int n, Map<Integer, Long> memo) {
    if (n <= 1) return n;
    if (memo.containsKey(n)) return memo.get(n);
    long result = fib(n-1, memo) + fib(n-2, memo);
    memo.put(n, result);
    return result;
}

// Fibonacci — Bottom-Up
long fib(int n) {
    if (n <= 1) return n;
    long[] dp = new long[n + 1];
    dp[0] = 0; dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    return dp[n];
}
```

---

## Classic DP Problems

### 0/1 Knapsack

Maximize value given weight capacity. Each item: take or leave.

```java
// dp[i][w] = max value with first i items and capacity w
int knapsack(int[] values, int[] weights, int capacity) {
    int n = values.length;
    int[][] dp = new int[n + 1][capacity + 1];
    
    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= capacity; w++) {
            if (weights[i-1] <= w) {
                dp[i][w] = Math.max(
                    values[i-1] + dp[i-1][w - weights[i-1]],  // Take
                    dp[i-1][w]                                   // Leave
                );
            } else {
                dp[i][w] = dp[i-1][w];
            }
        }
    }
    return dp[n][capacity];
}
// Time: O(n·W), Space: O(n·W) → optimize to O(W) with 1D array
```

### Longest Common Subsequence (LCS)

```java
// dp[i][j] = LCS of s1[0..i] and s2[0..j]
int lcs(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    int[][] dp = new int[m + 1][n + 1];
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i-1) == s2.charAt(j-1)) {
                dp[i][j] = 1 + dp[i-1][j-1];
            } else {
                dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
    }
    return dp[m][n];
}
```

### Coin Change (Minimum Coins)

```java
// dp[i] = minimum coins to make amount i
int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);
    dp[0] = 0;
    
    for (int i = 1; i <= amount; i++) {
        for (int coin : coins) {
            if (coin <= i) {
                dp[i] = Math.min(dp[i], 1 + dp[i - coin]);
            }
        }
    }
    return dp[amount] > amount ? -1 : dp[amount];
}
```

---

## Recognizing DP Problems

| Signal | Example |
|--------|---------|
| "Minimum/Maximum..." | Min coins, max profit |
| "Number of ways..." | Unique paths, coin change 2 |
| "Longest..." | LCS, LIS |
| "Is it possible..." | Subset sum, word break |
| Overlapping subproblems | Fibonacci, grid paths |
| Two sequences/strings | LCS, edit distance |

---

## DP Optimization Techniques

| Technique | When | Effect |
|-----------|------|--------|
| **1D instead of 2D** | dp[i] only depends on dp[i-1] | O(n) space |
| **State compression** | Small constraints (bitmask) | O(2ⁿ) feasible for n ≤ 20 |
| **Prefix sums** | Range queries in DP | O(1) range sum |

---

## Sources

- CLRS — Chapter 15
- LeetCode — Dynamic Programming problem sets
- Bellman, Richard. *Dynamic Programming*, 1957.


---

## Hands-On Exercises

### Exercise 1: Fibonacci — Both Approaches
Implement both top-down (memoization) and bottom-up (tabulation) from the note. Compare them.

```java
// Top-Down
long fibTD(int n, Map<Integer, Long> memo) { /* TODO */ }

// Bottom-Up
long fibBU(int n) { /* TODO: dp array, fill from 0 to n */ }

// Space-Optimized Bottom-Up (only 2 variables)
long fibOptimized(int n) { /* TODO */ }
```

---

### Exercise 2: Coin Change — Implement from Note
Implement the `coinChange` method from the note. Test with `coins = [1,5,10,25]`, `amount = 30` → expect `2` (25 + 5).

```java
int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    // TODO: Fill dp[0] = 0, dp[i] = min(dp[i], 1 + dp[i - coin]) for each coin
}
```

---

### Exercise 3: Climbing Stairs — Classic DP Intro
You can climb 1 or 2 steps at a time. How many distinct ways to reach step `n`? This is Fibonacci in disguise.

```java
int climbStairs(int n) {
    // TODO: dp[i] = dp[i-1] + dp[i-2]
    // Base: dp[1] = 1, dp[2] = 2
}
```

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) (LC 70) | 🟢 Easy | 1D DP (Fibonacci) |
| 2 | [House Robber](https://leetcode.com/problems/house-robber/) (LC 198) | 🟡 Medium | 1D DP |
| 3 | [Coin Change](https://leetcode.com/problems/coin-change/) (LC 322) | 🟡 Medium | 1D DP (Unbounded Knapsack) |
| 4 | [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) (LC 300) | 🟡 Medium | 1D DP / Binary Search |
| 5 | [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) (LC 1143) | 🟡 Medium | 2D DP |
| 6 | [0/1 Knapsack](https://www.geeksforgeeks.org/0-1-knapsack-problem-dp-10/) (GFG) | 🟡 Medium | 2D DP |
| 7 | [Edit Distance](https://leetcode.com/problems/edit-distance/) (LC 72) | 🟡 Medium | 2D DP |
| 8 | [Word Break](https://leetcode.com/problems/word-break/) (LC 139) | 🟡 Medium | 1D DP + HashSet |
| 9 | [Unique Paths](https://leetcode.com/problems/unique-paths/) (LC 62) | 🟡 Medium | 2D DP |

### Assignment Guidelines
- **Start** with 1 (Easy) — recognize the Fibonacci pattern.
- **Then** 2–4 (Medium, 1D DP) — single-sequence DP.
- **Then** 5–9 (Medium, 2D DP) — two-sequence or grid DP.
- **Key insight:** If you see "minimum/maximum/number of ways/longest" + overlapping subproblems → it's DP.
- **Target time:** 10 min per Easy, 25 min per Medium.
