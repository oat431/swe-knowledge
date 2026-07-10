---
tags:
- algorithms
- strings
- programming
---

# 02 String Algorithms

String matching is everywhere — search engines, DNA sequencing, log parsing, compilers. A naive approach works for small inputs, but real-world text processing demands algorithms that avoid re-scanning the same characters. This note covers the three fundamental pattern-matching algorithms and common string techniques.

---

## Naive Pattern Matching

The simplest approach: slide the pattern across the text one character at a time, checking for a full match at each position.

```java
// Naive pattern search — O(n * m) time
List<Integer> naiveSearch(String text, String pattern) {
    List<Integer> matches = new ArrayList<>();
    int n = text.length(), m = pattern.length();
    
    for (int i = 0; i <= n - m; i++) {
        int j = 0;
        while (j < m && text.charAt(i + j) == pattern.charAt(j)) {
            j++;
        }
        if (j == m) matches.add(i);
    }
    return matches;
}
```

❌ **Problem:** When a mismatch occurs after matching many characters, the naive approach slides by 1 and re-compares everything. For `text = "AAAAAAB"`, `pattern = "AAAB"`, it re-checks overlapping `AAA` multiple times.

| Time | Space | Best for |
|:----:|:-----:|----------|
| O(n × m) | O(1) | Tiny patterns, one-off searches |

---

## KMP (Knuth-Morris-Pratt)

KMP eliminates redundant comparisons by **pre-analyzing the pattern**. It builds a failure function (LPS array) that tells us exactly how far to shift on a mismatch — without backtracking in the text.

### The LPS Array (Longest Proper Prefix which is also Suffix)

`lps[i]` = length of the longest proper prefix of `pattern[0..i]` that is also a suffix.

For pattern `"AABAAAC"`:

| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------|---|---|---|---|---|---|---|
| Char  | A | A | B | A | A | A | C |
| LPS   | 0 | 1 | 0 | 1 | 2 | 2 | 0 |

At index 4 (`A`), `"AA"` is both a prefix and suffix of `"AABAA"` → `lps[4] = 2`.

### Full Implementation

```java
// KMP Pattern Search — O(n + m) time, O(m) space
List<Integer> kmpSearch(String text, String pattern) {
    List<Integer> matches = new ArrayList<>();
    int n = text.length(), m = pattern.length();
    if (m == 0) return matches;
    
    // Step 1: Build LPS array
    int[] lps = buildLPS(pattern);
    
    // Step 2: Search using LPS to skip
    int i = 0, j = 0;  // i = text pointer, j = pattern pointer
    while (i < n) {
        if (text.charAt(i) == pattern.charAt(j)) {
            i++;
            j++;
        }
        if (j == m) {
            matches.add(i - j);  // Match found
            j = lps[j - 1];      // Continue searching
        } else if (i < n && text.charAt(i) != pattern.charAt(j)) {
            if (j != 0) {
                j = lps[j - 1];  // Jump using LPS — no backtracking in text!
            } else {
                i++;
            }
        }
    }
    return matches;
}

int[] buildLPS(String pattern) {
    int m = pattern.length();
    int[] lps = new int[m];
    int len = 0;  // length of previous longest prefix-suffix
    int i = 1;
    
    while (i < m) {
        if (pattern.charAt(i) == pattern.charAt(len)) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1];  // Fall back — don't increment i
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
    return lps;
}
```

### How KMP Avoids Re-Scanning

```
Text:    A B A B A A B A A B C
Pattern: A B A B A A B C
                  ↑ mismatch at index 7

LPS tells us: pattern[0..6] = "ABABAAB" has prefix-suffix "AB" of length 2
→ Jump j from 7 to 2 (lps[6] = 2)
→ Continue comparing from pattern[2], text pointer stays at 7
→ No backtracking!
```

| Time | Space | Best for |
|:----:|:-----:|----------|
| O(n + m) | O(m) | Single pattern in large text |

---

## Rabin-Karp

Rabin-Karp uses a **rolling hash** to compare the pattern against text windows in O(1) per shift. Only when hashes match do we verify character-by-character (to handle hash collisions).

### Rolling Hash Concept

