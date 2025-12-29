# Merge Sort

**Merge Sort** is a comparison-based, **divide-and-conquer** sorting algorithm.

The core idea:

1. **Divide** the array into two halves
2. **Recursively sort** both halves
3. **Merge** the two sorted halves into one sorted array

Unlike Bubble/Selection/Insertion sorts, Merge Sort guarantees **O(n log n)** time in all cases.

---

## Why Merge Sort?

Merge Sort is used because it:

* guarantees **O(n log n)** time
* is **stable**
* works very well for **linked lists**
* supports **external sorting** (sorting huge files on disk)

It is one of the most important interview algorithms.

---

## Algorithm Intuition (Divide and Conquer)

Given array:

```
[7, 2, 5, 3]
```

1. Split → `[7,2]` and `[5,3]`
2. Recursively sort both parts
3. Merge sorted halves

```
[7,2] → [2,7]
[5,3] → [3,5]
Merge → [2,3,5,7]
```

Merging is the key operation:
we take **two sorted arrays** and merge them into a single sorted one.

---

## Algorithm Steps

1. Base Case
   If size ≤ 1 → already sorted

2. Recursive Case

   * divide array into two halves
   * recursively sort left half
   * recursively sort right half
   * merge two sorted halves

---

## Merge Sort Implementation in C++

```cpp
void merge(vector<int>& a, int l, int m, int r) {
    vector<int> temp;
    int i = l, j = m + 1;

    while (i <= m && j <= r) {
        if (a[i] <= a[j]) temp.push_back(a[i++]);
        else temp.push_back(a[j++]);
    }

    while (i <= m) temp.push_back(a[i++]);
    while (j <= r) temp.push_back(a[j++]);

    for (int k = 0; k < temp.size(); k++) {
        a[l + k] = temp[k];
    }
}

void mergeSort(vector<int>& a, int l, int r) {
    if (l >= r) return;

    int m = (l + r) / 2;

    mergeSort(a, l, m);
    mergeSort(a, m + 1, r);

    merge(a, l, m, r);
}
```

---

## Dry Run Example

Array:

```
[4, 1, 3, 2]
```

Step-by-step:

* split → `[4,1]` & `[3,2]`
* split → `[4] [1]` & `[3] [2]`
* merge `[4]` and `[1]` → `[1,4]`
* merge `[3]` and `[2]` → `[2,3]`
* final merge `[1,4]` & `[2,3]` → `[1,2,3,4]`

---

## Time Complexity (Intuition)

Merge Sort does:

* **log n** splits (keep dividing in half)
* **n** work at each level while merging

So:

```
T(n) = 2T(n/2) + O(n)
```

By Master Theorem:

```
T(n) = O(n log n)
```

### Summary Table

| Case    | Time Complexity |
| ------- | --------------- |
| Best    | O(n log n)      |
| Average | O(n log n)      |
| Worst   | O(n log n)      |

---

## Space Complexity

Merge sort needs **extra temporary array** during merging.

* Space complexity: **O(n)**
* Not in-place (in typical implementation)

---

## Stability

Merge Sort is **stable** because when merging:

```
if (a[i] <= a[j])
```

we ensure equal elements retain relative order.

This is extremely important for:

* sorting records by multiple keys
* applications like databases and compilers

---

## Merge Sort on Linked List (Important Trick)

Merge Sort is preferred for linked lists because:

* splitting is O(n)
* merging is easy by relinking pointers
* no random access required

Quick Sort is bad on linked lists due to random indexing.

---

## Variants of Merge Sort

* Top-Down Merge Sort (recursive) ← most common
* Bottom-Up Merge Sort (iterative)
* External Merge Sort (used for huge files)
* Parallel Merge Sort

---

## Advantages

* predictable O(n log n)
* stable
* works well on large data
* suitable for linked lists
* great for external sorting

---

## Disadvantages

* extra O(n) space
* slower than Quick Sort in practice due to memory overhead
* recursive overhead

---

## Comparison with Other Sorting Algorithms

| Algorithm      | Time                        | Space    | Stable |
| -------------- | --------------------------- | -------- | ------ |
| Merge Sort     | O(n log n)                  | O(n)     | Yes    |
| Quick Sort     | O(n log n) avg, O(n²) worst | O(log n) | No     |
| Heap Sort      | O(n log n)                  | O(1)     | No     |
| Insertion Sort | O(n²)                       | O(1)     | Yes    |

---

## Interview Questions

### Q1. Why merge sort is preferred over quicksort for linked lists?

Because:

* quicksort requires random index access
* merge sort only needs sequential traversal
* merging operation is pointer-based and efficient

---

### Q2. Why isn’t merge sort used in in-memory library sorts?

Because:

* extra O(n) memory
* quicksort/introsort usually faster in practice
* cache behavior favors quicksort

---

### Q3. Is merge sort in-place?

No, not in the classical implementation.
It requires O(n) extra memory.

---

### Q4. Why is merge sort always O(n log n)?

Because:

* array is always split in half
* merging always takes linear time
* recursion depth is always log n

---

### Q5. Can merge sort be made in-place?

Yes, but algorithms become complex and inefficient in practice; rarely expected in interviews.

---

## Summary

* Merge Sort uses **divide and conquer**
* Splits → sorts → merges
* Guarantees O(n log n)
* Requires extra space O(n)
* Stable and well suited for linked lists and external sorting

---
