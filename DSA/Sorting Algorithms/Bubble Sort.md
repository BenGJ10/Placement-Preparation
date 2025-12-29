# Bubble Sort

Bubble Sort is a simple comparison-based sorting algorithm that repeatedly **swaps adjacent elements if they are in the wrong order**.

It gets its name because **larger elements “bubble up” to the end** of the array after each pass.

Bubble sort is mainly used for:

* teaching sorting concepts
* very small input sizes
* detecting whether an array is already sorted

---

## How Bubble Sort Works (Intuition)

Consider the array:

```
5 1 4 2
```

We compare adjacent elements:

* compare 5 and 1 → swap → 1 5 4 2
* compare 5 and 4 → swap → 1 4 5 2
* compare 5 and 2 → swap → 1 4 2 5 ← largest element moved to the end

After the first pass, the largest element is in its **correct final position**.
We repeat the same process for the remaining portion.

---

## Algorithm Steps

1. Iterate multiple passes through array
2. In each pass, compare adjacent elements
3. Swap if left element > right element
4. End early if no swaps occur (array already sorted)

---

## Basic Bubble Sort Implementation (C++)

```cpp
void bubbleSort(vector<int>& a) {
    int n = a.size();

    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (a[j] > a[j + 1]) {
                swap(a[j], a[j + 1]);
            }
        }
    }
}
```

This is the **naive implementation**.
It always performs n² comparisons even if the array is already sorted.

---

## Optimized Bubble Sort (Early Exit)

We add a flag to check if any swap occurred.
If **no swap happens in a pass → array is already sorted**.

This improves best-case time complexity to O(n).

```cpp
void bubbleSortOptimized(vector<int>& a) {
    int n = a.size();

    for (int i = 0; i < n - 1; i++) {

        bool swapped = false;

        for (int j = 0; j < n - i - 1; j++) {
            if (a[j] > a[j + 1]) {
                swap(a[j], a[j + 1]);
                swapped = true;
            }
        }

        if (!swapped) break;
    }
}
```

---

## Time and Space Complexity

| Case                                          | Time Complexity |
| --------------------------------------------- | --------------- |
| Best Case (already sorted, optimized version) | O(n)            |
| Average Case                                  | O(n²)           |
| Worst Case (reverse sorted)                   | O(n²)           |

Space Complexity: **O(1)**
Bubble sort is **in-place**.

---

## Stability of Bubble Sort

Bubble sort is **stable** because equal elements are **never swapped across each other**.
Their relative order remains the same.

Example:

(3a, 3b, 2)

Sorted output preserves order of `3a` and `3b`.

---

## When Bubble Sort Is Used in Practice

* teaching sorting
* debugging / interviewing for basic concepts
* nearly sorted small arrays
* when stability matters and n is very small

It is not used for large datasets because of **O(n²)** complexity.

---

## When Not to Use Bubble Sort

* large datasets
* performance-critical apps
* memory is not the bottleneck but time is

Prefer Merge Sort, Quick Sort, Heap Sort in real systems.

---

## Dry Run Example

Array:

```
4 3 2 1
```

Pass 1:

```
3 4 2 1
3 2 4 1
3 2 1 4
```

Pass 2:

```
2 3 1 4
2 1 3 4
```

Pass 3:

```
1 2 3 4
```

Sorted.

---

## Advantages

* simple to understand
* easy to implement
* stable
* in-place (no extra memory needed)

---

## Disadvantages

* very slow for large inputs
* performs many unnecessary swaps
* rarely used in real-world software

---

## Interview Questions

### Q1. Why is bubble sort rarely used in practice?

Because its time complexity is **O(n²)** and faster algorithms exist.

---

### Q2. Is bubble sort stable?

Yes, because equal elements are not rearranged relative to each other.

---

### Q3. Is bubble sort adaptive?

With the optimized version (early exit), yes.
It becomes **O(n)** when array is already sorted.

---

### Q4. Difference between Bubble Sort and Selection Sort?

| Bubble Sort      | Selection Sort  |
| ---------------- | --------------- |
| swaps many times | minimizes swaps |
| stable           | not stable      |
| adaptive         | not adaptive    |

---

### Q5. How many passes are required in worst case?

`n − 1` passes.

---

## Summary

* Bubble sort repeatedly swaps adjacent elements
* Stable and in-place
* Worst and average time complexity O(n²)
* Only useful for:

  * learning
  * very small datasets
  * nearly sorted data

---