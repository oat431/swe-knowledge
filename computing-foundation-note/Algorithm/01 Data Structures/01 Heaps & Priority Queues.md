---
tags:
- algorithms
- data-structures
- programming
---

# 01 Heaps & Priority Queues

A heap is a complete binary tree where every parent is ordered relative to its children. Min-heap: parent ≤ children. Max-heap: parent ≥ children. The smallest/largest element is always at the root.

---

## Heap Operations

| Operation | Time |
|-----------|:----:|
| peek() — get min/max | O(1) |
| insert() — push + bubble up | O(log n) |
| poll() — remove root + bubble down | O(log n) |
| heapify() — build from array | O(n) |

```java
// Java PriorityQueue (min-heap by default)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(5);
minHeap.offer(2);
minHeap.offer(8);
minHeap.peek();  // 2 (smallest)
minHeap.poll();  // removes 2

// Max-heap
PriorityQueue<Integer> maxHeap = 
    new PriorityQueue<>(Collections.reverseOrder());
```

---

## Heap Internals

Stored as an array (no pointers needed — complete tree property):

```
Array index:
  parent = (i - 1) / 2
  left child = 2 * i + 1
  right child = 2 * i + 2

        2           ← index 0
       / \
      5   8         ← index 1, 2
     /
    9               ← index 3
```

```java
// Custom heap (min-heap)
class MinHeap {
    private List<Integer> heap = new ArrayList<>();
    
    void insert(int val) {
        heap.add(val);
        bubbleUp(heap.size() - 1);
    }
    
    private void bubbleUp(int i) {
        while (i > 0) {
            int parent = (i - 1) / 2;
            if (heap.get(i) >= heap.get(parent)) break;
            Collections.swap(heap, i, parent);
            i = parent;
        }
    }
    
    int poll() {
        int min = heap.get(0);
        heap.set(0, heap.remove(heap.size() - 1));
        bubbleDown(0);
        return min;
    }
    
    private void bubbleDown(int i) {
        int size = heap.size();
        while (true) {
            int left = 2 * i + 1, right = 2 * i + 2;
            int smallest = i;
            if (left < size && heap.get(left) < heap.get(smallest)) smallest = left;
            if (right < size && heap.get(right) < heap.get(smallest)) smallest = right;
            if (smallest == i) break;
            Collections.swap(heap, i, smallest);
            i = smallest;
        }
    }
}
```

---

## Applications

| Problem | Pattern |
|---------|---------|
| **Top K elements** | Min-heap of size K. For each new element: if > heap.peek(), poll + insert. |
| **K-th largest/smallest** | Heap of size K |
| **Merge K sorted lists** | Min-heap of list heads |
| **Median of stream** | Two heaps: max-heap (lower half) + min-heap (upper half) |
| **Dijkstra's algorithm** | Min-heap for shortest path |
| **Task scheduler** | Max-heap by frequency + cooldown |

### Top K Elements

```java
// Find K largest elements in array — O(n log k)
int[] topK(int[] nums, int k) {
    PriorityQueue<Integer> heap = new PriorityQueue<>(); // min-heap
    for (int n : nums) {
        heap.offer(n);
        if (heap.size() > k) heap.poll();
    }
    return heap.stream().mapToInt(i -> i).toArray();
}
```

---

## Heap Sort

Build heap O(n) + extract n times O(n log n) = **O(n log n)** total. In-place, not stable.

```java
void heapSort(int[] arr) {
    int n = arr.length;
    // Build max-heap
    for (int i = n / 2 - 1; i >= 0; i--) heapify(arr, n, i);
    // Extract one by one
    for (int i = n - 1; i > 0; i--) {
        swap(arr, 0, i);
        heapify(arr, i, 0);
    }
}
```

---

## Sources

- CLRS — Chapter 6
- LeetCode — Heap / Priority Queue problem sets


---

## Hands-On Exercises

### Exercise 1: Min-Heap Basics — Kth Smallest Element
Given an unsorted array, find the kth smallest element using a max-heap of size k.

```java
int kthSmallest(int[] nums, int k) {
    // TODO: Use a max-heap (reverseOrder)
    // Add elements, if size > k, poll (remove largest)
    // The root of the max-heap is the kth smallest
}
```

**Hint:** Why max-heap and not min-heap? Because we want to keep the k smallest elements, and the largest of those k is at the top.

---

### Exercise 2: Top K Elements — Implement from Note
Implement the `topK` method from the note. Test with `nums = [3,1,5,12,2,11]`, `k = 3` → expect `[5,11,12]` (order may vary).

```java
int[] topK(int[] nums, int k) {
    PriorityQueue<Integer> heap = new PriorityQueue<>(); // min-heap
    // TODO: For each element, add to heap
    // If heap size > k, remove the smallest (poll)
    // Remaining k elements are the k largest
}
```

---

### Exercise 3: Custom Heap — Build a Max-Heap from Scratch
Implement `insert` and `poll` for a max-heap (modify the MinHeap from the note).

```java
class MaxHeap {
    private List<Integer> heap = new ArrayList<>();

    void insert(int val) {
        // TODO: Add to end, bubble UP — swap with parent if val > parent
    }

    int poll() {
        // TODO: Save root, move last to root, bubble DOWN
        // Swap with larger child
    }
}
```

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/) (LC 703) | 🟢 Easy | Min-Heap of size K |
| 2 | [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/) (LC 1046) | 🟢 Easy | Max-Heap |
| 3 | [Kth Largest Element in Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) (LC 215) | 🟡 Medium | Min-Heap / Quickselect |
| 4 | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) (LC 347) | 🟡 Medium | HashMap + Heap |
| 5 | [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) (LC 973) | 🟡 Medium | Max-Heap of size K |
| 6 | [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) (LC 295) | 🔴 Hard | Two Heaps |
| 7 | [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) (LC 23) | 🔴 Hard | Min-Heap of list heads |
| 8 | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) (LC 621) | 🟡 Medium | Max-Heap + Cooldown |

### Assignment Guidelines
- **Start** with 1–2 (Easy) — basic heap operations.
- **Then** 3–5 (Medium) — the "Top K" pattern.
- **Problems 6–7** (Hard) are classic heap interview questions. Study the two-heap median pattern carefully.
- **Target time:** 10 min per Easy, 25 min per Medium, 35 min per Hard.
