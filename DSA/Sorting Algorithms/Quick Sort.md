# Quick Sort

**Quick Sort** is a comparison-based, divide-and-conquer sorting algorithm.

Core idea:

1. Pick a **pivot**
2. Rearrange array so:

   * elements **less than pivot** are on its left
   * elements **greater than pivot** are on its right
3. Recursively apply the same logic on left and right parts

Unlike Merge Sort, Quick Sort does **in-place partitioning** and is usually faster in practice.

---

## Why Quick Sort is Fast in Practice

Quick sort:

* uses **in-place partitioning** (low memory)
* has **excellent cache locality**
* very small constant factors
* is the basis of many library sorts (after introspection optimizations)

Even though worst case is O(n²), good pivot strategies avoid it.

---

## Algorithm Intuition

For array:

```
8 3 4 9 2
```

Choose pivot = 8

Rearrange so smaller elements go left:

```
3 4 2 | 8 | 9
```

8 is now in correct sorted position.
Recursively sort left and right parts.

---

## Partitioning — The Heart of Quick Sort

Partition places pivot at correct position and returns its index.

Two classical methods:

1. **Lomuto partition scheme** — simpler, uses last element as pivot
2. **Hoare partition scheme** — fewer swaps, often faster

---

## Lomuto Partition (simple, interview-friendly)

Steps:

* choose **last element** as pivot
* maintain index `i` for “≤ pivot region”
* scan array with `j`
* swap when element ≤ pivot
* finally place pivot in correct position

### Code: Lomuto Partition

```cpp
int lomutoPartition(vector<int>& a, int l, int r) {
    int pivot = a[r];
    int i = l;

    for (int j = l; j < r; j++) {
        if (a[j] < pivot) {
            swap(a[i], a[j]);
            i++;
        }
    }
    swap(a[i], a[r]);
    return i;
}
```

---

## Quick Sort using Lomuto Partition

```cpp
void quickSort(vector<int>& a, int l, int r) {
    if (l >= r) return;

    int p = lomutoPartition(a, l, r);

    quickSort(a, l, p - 1);
    quickSort(a, p + 1, r);
}
```

---

## Hoare Partition (fewer swaps)

Hoare partition picks **first element as pivot** and uses two pointers.

```cpp
int hoarePartition(vector<int>& a, int l, int r) {
    int pivot = a[l];
    int i = l - 1, j = r + 1;

    while (true) {
        do { i++; } while (a[i] < pivot);
        do { j--; } while (a[j] > pivot);

        if (i >= j) return j;

        swap(a[i], a[j]);
    }
}
```

Quick sort call changes slightly:

```cpp
void quickSort(vector<int>& a, int l, int r) {
    if (l >= r) return;

    int p = hoarePartition(a, l, r);

    quickSort(a, l, p);
    quickSort(a, p + 1, r);
}
```

---

## Dry Run Example (Lomuto)

Array:

```
4 2 7 1 3
```

Pivot = 3

Steps:

```
i = 0
j scans

4 2 7 1 3
↑       ↑
i       pivot

Compare 4 > 3 → no swap

2 < 3 → swap 4 & 2
2 4 7 1 3

1 < 3 → swap 4 & 1
2 1 7 4 3
```

Final swap pivot with `a[i]`:

```
2 1 3 4 7
      ^
   pivot placed correctly
```

---

## Time and Space Complexity

| Case                               | Time       |
| ---------------------------------- | ---------- |
| Best                               | O(n log n) |
| Average                            | O(n log n) |
| Worst (already sorted + bad pivot) | O(n²)      |

Space Complexity: **O(log n)** (due to recursion stack)

---

## Why Worst Case Happens?

Worst case when:

* pivot always chosen as min or max
* array already sorted or reverse sorted
* no randomization

Example:

```
1 2 3 4 5
```

Pivot chosen = 5 always → skewed recursion → O(n²)

---

## How to Avoid Worst Case

* random pivot selection
* median of three pivot
* introsort (switch to heap sort later)

Randomized pivot:

```cpp
int randomizedPartition(vector<int>& a, int l, int r) {
    int randomIndex = l + rand() % (r - l + 1);
    swap(a[randomIndex], a[r]);
    return lomutoPartition(a, l, r);
}
```

---

## Stability

Quick sort is normally **not stable**

Example:

```
(3a, 3b, 1)
```

Partitioning may reorder 3a and 3b.

Stable quicksort exists but is complex and rarely used.

---

## Comparison With Merge Sort

| Feature         | Quick Sort | Merge Sort |
| --------------- | ---------- | ---------- |
| Time (avg)      | O(n log n) | O(n log n) |
| Worst time      | O(n²)      | O(n log n) |
| Space           | O(log n)   | O(n)       |
| Stable          | No         | Yes        |
| In-place        | Yes        | No         |
| Practical speed | Faster     | Slower     |
| Linked lists    | Bad        | Excellent  |

---

## Advantages

* fast in practice
* in-place
* average O(n log n)
* great cache performance

---

## Disadvantages

* worst-case O(n²)
* not stable
* recursion overhead

---

## Tail Recursion Optimization Note

Quick sort usually recurses on:

* smaller partition first
* converts larger partition to loop

This prevents stack overflow in large input cases.

---

## Interview Questions

### Q1. Why is quick sort faster than merge sort in practice?

Because:

* in-place
* better cache locality
* fewer memory allocations

---

### Q2. Why does quick sort fail on sorted arrays (naive version)?

Because pivot becomes smallest or largest every time → recursion degenerates to O(n²).

---

### Q3. Is quick sort stable?

No, not by default.

---

### Q4. Why is quick sort preferred in libraries?

Because average-case is extremely fast with optimizations.

---

### Q5. When should you not use quick sort?

* when stability is required
* when O(n²) risk is unacceptable
* when sorting linked lists

---

## Summary

* Quick sort = divide → partition → recurse
* average O(n log n), worst O(n²)
* in-place, not stable
* pivot selection crucial for performance
* widely used in practice

---
