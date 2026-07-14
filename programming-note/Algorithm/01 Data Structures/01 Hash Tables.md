---
tags:
- algorithms
- data-structures
- programming
---

# 01 Hash Tables

Hash tables give O(1) average-time insert, delete, and lookup. They're the most-used data structure in practice — dictionaries, caches, sets, database indexes all use hashing.

---

## How Hashing Works

```
key → hash function → index → bucket

"Alice" → hash("Alice") → 7 → bucket[7] contains ("Alice", value)
```

---

## Collision Resolution

Two keys hashing to the same index:

### Chaining (Java HashMap)

Each bucket is a linked list (or tree if > 8 collisions).

```
bucket[3]: ("Alice", 25) → ("Charlie", 30)  (same hash)
bucket[7]: ("Bob", 42)
```

```java
// Java HashMap uses chaining + red-black tree for large buckets
HashMap<String, Integer> map = new HashMap<>();
map.put("Alice", 25);
map.get("Alice");  // 25 — O(1) average
```

### Open Addressing

If bucket is taken, probe for next empty slot (linear probing, quadratic probing, double hashing).

---

## Key Concepts

| Concept | What It Means |
|---------|--------------|
| **Load factor** | entries / capacity. Default threshold = 0.75. When exceeded → resize (double capacity, rehash all). |
| **Hash function** | Maps key → integer. Must be deterministic, fast, uniform distribution. |
| **Rehashing** | When resizing, all entries are re-inserted. O(n) — but amortized over many inserts. |

```java
// Good hashCode() — uses all significant fields
@Override
public int hashCode() {
    return Objects.hash(name, email, age);
}

// Always override equals() when overriding hashCode()
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof Person)) return false;
    Person p = (Person) o;
    return Objects.equals(name, p.name) 
        && Objects.equals(email, p.email)
        && age == p.age;
}
```

---

## Common Patterns

| Pattern | Data Structure | Example |
|---------|:-------------:|---------|
| **Count/frequency** | `Map<T, Integer>` | Character count, word frequency |
| **Two-sum** | `Map<value, index>` | `target - nums[i]` lookup |
| **Group by key** | `Map<K, List<V>>` | Group anagrams, group by category |
| **Cache / memoization** | `Map<input, output>` | Fibonacci memoization |
| **Duplicate detection** | `Set<T>` | Contains duplicate? First repeating? |
| **LRU Cache** | `LinkedHashMap` or `Map + DoublyLinkedList` | Evict least recently used |

```java
// Two Sum — O(n)
int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i);
    }
    return new int[]{};
}
```

---

## Hash Table vs Other Structures

| | Hash Table | Array | Tree (BST) |
|---|:---:|:---:|:---:|
| **Search** | O(1) avg | O(log n)* | O(log n) |
| **Insert** | O(1) avg | O(n) | O(log n) |
| **Ordered?** | ❌ | ✅ | ✅ |
| **Memory** | Extra (load factor) | Compact | Per-node overhead |

---

## Sources

- CLRS — Chapter 11
- Java HashMap source — https://github.com/openjdk/jdk


---

## Hands-On Exercises

### Exercise 1: Two Sum — HashMap Pattern
Implement the Two Sum algorithm from the note. Test with `nums = [2,7,11,15]`, `target = 9` → expect `[0,1]`.

```java
int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    // TODO: For each element, check if complement exists in map
    // If yes, return indices. If no, store current element → index.
}
```

---

### Exercise 2: Frequency Count — First Non-Repeating Character
Given a string, return the index of the first non-repeating character. Return -1 if none exists.

```java
int firstUniqChar(String s) {
    // TODO: Count frequencies with HashMap or int[26]
    // Then scan string to find first char with frequency 1
}
```

---

### Exercise 3: Group By Key — Group Anagrams
Given an array of strings, group anagrams together. Two strings are anagrams if they have the same characters in the same frequencies.

```java
List<List<String>> groupAnagrams(String[] strs) {
    // TODO: Use sorted string as key, or character frequency as key
    // Map<String, List<String>> to group
}
```

**Hint:** The key can be `Arrays.toString(int[26])` for O(n) per string instead of sorting.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Two Sum](https://leetcode.com/problems/two-sum/) (LC 1) | 🟢 Easy | HashMap Lookup |
| 2 | [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) (LC 217) | 🟢 Easy | HashSet |
| 3 | [Valid Anagram](https://leetcode.com/problems/valid-anagram/) (LC 242) | 🟢 Easy | Frequency Count |
| 4 | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) (LC 49) | 🟡 Medium | HashMap Grouping |
| 5 | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) (LC 347) | 🟡 Medium | HashMap + Heap |
| 6 | [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) (LC 128) | 🟡 Medium | HashSet |
| 7 | [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) (LC 560) | 🟡 Medium | Prefix Sum + HashMap |
| 8 | [LRU Cache](https://leetcode.com/problems/lru-cache/) (LC 146) | 🟡 Medium | LinkedHashMap / Map+DLL |

### Assignment Guidelines
- **Start** with 1–3 (Easy) — these are the core HashMap patterns.
- **Then** 4–8 (Medium) — each uses a different hash table pattern.
- **LRU Cache** (problem 8) is a classic interview question. Implement it with `HashMap + DoublyLinkedList` for full understanding.
- **Target time:** 10 min per Easy, 20 min per Medium.
