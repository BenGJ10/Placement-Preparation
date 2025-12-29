# Insertion Sort

Insertion Sort is a comparison-based sorting algorithm that **builds the final sorted array one element at a time**.

At each step:

* the left part of the array is maintained as **sorted**
* the current element is **inserted** into its correct position in that sorted part

It works very similarly to how people sort **playing cards in hand**.

---

## Intuition

Imagine you have cards:

```
7 3 5 2
```

You:

1. take 7 → already sorted part = `7`
2. take 3 → insert before 7 → `3 7`
3. take 5 → insert between 3 and 7 → `3 5 7`
4. take 2 → insert at beginning → `2 3 5 7`

You don’t swap randomly;
you **shift elements** to make space for the new element.

This is exactly Insertion Sort.

---

## Algorithm Steps

For each index `i` from `1 to n−1`:

1. store the current value `key = a[i]`
2. compare it with elements on the left
3. shift elements greater than `key` to the right
4. insert `key` at the correct position

---

## C++ Implementation (Standard Insertion Sort)

```cpp
void insertionSort(vector<int>& a) {
    int n = a.size();

    for (int i = 1; i < n; i++) {
        int key = a[i];
        int j = i - 1;

        while (j >= 0 && a[j] > key) {
            a[j + 1] = a[j];   // shift right
            j--;
        }

        a[j + 1] = key;         // insert key
    }
}
```

---

## Dry Run Example

Array:

```
5 2 4 6 1
```

Iteration details:

* `i = 1`, key = 2 → insert before 5 → `2 5 4 6 1`
* `i = 2`, key = 4 → insert between 2 and 5 → `2 4 5 6 1`
* `i = 3`, key = 6 → already larger → `2 4 5 6 1`
* `i = 4`, key = 1 → shift all right → `1 2 4 5 6`

Result: `1 2 4 5 6`

---

## Time and Space Complexity

| Case                   | Time  |
| ---------------------- | ----- |
| Best (already sorted)  | O(n)  |
| Average                | O(n²) |
| Worst (reverse sorted) | O(n²) |

Space Complexity: **O(1)**
Insertion Sort is **in-place**.

---

## Stability

Insertion Sort is **stable**.

Equal elements **do not change relative order**, because we only shift elements strictly greater than `key`.

---

## Adaptiveness

Insertion Sort is **adaptive**:

* if the array is nearly sorted
* it runs very fast
* few shifts and comparisons

This is why many libraries use it for **small subarrays** inside hybrid algorithms.

---

## When Insertion Sort Is Useful

Insertion Sort is a very good choice when:

* data is **small**
* data is **nearly sorted**
* incoming data arrives **online** (streaming)
* stability is required
* memory is limited

Real systems use insertion sort as a **base case** for merge sort / introsort when input sizes get tiny.

---

## Characteristics at a Glance

* in-place
* stable
* adaptive
* simple and intuitive
* quadratic worst-case
* fast on nearly sorted inputs

---

## Comparison with Bubble & Selection Sort

| Feature        | Bubble          | Selection | Insertion |
| -------------- | --------------- | --------- | --------- |
| Adaptive       | Yes (optimized) | No        | Yes       |
| Stable         | Yes             | No        | Yes       |
| Swaps          | Many            | Few       | Few       |
| Shifts         | Medium          | Few       | Many      |
| Best Case Time | O(n)            | O(n²)     | O(n)      |

Insertion sort is generally **better than both** in practice.

---

## Binary Insertion Sort (Optional Optimization)

Instead of scanning linearly to find insert position, use **binary search**.

Shifts remain O(n), comparisons become O(log n), so total complexity is still O(n²), but constant factors reduce.

---

## Interview Questions

### Q1. Why is insertion sort good for nearly sorted data?

Because very few shifts are required, giving **almost O(n)** behavior.

---

### Q2. Why is insertion sort stable?

Because equal elements are **not moved past each other** while inserting.

---

### Q3. How many passes does insertion sort take?

Exactly `n−1` passes.

---

### Q4. Where is insertion sort used in practice?

As the base case of:

* Merge Sort
* TimSort
* Introsort (`std::sort` uses it for small segments)

---

### Q5. What is the worst-case input for insertion sort?

Reverse sorted array.

---

## Summary

* insertion sort inserts elements into already sorted prefix
* stable, adaptive, in-place
* O(n²) worst case but O(n) on nearly sorted arrays
* excellent for:

  * small inputs
  * online input streams
  * almost-sorted arrays

---