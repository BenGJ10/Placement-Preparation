# Selection Sort

Selection Sort is a simple comparison-based sorting algorithm that repeatedly **selects the minimum element** from the unsorted part of the array and places it at its correct sorted position.

At each iteration:

* the left side of the array becomes sorted
* the right side remains unsorted

It emphasizes **selecting elements, not swapping adjacent ones** (unlike Bubble Sort).

---

## Intuition

Think of how you sort cards manually:

1. Find the **smallest card**
2. Place it in the first position
3. From the remaining cards, find the next smallest
4. Place it in the second position
5. Continue until sorted

This is exactly Selection Sort.

---

## Algorithm Steps

For an array of size `n`:

1. Loop from `i = 0` to `n−2`
2. Assume index `i` holds minimum value
3. Search for actual minimum in remaining subarray `[i+1 … n-1]`
4. Swap found minimum with element at `i`
5. After i-th pass, first `i+1` elements are sorted

---

## Selection Sort C++ Implementation

```cpp
void selectionSort(vector<int>& a) {
    int n = a.size();

    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;

        for (int j = i + 1; j < n; j++) {
            if (a[j] < a[minIndex]) {
                minIndex = j;
            }
        }

        swap(a[i], a[minIndex]);
    }
}
```

This performs exactly **n − 1 selections and up to n − 1 swaps**.

---

## Dry Run Example

Array:

```
7 4 5 2
```

Pass 1: find minimum

* smallest = 2 at index 3
* swap with index 0

Result:

```
2 4 5 7
```

Pass 2:

* smallest from remaining = 4
* already in place

Pass 3:

* smallest from remaining = 5
* already in place

Final sorted array:

```
2 4 5 7
```

---

## Time and Space Complexity

| Case    | Time Complexity |
| ------- | --------------- |
| Best    | O(n²)           |
| Average | O(n²)           |
| Worst   | O(n²)           |

Space Complexity: **O(1)**
Selection Sort is **in-place**.

Number of comparisons is fixed:

```
n(n−1)/2
```

---

## Stability of Selection Sort

Selection Sort is **not stable** by default because swapping may change the relative order of equal elements.

Example:

```
(4a, 4b, 2)
```

After sorting:

```
(2, 4b, 4a)  ← order changed
```

However, it can be made stable using shifting instead of swapping (rarely required in interviews).

---

## Characteristics at a Glance

* not adaptive (does not detect sorted array)
* performs minimum number of swaps (important point)
* predictable comparison count
* good when memory writes are expensive
* bad for large inputs due to O(n²)

---

## Comparison with Bubble Sort

| Bubble Sort                | Selection Sort        |
| -------------------------- | --------------------- |
| many swaps                 | minimal swaps         |
| adaptive with optimization | never adaptive        |
| stable                     | not stable            |
| adjacent comparisons       | global minimum search |

---

## When Selection Sort Is Used

* when the cost of swapping is high
* when memory writes must be minimized (e.g., EEPROM/Flash)
* teaching algorithm design fundamentals
* small arrays with expensive swap operations

Not used in modern large-scale systems due to inefficiency.

---

## Advantages

* simple to implement
* minimal number of swaps
* works well when memory writes are costly
* in-place algorithm

---

## Disadvantages

* O(n²) always
* not stable (normally)
* not adaptive
* performs poorly on large datasets

---

## Interview Questions

### Q1. Why is selection sort not adaptive?

Because it always performs full passes even if the array is already sorted.

---

### Q2. Why does selection sort perform minimal swaps?

It swaps at most once per outer loop iteration, unlike Bubble Sort which swaps repeatedly.

---

### Q3. Is selection sort stable?

Not in its usual implementation, because swapping can break order of equal elements.

---

### Q4. Why is selection sort better than bubble sort sometimes?

When swaps are expensive and comparisons are cheap.

---

### Q5. How many swaps does selection sort do?

At most `n − 1`.

---

## Summary

* repeatedly selects **minimum element**
* places it at the correct position
* time complexity O(n²)
* space complexity O(1)
* not adaptive and not stable
* performs minimal swaps
* good for concept-building, poor for large inputs

--- 