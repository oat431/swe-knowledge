---
tags:
- algorithms
- data-structures
- programming
---

# 01 Stacks & Queues

Stack = LIFO (last in, first out). Queue = FIFO (first in, first out). Both are O(1) for core operations. Simple structures, powerful patterns.

---

## Stack

```
Push:  [    ] → [ 3  ] → [ 3, 7 ] → [ 3, 7, 2 ]
Pop:   [ 3,7,2 ] → 2 → [ 3,7 ]
Peek:  [ 3,7 ] → 7 (without removing)
```

| Operation | Time |
|-----------|:----:|
| push() | O(1) |
| pop() | O(1) |
| peek() | O(1) |
| isEmpty() | O(1) |

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.push(20);
int top = stack.pop();  // 20
```

### Monotonic Stack

Maintains elements in increasing/decreasing order. Used when you need "next greater/smaller element."

```java
// Next Greater Element — O(n)
int[] nextGreater(int[] nums) {
    int[] result = new int[nums.length];
    Deque<Integer> stack = new ArrayDeque<>();
    
    for (int i = nums.length - 1; i >= 0; i--) {
        while (!stack.isEmpty() && stack.peek() <= nums[i]) {
            stack.pop();
        }
        result[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return result;
}
```

### Stack Applications

| Use Case | Pattern |
|----------|---------|
| **Balanced parentheses** | Push on '(', pop on ')' |
| **Undo/redo** | Push state on each action |
| **Function calls** | Call stack — recursion uses it implicitly |
| **Expression evaluation** | Infix → postfix, evaluate RPN |
| **DFS** | Stack-based traversal (or recursion) |

---

## Queue

```
Enqueue:  [    ] → [ A ] → [ A, B ] → [ A, B, C ]
Dequeue:  [ A,B,C ] → A → [ B, C ]
```

| Operation | Time |
|-----------|:----:|
| enqueue() | O(1) |
| dequeue() | O(1) |
| peek() | O(1) |

```java
Queue<String> queue = new LinkedList<>();
queue.offer("A");     // enqueue
queue.offer("B");
String first = queue.poll();  // dequeue → "A"
```

### Deque (Double-Ended Queue)

Insert/delete from both ends in O(1).

```java
Deque<Integer> deque = new ArrayDeque<>();
deque.addFirst(1);   // [1]
deque.addLast(2);    // [1, 2]
deque.addFirst(0);   // [0, 1, 2]
deque.removeLast();  // [0, 1]
```

### Queue Applications

| Use Case | Pattern |
|----------|---------|
| **BFS** | Level-order tree/ graph traversal |
| **Sliding window** | Deque for max/min in window |
| **Task scheduling** | Producer-consumer, thread pools |
| **LRU Cache** | Queue + Hash Map |
| **Message queues** | Kafka, RabbitMQ — at scale |

---

## Stack vs Queue

| | Stack | Queue |
|---|:---:|:---:|
| Order | LIFO | FIFO |
| DFS vs BFS | DFS | BFS |
| Backtracking | Implicit (call stack) | Not used |
| Java | `ArrayDeque` (push/pop) | `LinkedList` / `ArrayDeque` |

---

## Sources

- CLRS — Chapter 10.1
- LeetCode — Stack / Queue problem sets


---

## Hands-On Exercises

### Exercise 1: Stack — Valid Parentheses
Use a stack to check if parentheses are balanced. Handle `()`, `{}`, `[]`.

```java
boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    for (char c : s.toCharArray()) {
        // TODO: Push opening brackets
        // On closing bracket, check if top matches
    }
    return stack.isEmpty();
}
```

---

### Exercise 2: Monotonic Stack — Next Greater Element
Implement the `nextGreater` method from the note. Test with `[4,5,2,25]` → expect `[5,25,25,-1]`.

```java
int[] nextGreater(int[] nums) {
    int[] result = new int[nums.length];
    Deque<Integer> stack = new ArrayDeque<>();
    // TODO: Traverse right to left
    // Pop elements ≤ current, peek for next greater
}
```

---

### Exercise 3: Queue — Implement a Queue Using Stacks
Implement a FIFO queue using only two stacks. `push`, `pop`, `peek`, `empty`.

```java
class MyQueue {
    Deque<Integer> inStack = new ArrayDeque<>();
    Deque<Integer> outStack = new ArrayDeque<>();

    void push(int x) {
        // TODO: Always push to inStack
    }

    int pop() {
        // TODO: If outStack empty, pour all from inStack
        // Then pop from outStack
    }
}
```

**Hint:** This is the "two-stack queue" pattern. Amortized O(1) per operation.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) (LC 20) | 🟢 Easy | Stack Matching |
| 2 | [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/) (LC 232) | 🟢 Easy | Two Stacks |
| 3 | [Min Stack](https://leetcode.com/problems/min-stack/) (LC 155) | 🟡 Medium | Auxiliary Stack |
| 4 | [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/) (LC 150) | 🟡 Medium | Stack Evaluation |
| 5 | [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) (LC 739) | 🟡 Medium | Monotonic Stack |
| 6 | [Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/) (LC 503) | 🟡 Medium | Monotonic Stack (Circular) |
| 7 | [Decode String](https://leetcode.com/problems/decode-string/) (LC 394) | 🟡 Medium | Stack (Nested) |
| 8 | [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) (LC 84) | 🔴 Hard | Monotonic Stack |

### Assignment Guidelines
- **Start** with 1–2 (Easy) — basic stack/queue operations.
- **Then** 3–7 (Medium) — monotonic stack is the key pattern here.
- **Problem 8** (Hard) is a classic monotonic stack problem. Think about finding the next smaller element on both sides.
- **Target time:** 10 min per Easy, 20 min per Medium, 35 min per Hard.
