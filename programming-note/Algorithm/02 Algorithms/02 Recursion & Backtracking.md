---
tags:
- algorithms
- data-structures
- programming
---

# 02 Recursion & Backtracking

Recursion: a function that calls itself. Backtracking: systematically trying possibilities and abandoning dead ends. Together they solve combinatorial problems that iteration can't express cleanly.

---

## Recursion Fundamentals

Every recursive function needs:

1. **Base case** — when to stop
2. **Recursive case** — call itself with a smaller/simpler input

```java
// Factorial — classic recursion
int factorial(int n) {
    if (n <= 1) return 1;           // Base case
    return n * factorial(n - 1);    // Recursive case
}

// Fibonacci with memoization — O(n)
Map<Integer, Long> memo = new HashMap<>();
long fib(int n) {
    if (n <= 1) return n;
    if (memo.containsKey(n)) return memo.get(n);
    long result = fib(n - 1) + fib(n - 2);
    memo.put(n, result);
    return result;
}
```

---

## Recursion vs Iteration

| | Recursion | Iteration |
|---|:---:|:---:|
| **Tree/graph traversal** | ✅ Natural | ❌ Need explicit stack |
| **Stack overflow risk** | ❌ Deep recursion | ✅ Safe |
| **Space** | O(n) call stack | O(1) |
| **Readability** | ✅ Clean for divide & conquer | Better for simple loops |

---

## Backtracking

Try → fail → undo → try next. The "systematic trial and error" approach.

### Template

```java
void backtrack(List<List<Integer>> result, List<Integer> current, 
               int[] nums, boolean[] used) {
    if (/* solution found */) {
        result.add(new ArrayList<>(current));
        return;
    }
    for (/* each choice */) {
        if (/* invalid choice */) continue;
        // Choose
        current.add(choice);
        used[i] = true;
        // Explore
        backtrack(result, current, nums, used);
        // Un-choose (backtrack)
        current.remove(current.size() - 1);
        used[i] = false;
    }
}
```

### Permutations

```java
List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(result, new ArrayList<>(), nums, new boolean[nums.length]);
    return result;
}
// All permutations of [1,2,3]:
// [[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]
```

### N-Queens

Place N queens on an N×N board so no two attack each other.

```java
void solve(int row, int n, List<Integer> cols, List<List<String>> result) {
    if (row == n) {
        result.add(buildBoard(cols, n));
        return;
    }
    for (int col = 0; col < n; col++) {
        if (isValid(cols, row, col)) {
            cols.add(col);
            solve(row + 1, n, cols, result);
            cols.remove(cols.size() - 1);  // Backtrack
        }
    }
}
```

---

## Common Backtracking Problems

| Problem | Pattern |
|---------|---------|
| **Permutations** | Track used elements |
| **Combinations** | Forbid revisiting by passing start index |
| **Subsets** | Include/exclude each element |
| **Sudoku** | Try 1-9, backtrack on conflict |
| **Word search** | DFS on grid, mark visited |

---

## Pruning — Cut Dead Branches Early

Don't explore paths that can never lead to a solution.

```java
// Combination sum with pruning
if (remaining < 0) return;           // Dead branch
if (remaining < nums[start]) return; // Remaining too small
```

---

## Sources

- CLRS — Chapter 4 (Divide and Conquer)
- LeetCode — Backtracking problem sets


---

## Hands-On Exercises

### Exercise 1: Recursion — Factorial and Fibonacci
Implement both from the note. Then add memoization to Fibonacci.

```java
int factorial(int n) { /* TODO */ }
long fib(int n, Map<Integer, Long> memo) { /* TODO: with memoization */ }
```

---

### Exercise 2: Backtracking — Generate All Subsets
Given a set of distinct integers, return all possible subsets (the power set).

```java
List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    // TODO: For each element, either include it or skip it
    // Backtrack: add current subset to result, try including nums[i], recurse, remove
}
```

**Hint:** The include/exclude pattern — at each index, branch into "include nums[i]" and "exclude nums[i]".

---

### Exercise 3: Backtracking — Permutations
Implement the `permute` method from the note. Test with `[1,2,3]` → expect 6 permutations.

```java
List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    // TODO: Use the backtracking template with a `used` boolean array
}
```

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Subsets](https://leetcode.com/problems/subsets/) (LC 78) | 🟡 Medium | Backtracking (Include/Exclude) |
| 2 | [Permutations](https://leetcode.com/problems/permutations/) (LC 46) | 🟡 Medium | Backtracking (Used Array) |
| 3 | [Combination Sum](https://leetcode.com/problems/combination-sum/) (LC 39) | 🟡 Medium | Backtracking + Pruning |
| 4 | [Letter Combinations of Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) (LC 17) | 🟡 Medium | Backtracking |
| 5 | [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) (LC 22) | 🟡 Medium | Backtracking + Pruning |
| 6 | [Word Search](https://leetcode.com/problems/word-search/) (LC 79) | 🟡 Medium | DFS + Backtracking on Grid |
| 7 | [N-Queens](https://leetcode.com/problems/n-queens/) (LC 51) | 🔴 Hard | Backtracking |
| 8 | [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/) (LC 37) | 🔴 Hard | Backtracking |

### Assignment Guidelines
- **Start** with 1–2 (Medium) — the two fundamental backtracking templates.
- **Then** 3–6 (Medium) — backtracking with pruning.
- **Problems 7–8** (Hard) are the classic backtracking interview problems.
- **Key insight:** Every backtracking problem follows the same template: Choose → Explore → Un-choose.
- **Target time:** 20 min per Medium, 35 min per Hard.
