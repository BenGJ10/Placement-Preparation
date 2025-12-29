# Sorting Algorithms — Overview

Sorting is the process of arranging data **in a specific order** (usually ascending or descending).
Sorting acts as a foundation for:

* searching algorithms
* greedy and DP problems
* optimization in real systems
* database indexing
* ordering and ranking tasks

Good knowledge of sorting includes understanding:

* how algorithms work
* time and space complexity
* stability
* practical use-cases

---

## Key Terms

### Stable Sorting

A sorting algorithm is **stable** if it preserves the **relative order of equal elements**.

Example:
Input → (5a, 3, 5b)
Stable sorted output → (3, 5a, 5b)

### In-place Sorting

Uses **O(1) or O(log n)** extra memory.

### Comparison vs Non-comparison sorting

* Comparison based → use comparisons (O(n log n) lower bound)
* Non-comparison based → use counts/buckets (can be O(n))

---

## Time Complexity Landscape

| Algorithm      | Best       | Average    | Worst      | Space    | Stable | Type           |
| -------------- | ---------- | ---------- | ---------- | -------- | ------ | -------------- |
| Bubble Sort    | O(n)       | O(n²)      | O(n²)      | O(1)     | Yes    | Comparison     |
| Selection Sort | O(n²)      | O(n²)      | O(n²)      | O(1)     | No     | Comparison     |
| Insertion Sort | O(n)       | O(n²)      | O(n²)      | O(1)     | Yes    | Comparison     |
| Merge Sort     | O(n log n) | O(n log n) | O(n log n) | O(n)     | Yes    | Comparison     |
| Quick Sort     | O(n log n) | O(n log n) | O(n²)      | O(log n) | No     | Comparison     |
| Heap Sort      | O(n log n) | O(n log n) | O(n log n) | O(1)     | No     | Comparison     |
| Counting Sort  | O(n+k)     | O(n+k)     | O(n+k)     | O(n+k)   | Yes    | Non-comparison |
| Radix Sort     | O(d·(n+k)) | O(d·(n+k)) | O(d·(n+k)) | O(n+k)   | Yes    | Non-comparison |
| Bucket Sort    | O(n)       | O(n)       | O(n²)      | O(n)     | Yes    | Non-comparison |

---

## Bubble Sort

### Idea

Repeatedly swap adjacent elements if they are in the wrong order.

### C++ Code

```cpp
void bubbleSort(vector<int>& a) {
    int n = a.size();
    for (int i = 0; i < n-1; i++) {
        bool swapped = false;
        for (int j = 0; j < n-i-1; j++) {
            if (a[j] > a[j+1]) {
                swap(a[j], a[j+1]);
                swapped = true;
            }
        }
        if (!swapped) break;
    }
}
```

### Notes

* rarely used in practice
* useful for introducing sorting concepts

---

## Selection Sort

### Idea

Repeatedly select the **minimum element** and place it in correct position.

### C++ Code

```cpp
void selectionSort(vector<int>& a) {
    int n = a.size();
    for (int i = 0; i < n-1; i++) {
        int mn = i;
        for (int j = i+1; j < n; j++)
            if (a[j] < a[mn]) mn = j;
        swap(a[i], a[mn]);
    }
}
```

### Notes

* minimal swaps
* not stable
* used where memory writes are costly

---

## Insertion Sort

### Idea

Build the sorted array one element at a time by inserting elements into their correct position.

### C++ Code

```cpp
void insertionSort(vector<int>& a) {
    for (int i = 1; i < a.size(); i++) {
        int key = a[i];
        int j = i - 1;
        while (j >= 0 && a[j] > key) {
            a[j+1] = a[j];
            j--;
        }
        a[j+1] = key;
    }
}
```

### Notes

* efficient for **small / nearly sorted arrays**
* used inside hybrid sorts (like Timsort)

---

## Efficient Sorting Algorithms

## Merge Sort

### Idea

Divide array into two halves, sort each half, then merge.

### Properties

* time → O(n log n)
* stable
* not in-place (needs extra array)

### C++ Code

```cpp
void merge(vector<int>& a, int l, int m, int r) {
    vector<int> tmp;
    int i=l, j=m+1;
    while(i<=m && j<=r)
        tmp.push_back(a[i] < a[j] ? a[i++] : a[j++]);
    while(i<=m) tmp.push_back(a[i++]);
    while(j<=r) tmp.push_back(a[j++]);
    for(int k=0;k<tmp.size();k++)
        a[l+k]=tmp[k];
}

void mergeSort(vector<int>& a, int l, int r) {
    if(l>=r) return;
    int m=(l+r)/2;
    mergeSort(a,l,m);
    mergeSort(a,m+1,r);
    merge(a,l,m,r);
}
```

