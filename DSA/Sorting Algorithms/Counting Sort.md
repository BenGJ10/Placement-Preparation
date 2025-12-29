# Counting Sort

**Counting Sort** is a non-comparison sorting algorithm that sorts numbers by **counting frequency of each distinct value** and then reconstructing the sorted array.

Unlike comparison-based sorts such as merge/quick/heap sort, Counting Sort can achieve **O(n + k)** time, where:

* `n` = number of elements
* `k` = range of input values

Counting Sort is usually used when the **range of elements is small** compared to `n`.

---

## When Counting Sort Should Be Used

Counting sort is ideal when:

* array elements are integers
* the value range is limited (e.g., 0 to 10^5 or smaller)
* you need linear-time sorting
* stability is important (for radix sort usage)

It is **not preferred** when the range is extremely large.

---

## Core Idea

Example array:

```
4 2 2 8 3 3 1
```

Steps:

1. find range → max = 8
2. make frequency array `count[]`
3. store frequency of each number
4. compute prefix sum to know positions (for stable version)
5. place elements into output array using counts
6. copy output to original array

---

## Basic Counting Sort (non-stable, simpler)

This simply counts frequency and rewrites.

```cpp
void countingSort(vector<int>& a) {
    if (a.empty()) return;

    int mx = *max_element(a.begin(), a.end());
    int mn = *min_element(a.begin(), a.end());

    int range = mx - mn + 1;

    vector<int> count(range, 0);

    // frequency count
    for (int x : a)
        count[x - mn]++;

    // write back sorted output
    int idx = 0;
    for (int i = 0; i < range; i++) {
        while (count[i]--) {
            a[idx++] = i + mn;
        }
    }
}
```

This version works with **negative numbers** as well, using offset `mn`.

---

## Stable Counting Sort (important for interviews)

Stable version preserves **relative order of equal elements**, required in **Radix Sort**.

### Steps

1. compute frequency array
2. compute prefix sum to get positions
3. iterate original array **from right to left**
4. place elements into output array

```cpp
void countingSortStable(vector<int>& a) {
    if (a.empty()) return;

    int mx = *max_element(a.begin(), a.end());
    int mn = *min_element(a.begin(), a.end());
    int range = mx - mn + 1;

    vector<int> count(range, 0);
    vector<int> output(a.size());

    // Step 1: frequency
    for (int x : a)
        count[x - mn]++;

    // Step 2: prefix sum
    for (int i = 1; i < range; i++)
        count[i] += count[i - 1];

    // Step 3: build output (iterate backwards for stability)
    for (int i = (int)a.size() - 1; i >= 0; i--) {
        int val = a[i];
        output[count[val - mn] - 1] = val;
        count[val - mn]--;
    }

    // Step 4: copy back
    a = output;
}
```

This one guarantees **stability**.

---

## Time and Space Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n + k)   |
| Space  | O(n + k)   |

Where:

* `n` = number of elements
* `k` = range = `max − min + 1`

---

## Stability

* simple frequency rewrite version → **not stable**
* prefix-sum version → **stable**

Stability matters when:

* sorting structures/records
* radix sort digit sorting

---

## Advantages

* linear time when `k` is small
* stable version available
* simple concept
* great for integer keys
* base algorithm for Radix Sort

---

## Disadvantages

* requires extra memory
* inefficient when range is large
* only works on integers or map-able discrete keys

Example of bad case:

```
n = 10^5 elements
values range = 0 to 10^12
```

Memory becomes impractical.

---

## Handling Negative Numbers

Negative numbers are handled by **shifting**:

```
index = value − min_value
```

So smallest value maps to index 0.

Already implemented in code above.

---

## Counting Sort vs Comparison Sorts

| Feature    | Counting Sort        | Merge/Quick/Heap    |
| ---------- | -------------------- | ------------------- |
| Type       | Non-comparison       | Comparison          |
| Best Time  | O(n + k)             | O(n log n)          |
| Worst Time | O(n + k)             | O(n log n) or worse |
| Space      | O(n + k)             | O(1) to O(n)        |
| Stability  | Yes (stable version) | Varies              |

---

## Relation to Radix Sort

Counting sort is often used as **subroutine** inside **Radix Sort** because:

* it is stable
* it runs in linear time for limited base

Understanding counting sort is essential for understanding radix sort.

---

## Practical Use Cases

* sorting exam marks
* frequency tables
* character sorting
* digit sorting in radix sort
* when input domain is small (e.g., 0–255 RGB values)

---

## Interview Questions

### Q1. Why is counting sort not comparison-based?

Because it does not compare elements.
It counts occurrences.

---

### Q2. Why counting sort is O(n + k)?

Because it:

* scans array once → O(n)
* scans count array → O(k)

---

### Q3. Why is counting sort used inside radix sort?

Because radix sort needs a **stable** sorting algorithm on digits.

---

### Q4. Does counting sort work on floating numbers?

Not directly.
We need discrete integer keys or mapping to integers.

---

### Q5. How do we handle negative numbers?

By offset shift: `value − min_value`.

---

## Summary

* Counting Sort is a **non-comparison** sorting algorithm
* Time = O(n + k)
* Works best when value range is small
* Stable version exists and is widely used
* Not memory efficient for large ranges
* Fundamental building block of Radix Sort

---