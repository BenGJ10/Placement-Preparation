# Heap Sort

**Heap Sort** is a comparison-based sorting algorithm that uses a **binary heap data structure**.

Core idea:

1. Build a **max-heap**
2. The **largest element** moves to the root
3. Swap root with last element
4. Reduce heap size and **heapify**
5. Repeat until array becomes sorted

Heap sort always guarantees **O(n log n)** time and sorts **in-place**.

---

## What is a Binary Heap?

A **binary heap** is:

* a **complete binary tree**
* stored as an array
* satisfies **heap property**

### Max-Heap Property

For every node `i`:

```
a[parent(i)] >= a[i]
```

Meaning parent is always greater than its children.

Heap sort uses **max-heap** for ascending sort.

---

## Why Heap Sort Works

Once array is converted to a max-heap:

* the **maximum element** is at index `0`
* swap it with last element
* reduce heap size (ignore sorted part)
* restore heap property by **heapify**

Repeat until heap size becomes 1.

---

## Array Representation of Heap

For index `i`:

* left child → `2*i + 1`
* right child → `2*i + 2`
* parent → `(i-1)/2`

No explicit tree or pointers needed.

---

## Heapify (Key Operation)

`heapify(i)` assumes subtrees are heap, fixes violation at node `i`.

```cpp
void heapify(vector<int>& a, int n, int i) {
    int largest = i;
    int l = 2*i + 1;
    int r = 2*i + 2;

    if (l < n && a[l] > a[largest])
        largest = l;

    if (r < n && a[r] > a[largest])
        largest = r;

    if (largest != i) {
        swap(a[i], a[largest]);
        heapify(a, n, largest);
    }
}
```

---

## Heap Sort Algorithm (C++)

```cpp
void heapSort(vector<int>& a) {
    int n = a.size();

    // Step 1: Build max heap
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(a, n, i);

    // Step 2: Extract elements from heap
    for (int i = n - 1; i > 0; i--) {
        swap(a[0], a[i]);        // move max to end
        heapify(a, i, 0);        // restore heap on reduced heap
    }
}
```

---

## Dry Run Example

Array:

```
4 10 3 5 1
```

1. Build max heap:

```
10 5 3 4 1
```

2. Swap root with last:

```
1 5 3 4 10
```

3. Heapify remaining:

```
5 4 3 1 10
```

4. Repeat until sorted:

```
1 3 4 5 10
```

---

## Time and Space Complexity

### Time Complexity

| Phase                  | Time           |
| ---------------------- | -------------- |
| Build heap             | O(n)           |
| Heapify for n elements | O(n log n)     |
| Total                  | **O(n log n)** |

### Space Complexity

* **O(1)** extra space (in-place)

---

## Stability of Heap Sort

Heap sort is **not stable**.

Reason:

* elements may jump across large distances during heapify and swaps
* equal elements can change relative order

---

## Advantages of Heap Sort

* predictable **O(n log n)** time
* in-place sorting
* not vulnerable to bad pivot cases like quick sort
* simple to implement from heap structure

---

## Disadvantages

* not stable
* slower in practice than quick sort due to:

  * poorer cache locality
  * more overhead in heap maintenance
* extra complexity compared to merge/insertion sorts

---

## Comparison with Other Sorting Algorithms

| Feature         | Heap Sort  | Quick Sort | Merge Sort |
| --------------- | ---------- | ---------- | ---------- |
| Time (avg)      | O(n log n) | O(n log n) | O(n log n) |
| Worst time      | O(n log n) | O(n²)      | O(n log n) |
| Space           | O(1)       | O(log n)   | O(n)       |
| Stable          | No         | No         | Yes        |
| In-place        | Yes        | Yes        | No         |
| Practical speed | Medium     | Fastest    | Medium     |

---

## Priority Queue Connection

Heap sort is conceptually the same as repeatedly:

* inserting into **max-heap priority queue**
* popping maximum

C++ priority queue automatically uses **max-heap** underneath.

---

## Interview Questions

### Q1. Why is heap sort not stable?

Because heapify can swap equal elements far apart, changing order.

---

### Q2. Why isn’t heap sort used in libraries despite O(n log n)?

Because:

* slower than quicksort in practice
* cache inefficient
* not stable
* harder to optimize in real systems

---

### Q3. Why is heap construction O(n) and not O(n log n)?

Because bottom-up heap building performs decreasing work per level; amortized complexity is linear.

---

### Q4. Why heap sort is preferred for memory-limited systems?

It uses **constant extra space**.

---

### Q5. What tree does heap represent?

A **complete binary tree** stored implicitly in an array.

---

## Summary

* Heap sort uses **binary heap**
* Steps:

  * build max heap
  * repeatedly extract max
  * heapify remaining array
* Time complexity always O(n log n)
* Space O(1)
* Not stable but in-place
* Useful conceptually and for priority queues

---