---

## Quick Sort

### Idea

Choose a pivot and partition array into elements:

* less than pivot
* greater than pivot

Recursively sort halves.

### C++ Code (Lomuto Partition)

```cpp
int partition(vector<int>& a, int l, int r) {
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

void quickSort(vector<int>& a, int l, int r) {
    if (l < r) {
        int p = partition(a, l, r);
        quickSort(a, l, p-1);
        quickSort(a, p+1, r);
    }
}
```

### Notes

* average O(n log n)
* worst O(n²)
* in-place
* not stable
* used in competitive programming due to low constants

---

## Heap Sort

### Idea

Use a **binary heap** to extract maximum element repeatedly.

### Outline

1. build max heap
2. repeatedly extract max
3. place it at the end

### Key Points

* always O(n log n)
* in-place
* not stable

---

## Non-Comparison Sorting Algorithms

These use properties of numbers, not comparisons.

---

## Counting Sort

### Idea

Count occurrences of each element.

### Works When

* elements are integers
* small value range

### Time

O(n + k)

---

## Radix Sort

### Idea

Sort digits from least significant to most significant using a stable sort.

Used for:

* integers
* strings of same length

---

## Bucket Sort

### Idea

Distribute into buckets, sort each bucket, combine.

Used when data is uniformly distributed.

---

## When to Use Which Sorting Algorithm

| Situation                      | Best Choice          |
| ------------------------------ | -------------------- |
| Small / almost sorted          | Insertion sort       |
| Always O(n log n) & stable     | Merge sort           |
| Best practical general-purpose | Quick sort           |
| Memory constrained             | Heap sort            |
| Small integer range            | Counting sort        |
| Big integers / strings         | Radix sort           |
| CPU caches                     | Quick sort / Timsort |
| Parallel sorting               | Merge sort           |

Also note: **C++ std::sort** uses **Introsort**
= QuickSort + HeapSort + InsertionSort hybrid.

---

## Interview Questions

### Q1. Why is merge sort stable but quick sort is not?

Merge combines elements without swapping equal values across sides, preserving order. Partitioning in quicksort may reorder equal elements.

### Q2. Why is O(n log n) the lower bound for comparison sorting?

Because any comparison sorter must distinguish n! permutations, needing log₂(n!) ≈ n log n comparisons.

### Q3. Why is quicksort used in practice despite worst case O(n²)?

Average case is O(n log n) with small constants and good cache locality.

### Q4. When does insertion sort beat quicksort?

For very small arrays or nearly sorted data because of low overhead.

### Q5. Why does counting sort beat comparison sorts?

It does not rely on comparisons, so avoids O(n log n) lower bound when constraints allow.

---

## LeetCode Problems Related to Sorting

* [https://leetcode.com/problems/sort-colors/](https://leetcode.com/problems/sort-colors/)
* [https://leetcode.com/problems/merge-intervals/](https://leetcode.com/problems/merge-intervals/)
* [https://leetcode.com/problems/largest-number/](https://leetcode.com/problems/largest-number/)
* [https://leetcode.com/problems/kth-largest-element-in-an-array/](https://leetcode.com/problems/kth-largest-element-in-an-array/)
* [https://leetcode.com/problems/relative-sort-array/](https://leetcode.com/problems/relative-sort-array/)
* [https://leetcode.com/problems/contains-duplicate/](https://leetcode.com/problems/contains-duplicate/)
* [https://leetcode.com/problems/meeting-rooms-ii/](https://leetcode.com/problems/meeting-rooms-ii/)
* [https://leetcode.com/problems/top-k-frequent-elements/](https://leetcode.com/problems/top-k-frequent-elements/)
* [https://leetcode.com/problems/sort-characters-by-frequency/](https://leetcode.com/problems/sort-characters-by-frequency/)
* [https://leetcode.com/problems/merge-k-sorted-lists/](https://leetcode.com/problems/merge-k-sorted-lists/)

---

## Summary

* Sorting is a fundamental building block for algorithms

* Various algorithms exist with different trade-offs

* Understanding when to use which algorithm is key for efficiency

* Practice problems help solidify understanding and application

---