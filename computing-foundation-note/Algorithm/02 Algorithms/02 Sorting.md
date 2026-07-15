---
tags:
- algorithms
- data-structures
- programming
---

# 02 Sorting

Sorting is the most studied algorithm family. Know quick sort for interviews, merge sort for stable external sort, and when to use counting/radix for integers.

---

## Comparison Table

| Algorithm | Time (Avg) | Time (Worst) | Space | Stable? | In-Place? |
|-----------|:--------:|:----------:|:-----:|:------:|:---------:|
| **Quick** | O(n log n) | O(n²) | O(log n) | ❌ | ✅ |
| **Merge** | O(n log n) | O(n log n) | O(n) | ✅ | ❌ |
| **Heap** | O(n log n) | O(n log n) | O(1) | ❌ | ✅ |
| **Insertion** | O(n²) | O(n²) | O(1) | ✅ | ✅ |
| **Counting** | O(n + k) | O(n + k) | O(k) | ✅ | ❌ |
| **Radix** | O(d(n + k)) | O(d(n + k)) | O(n + k) | ✅ | ❌ |

---

## Quick Sort

Pivot-based divide and conquer. Fast in practice. Not stable.

```java
void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pivot = partition(arr, low, high);
        quickSort(arr, low, pivot - 1);
        quickSort(arr, pivot + 1, high);
    }
}

int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr, i, j);
        }
    }
    swap(arr, i + 1, high);
    return i + 1;
}
```

---

## Merge Sort

Divide, sort halves, merge. Stable. O(n) extra space.

```java
void mergeSort(int[] arr, int left, int right) {
    if (left >= right) return;
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}

void merge(int[] arr, int left, int mid, int right) {
    int[] temp = new int[right - left + 1];
    int i = left, j = mid + 1, k = 0;
    while (i <= mid && j <= right) {
        temp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
    }
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];
    System.arraycopy(temp, 0, arr, left, temp.length);
}
```

---

## Non-Comparison Sorts (Integer Only)

| Algorithm | How | When |
|-----------|-----|------|
| **Counting** | Count frequency, rebuild | Small range of integers |
| **Radix** | Sort by each digit (LSD/MSD) | Large range, fixed-length keys |
| **Bucket** | Distribute into buckets, sort each | Uniformly distributed floats |

```java
// Counting Sort — O(n + k), stable
void countingSort(int[] arr) {
    int max = Arrays.stream(arr).max().orElse(0);
    int[] count = new int[max + 1];
    int[] output = new int[arr.length];
    
    for (int n : arr) count[n]++;
    for (int i = 1; i <= max; i++) count[i] += count[i - 1];
    for (int i = arr.length - 1; i >= 0; i--) {
        output[count[arr[i]] - 1] = arr[i];
        count[arr[i]]--;
    }
    System.arraycopy(output, 0, arr, 0, arr.length);
}
```

---

## Stability

A **stable** sort preserves the relative order of equal elements.

```
Input:  [(A,2), (B,1), (C,2)]   ← sort by number
Stable: [(B,1), (A,2), (C,2)]   ← A before C preserved
Unstable:[(B,1), (C,2), (A,2)]   ← A and C swapped
```

| Stable | Not Stable |
|--------|-----------|
| Merge, Insertion, Counting, Radix | Quick, Heap, Selection |

---

## When to Use

| Scenario | Best Algorithm |
|----------|:-------------:|
| General purpose, in-memory | Quick (or Timsort — Java's default) |
| Need stability | Merge sort |
| Mostly sorted data | Insertion sort |
| Integer keys, limited range | Counting / Radix |
| External (disk) sorting | Merge sort |

---

## Sources

- CLRS — Chapters 6–8
- Sedgewick — Chapter 2 (Sorting)


---

## Hands-On Exercises

### Exercise 1: Quick Sort — Implement Partition
Implement the `partition` method from the note. Test with `[3,6,8,10,1,2,1]`, pivot = last element.

```java
int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    // TODO: Move all elements ≤ pivot to the left
    // Return final pivot position
}
```

---

### Exercise 2: Merge Sort — Implement Merge
Implement the `merge` method from the note. Given two sorted halves of an array, merge them in-place.

```java
void merge(int[] arr, int left, int mid, int right) {
    int[] temp = new int[right - left + 1];
    // TODO: Two-pointer merge from both halves into temp
    // Copy temp back to arr
}
```

---

### Exercise 3: Counting Sort — Implement from Note
Implement counting sort for an array of non-negative integers.

```java
void countingSort(int[] arr) {
    // TODO: 1. Find max
    // 2. Count frequencies
    // 3. Compute prefix sums (cumulative count)
    // 4. Build output array (traverse backwards for stability)
}
```

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Sort an Array](https://leetcode.com/problems/sort-an-array/) (LC 912) | 🟡 Medium | Merge / Quick Sort |
| 2 | [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/) (LC 88) | 🟢 Easy | Merge (Two Pointers) |
| 3 | [Sort Colors](https://leetcode.com/problems/sort-colors/) (LC 75) | 🟡 Medium | Dutch National Flag |
| 4 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) (LC 56) | 🟡 Medium | Sort + Merge |
| 5 | [Kth Largest Element](https://leetcode.com/problems/kth-largest-element-in-an-array/) (LC 215) | 🟡 Medium | Quickselect |
| 6 | [Wiggle Sort II](https://leetcode.com/problems/wiggle-sort-ii/) (LC 324) | 🟡 Medium | Sort + Interleave |
| 7 | [Maximum Gap](https://leetcode.com/problems/maximum-gap/) (LC 164) | 🔴 Hard | Radix Sort / Bucket |
| 8 | [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) (LC 315) | 🔴 Hard | Merge Sort + Count |

### Assignment Guidelines
- **Start** with problem 2 (Easy) — merging sorted arrays.
- **Then** 1, 3–6 (Medium) — sorting applications and quickselect.
- **Problems 7–8** (Hard) require advanced sorting techniques.
- **Target time:** 10 min per Easy, 20 min per Medium, 35 min per Hard.