```
Hash of "ABC" = A×base² + B×base¹ + C×base⁰

To slide from "ABC" to "BCD":
- Remove 'A' contribution:  hash -= A × base²
- Multiply by base:         hash ×= base
- Add 'D' contribution:    hash += D
→ O(1) update!
```

### Full Implementation

```java
// Rabin-Karp — O(n + m) average, O(nm) worst case
List<Integer> rabinKarpSearch(String text, String pattern) {
    List<Integer> matches = new ArrayList<>();
    int n = text.length(), m = pattern.length();
    if (m > n) return matches;
    
    long BASE = 256;
    long MOD = 1_000_000_007;
    
    // Precompute BASE^(m-1) mod MOD
    long basePow = 1;
    for (int i = 0; i < m - 1; i++) {
        basePow = (basePow * BASE) % MOD;
    }
    
    // Compute initial hashes
    long patHash = 0, winHash = 0;
    for (int i = 0; i < m; i++) {
        patHash = (patHash * BASE + pattern.charAt(i)) % MOD;
        winHash = (winHash * BASE + text.charAt(i)) % MOD;
    }
    
    // Slide the window
    for (int i = 0; i <= n - m; i++) {
        if (winHash == patHash) {
            // Hash match — verify character by character
            if (text.substring(i, i + m).equals(pattern)) {
                matches.add(i);
            }
        }
        // Roll the hash: slide window right
        if (i < n - m) {
            winHash = (winHash - text.charAt(i) * basePow % MOD + MOD) % MOD;
            winHash = (winHash * BASE + text.charAt(i + m)) % MOD;
        }
    }
    return matches;
}
```

### When to Use Rabin-Karp

| Scenario | Why Rabin-Karp |
|----------|---------------|
| **Multiple pattern search** | Compute pattern hashes once, check all against text in one pass |
| **Plagiarism detection** | Hash substrings, compare hash sets |
| **2D pattern matching** | Extend rolling hash to rows and columns |

| Time (average) | Time (worst) | Space | Best for |
|:--------------:|:------------:|:-----:|----------|
| O(n + m) | O(n × m) | O(1) | Multiple patterns, substring hashing |

---

## Algorithm Comparison

| | Naive | KMP | Rabin-Karp |
|---|:---:|:---:|:---:|
| **Time (best)** | O(n) | O(n) | O(n + m) |
| **Time (average)** | O(n × m) | O(n + m) | O(n + m) |
| **Time (worst)** | O(n × m) | O(n + m) | O(n × m) |
| **Space** | O(1) | O(m) | O(1) |
| **Preprocessing** | None | LPS array | Rolling hash |
| **Multiple patterns** | ❌ | ❌ (need Aho-Corasick) | ✅ Hash all patterns |
| **Best for** | Tiny text | Single pattern, large text | Multiple patterns, hashing |

---

## Common String Patterns

### Anagram Detection

```java
// ✅ Frequency count — O(n)
boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    int[] freq = new int[26];
    for (int i = 0; i < s.length(); i++) {
        freq[s.charAt(i) - 'a']++;
        freq[t.charAt(i) - 'a']--;
    }
    for (int f : freq) {
        if (f != 0) return false;
    }
    return true;
}

// ❌ Sorting — O(n log n), slower
boolean isAnagramSort(String s, String t) {
    char[] a = s.toCharArray(), b = t.toCharArray();
    Arrays.sort(a);
    Arrays.sort(b);
    return Arrays.equals(a, b);
}
```

### Longest Common Substring

Use **dynamic programming** — `dp[i][j]` = length of longest common suffix of `s1[0..i-1]` and `s2[0..j-1]`. Time O(n × m), Space O(n × m).

### String Hashing

Rolling hash (as in Rabin-Karp) generalizes to:
- **Substring equality check** in O(1) after O(n) preprocessing
- **Longest common substring** in O(n log n) with binary search + hashing
- **Palindrome detection** with forward + reverse hash comparison

> For Python: use `hashlib` for cryptographic hashes, or implement custom rolling hash for competitive programming.

---

## Sources

- CLRS — Chapter 32 (String Matching)
- Sedgewick — Algorithms, Chapter 5.3
- Knuth, Morris, Pratt. "Fast Pattern Matching in Strings," 1977.
