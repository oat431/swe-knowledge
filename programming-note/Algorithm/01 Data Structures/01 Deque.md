---
tags:
- algorithms
- data-structures
- programming
---

# 01 Deque

A Deque (Double-Ended Queue) supports insertion and deletion at both ends in O(1). It's the Swiss Army knife of linear data structures — use it as a stack, a queue, or a sliding window engine.

---

## ArrayDeque vs LinkedList

Java offers two `Deque` implementations. **Always prefer `ArrayDeque`** unless you need `null` elements.

| Implementation | Backing | Add/Remove | Memory | Cache |
|----------------|---------|:----------:|--------|-------|
| `ArrayDeque` | Circular array | O(1) amortized | Compact | ✅ Cache-friendly |
| `LinkedList` | Doubly-linked list | O(1) | Node overhead per element | ❌ Pointer chasing |

```java
// ✅ Good — use ArrayDeque
Deque<Integer> deque = new ArrayDeque<>();

// ❌ Bad — LinkedList is slower for deque operations
Deque<Integer> deque = new LinkedList<>();
```

---

## Operations

All core operations are **O(1)**.

| Operation | Stack Name | Queue Name | Description |
|-----------|------------|------------|-------------|
| `addFirst(e)` | `push(e)` | — | Insert at front |
| `addLast(e)` | — | `offer(e)` | Insert at back |
| `removeFirst()` | `pop()` | `poll()` | Remove from front |
| `removeLast()` | — | — | Remove from back |
| `peekFirst()` | `peek()` | `peek()` | View front |
| `peekLast()` | — | — | View back |

```java
Deque<Integer> dq = new ArrayDeque<>();
dq.addFirst(1);    // [1]
dq.addLast(2);     // [1, 2]
dq.addFirst(0);    // [0, 1, 2]
dq.removeLast();   // [0, 1]
dq.peekFirst();    // 0
dq.peekLast();     // 1
```

---

## Deque as Stack

`ArrayDeque` is **faster than `Stack`** (which is synchronized and legacy). This is the recommended way to use a stack in Java.

```java
// ✅ Good — ArrayDeque as stack
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.push(20);
int top = stack.pop();  // 20

// ❌ Bad — legacy Stack class with synchronized overhead
Stack<Integer> stack = new Stack<>();
stack.push(10);
```

| | `ArrayDeque` | `Stack` |
|---|:---:|:---:|
| Synchronized | ❌ No (faster) | ✅ Yes (slower) |
| Null elements | ❌ | ✅ |
| Recommended | ✅ | ❌ Legacy |

---

## Deque as Queue

Use `addLast` / `removeFirst` to simulate FIFO. **`ArrayDeque` is faster than `LinkedList`** for queue operations.

```java
// ✅ Good — ArrayDeque as queue
Deque<String> queue = new ArrayDeque<>();
queue.addLast("A");
queue.addLast("B");
String first = queue.removeFirst();  // "A"

// ❌ Bad — LinkedList has pointer overhead
Queue<String> queue = new LinkedList<>();
queue.offer("A");
```

---

## Monotonic Deque (Sliding Window Max)

This is the **killer use case** for deques. A monotonic deque maintains elements in decreasing (or increasing) order. For sliding window maximum, we keep a deque of indices where corresponding values are in **decreasing order**.

**How it works:**
1. For each new element, remove all smaller elements from the back
2. Remove elements that fell out of the window from the front
3. The front of the deque is always the maximum in the current window

```java
// Sliding Window Maximum — O(n) time, O(k) space
int[] maxSlidingWindow(int[] nums, int k) {
    if (nums == null || k <= 0) return new int[0];
    
    int n = nums.length;
    int[] result = new int[n - k + 1];
    Deque<Integer> deque = new ArrayDeque<>();  // stores indices
    
    for (int i = 0; i < n; i++) {
        // Remove elements outside the window
        while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
            deque.pollFirst();
        }
        
        // Remove smaller elements from back (maintain decreasing order)
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
            deque.pollLast();
        }
        
        deque.offerLast(i);
        
        // Window is full — record the max (front of deque)
        if (i >= k - 1) {
            result[i - k + 1] = nums[deque.peekFirst()];
        }
    }
    return result;
}
```

**Walkthrough** — `nums = [1, 3, -1, -3, 5, 3, 6, 7]`, `k = 3`:

| i | nums[i] | Deque (indices) | Deque (values) | Window | Max |
|---|---------|-----------------|----------------|--------|-----|
| 0 | 1 | [0] | [1] | — | — |
| 1 | 3 | [1] | [3] | — | — |
| 2 | -1 | [1, 2] | [3, -1] | [1,3,-1] | 3 |
| 3 | -3 | [1, 2, 3] | [3, -1, -3] | [3,-1,-3] | 3 |
| 4 | 5 | [4] | [5] | [-1,-3,5] | 5 |
| 5 | 3 | [4, 5] | [5, 3] | [-3,5,3] | 5 |
| 6 | 6 | [6] | [6] | [5,3,6] | 6 |
| 7 | 7 | [7] | [7] | [3,6,7] | 7 |

**Result:** `[3, 3, 5, 5, 6, 7]`

---

## When to Use

| Need | Use |
|------|-----|
| Stack (LIFO) | `ArrayDeque` — push/pop at one end |
| Queue (FIFO) | `ArrayDeque` — addLast/removeFirst |
| Max/min in sliding window | Monotonic deque |
| Palindrome check | Two-ended comparison |
| Work stealing (concurrency) | `ConcurrentLinkedDeque` |

---

## Common Patterns

| Pattern | Description |
|---------|-------------|
| **Sliding window max/min** | Monotonic decreasing/increasing deque |
| **Palindrome check** | Compare chars from both ends using `removeFirst` / `removeLast` |
| **BFS with state** | Deque-based BFS (also useful for 0-1 BFS on weighted graphs) |
| **Work stealing** | Each thread has its own deque; steal from opposite end |

> For basic stack and queue concepts, see [[01 Stacks & Queues]].

---

## Sources

- CLRS — Chapter 10.1
- Oracle Java Docs — `Deque` interface
- LeetCode 239 — Sliding Window Maximum
