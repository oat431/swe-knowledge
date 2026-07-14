---
tags:
- algorithms
- data-structures
- programming
---

# 01 Arrays & Linked Lists

The two most fundamental data structures. Arrays give O(1) random access. Linked lists give O(1) insertion/deletion at ends. Know both cold.

---

## Arrays

Contiguous memory. Fixed size (static) or resizable (dynamic).

| Operation | Time | Notes |
|-----------|:----:|-------|
| Access by index | O(1) | Direct memory address calculation |
| Search (unsorted) | O(n) | Linear scan |
| Insert at end | O(1)* | Amortized (dynamic array) |
| Insert at middle | O(n) | Shift elements right |
| Delete | O(n) | Shift elements left |

### Dynamic Array (ArrayList / Vector)

```java
ArrayList<Integer> list = new ArrayList<>();
list.add(10);         // [10]
list.add(20);         // [10, 20]
list.add(1, 15);      // [10, 15, 20]  — O(n) shift
list.remove(1);       // [10, 20]      — O(n) shift
int x = list.get(0);  // O(1)
```

### Common Patterns

| Pattern | When |
|---------|------|
| **Two pointers** | Sorted array, pair sum, remove duplicates |
| **Sliding window** | Subarray sum, longest substring |
| **Prefix sum** | Range sum queries in O(1) |
| **Binary search** | Sorted array search in O(log n) |

---

## Linked Lists

Nodes pointing to each other. No contiguous memory needed.

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}
```

| Operation | Singly | Doubly |
|-----------|:------:|:------:|
| Access by index | O(n) | O(n) |
| Insert at head | O(1) | O(1) |
| Insert at tail | O(n) / O(1)* | O(1) |
| Delete at head | O(1) | O(1) |
| Delete at tail | O(n) | O(1) |

*O(1) with tail pointer.

```java
// Reverse a linked list (iterative) — O(n) time, O(1) space
ListNode reverse(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode next = curr.next;  // Save next
        curr.next = prev;           // Reverse link
        prev = curr;                // Move forward
        curr = next;
    }
    return prev;
}
```

### Common Patterns

| Pattern | Example |
|---------|---------|
| **Fast & slow pointers** | Cycle detection, middle of list |
| **Dummy head** | Simplify edge cases (empty list, delete head) |
| **Two pointers** | Merge sorted lists, intersection |

```java
// Detect cycle (Floyd's algorithm) — O(n) time, O(1) space
boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}
```

---

## Array vs Linked List

| | Array | Linked List |
|---|:---:|:---:|
| **Random access** | O(1) | O(n) |
| **Insert/delete at ends** | O(1) amortized | O(1) |
| **Insert/delete middle** | O(n) | O(1)* |
| **Memory** | Contiguous, fixed overhead | Per-node overhead (pointer) |
| **Cache friendly** | ✅ | ❌ (scattered in memory) |
| **Use when** | Random access, fixed size, cache matters | Frequent insert/delete at ends, unknown size |

---

## Sources

- CLRS — Chapters 10.1–10.2
- LeetCode — Array / Linked List problem sets


---

## Hands-On Exercises

### Exercise 1: Two Pointers — Remove Duplicates
Given a **sorted** array, remove duplicates in-place and return the new length. Use the two-pointer technique.

```java
int removeDuplicates(int[] nums) {
    // TODO: Use two pointers (slow, fast)
    // slow = position to write next unique element
    // fast = scanning pointer
    // Return the new length
}
```

**Hint:** `slow` starts at 1, `fast` starts at 1. When `nums[fast] != nums[fast - 1]`, write to `nums[slow]`.

---

### Exercise 2: Sliding Window — Max Sum Subarray of Size K
Given an array of integers and a number `k`, find the maximum sum of a subarray of size `k`.

```java
int maxSumSubarray(int[] arr, int k) {
    // TODO: Compute sum of first k elements
    // Slide the window: add new element, remove old element
    // Track the maximum sum
}
```

**Hint:** `windowSum += arr[i] - arr[i - k]` at each slide step.

---

### Exercise 3: Linked List — Reverse a Linked List
Implement the iterative reverse algorithm shown in the note. Test with: `1 → 2 → 3 → 4 → 5` → expect `5 → 4 → 3 → 2 → 1`.

```java
ListNode reverse(ListNode head) {
    // TODO: Use prev, curr, next pointers
    // Follow the 4-step pattern from the note
}
```

---

### Exercise 4: Fast & Slow Pointers — Find Middle of Linked List
Given the head of a singly linked list, return the middle node. If two middle nodes, return the second.

```java
ListNode middleNode(ListNode head) {
    // TODO: slow moves 1 step, fast moves 2 steps
    // When fast reaches end, slow is at middle
}
```

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Two Sum](https://leetcode.com/problems/two-sum/) (LC 1) | 🟢 Easy | Array + HashMap |
| 2 | [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) (LC 26) | 🟢 Easy | Two Pointers |
| 3 | [Move Zeroes](https://leetcode.com/problems/move-zeroes/) (LC 283) | 🟢 Easy | Two Pointers |
| 4 | [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) (LC 21) | 🟢 Easy | Linked List Merge |
| 5 | [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) (LC 141) | 🟢 Easy | Fast & Slow Pointers |
| 6 | [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) (LC 206) | 🟢 Easy | Linked List |
| 7 | [3Sum](https://leetcode.com/problems/3sum/) (LC 15) | 🟡 Medium | Two Pointers + Sort |
| 8 | [Remove Nth Node From End](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) (LC 19) | 🟡 Medium | Two Pointers |
| 9 | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) (LC 11) | 🟡 Medium | Two Pointers |
| 10 | [Longest Substring Without Repeating](https://leetcode.com/problems/longest-substring-without-repeating-characters/) (LC 3) | 🟡 Medium | Sliding Window |

### Assignment Guidelines
- **Start** with problems 1–6 (Easy). These directly apply the patterns from this note.
- **Then** attempt 7–10 (Medium). These combine multiple patterns.
- **Target time:** 15 min per Easy, 25 min per Medium.
- **If stuck > 15 min:** Re-read the note section, try a simpler version of the problem, then retry.
