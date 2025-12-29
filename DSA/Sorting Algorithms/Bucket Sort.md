# Bucket Sort

**Bucket Sort** is a non-comparison sorting algorithm that distributes elements into multiple **buckets**, sorts each bucket individually, and then concatenates the results.

It is especially efficient when:

* input is **uniformly distributed**
* elements are real numbers or floats in a range (commonly `[0, 1)`)

Bucket sort can achieve **average O(n)** time under reasonable assumptions.

---

## Core Intuition

Instead of comparing elements:

1. **divide range into equal intervals** (buckets)
2. **place each element in its corresponding bucket**
3. **sort each bucket** (often with insertion sort)
4. **concatenate all buckets in order**

If numbers are uniformly distributed, each bucket has few elements → sorting is cheap.

---

## When Bucket Sort Works Best

Bucket sort is ideal when:

* values lie in a **known, limited range**
* values are **uniformly or near-uniformly distributed**
* input includes **fractions / decimals / floating values**
* approximate linear time performance is desired

Examples:

* sorting marks in range [0, 100]
* sorting probabilities in [0,1)
* hash-based grouping tasks
* graphics and numeric simulation

---

## Difference from Similar Algorithms

| Algorithm     | Key Idea                                     |
| ------------- | -------------------------------------------- |
| Counting Sort | counts occurrences                           |
| Radix Sort    | sorts digit by digit                         |
| Bucket Sort   | groups values into buckets and sorts buckets |

Bucket sort **can use counting sort or insertion sort internally**.

---

## Bucket Sort Algorithm Steps

1. Create `k` empty buckets
2. Insert each element into appropriate bucket
3. Sort each bucket individually
4. Concatenate all buckets in order

---

## Example (numbers in range [0,1))

Array:

```
0.12 0.42 0.33 0.25 0.87 0.56
```

Buckets (size = 0.2 for demonstration)

```
[0.0–0.2): 0.12
[0.2–0.4): 0.25 0.33
[0.4–0.6): 0.42 0.56
[0.6–0.8): —
[0.8–1.0): 0.87
```

Sort each bucket and concatenate:

```
0.12 | 0.25 0.33 | 0.42 0.56 | 0.87
```

---

## Bucket Sort Implementation (C++)

### Sorting floats in range `[0,1)`

```cpp
void bucketSort(vector<double>& a) {
    int n = a.size();
    if (n <= 0) return;

    vector<vector<double>> buckets(n);

    // 1. Distribute elements into buckets
    for (double x : a) {
        int idx = n * x;                // bucket index
        buckets[idx].push_back(x);
    }

    // 2. Sort each bucket individually
    for (int i = 0; i < n; i++) {
        sort(buckets[i].begin(), buckets[i].end());
    }

    // 3. Concatenate buckets
    int k = 0;
    for (int i = 0; i < n; i++) {
        for (double x : buckets[i]) {
            a[k++] = x;
        }
    }
}
```

---

## Bucket Sort for Integers in Known Range

Generalized using mapping formula:

```
bucket_index = (value − min) / bucket_size
```

### Example Implementation

```cpp
void bucketSortIntegers(vector<int>& a, int bucketSize = 5) {
    if (a.empty()) return;

    int mn = *min_element(a.begin(), a.end());
    int mx = *max_element(a.begin(), a.end());

    int bucketCount = (mx - mn) / bucketSize + 1;

    vector<vector<int>> buckets(bucketCount);

    // distribute
    for (int x : a) {
        int idx = (x - mn) / bucketSize;
        buckets[idx].push_back(x);
    }

    // sort buckets (insertion sort or std::sort)
    for (auto& b : buckets)
        sort(b.begin(), b.end());

    // concatenate
    int k = 0;
    for (auto& b : buckets)
        for (int x : b)
            a[k++] = x;
}
```

---

## Complexity Analysis

### Time Complexity

| Case           | Complexity |
| -------------- | ---------- |
| Best / Average | O(n)       |
| Worst          | O(n²)      |

Worst case happens when:

* all elements fall into **one bucket**
* bucket sorting degenerates to quadratic sorting

### Space Complexity

* O(n + k)
  where k = number of buckets

---

## Stability

Bucket sort is **stable if bucket sorting is stable**.

For example:

* using insertion sort → stable
* using std::sort (not stable in C++) → not stable

---

## Advantages

* linear time on average when distribution is good
* good for floating-point values
* highly parallelizable (each bucket can be sorted independently)
* conceptually simple

---

## Disadvantages

* needs extra memory for buckets
* relies on good distribution
* performance degrades if data is skewed
* requires knowledge of input range

---

## Practical Applications

* distribution sort
* hashing-based partitioning
* external sorting design inspiration
* uniform random data sorting
* load-balancing problems

---

## Common Interview Pitfalls

* assuming O(n) always (it is data dependent)
* forgetting range assumption
* buckets too large or too small
* ignoring stability requirement
* not handling skewed distribution

---

## Interview Questions

### Q1. When is bucket sort better than counting sort?

When input contains **fractional/real numbers** or large range with uniform distribution.

---

### Q2. Why is bucket sort not always O(n)?

Because worst case degenerates when elements land in the **same bucket**.

---

### Q3. Is bucket sort comparison-based?

Internal bucket sorting may be, but overall strategy is **not purely comparison-based**.

---

### Q4. Why does bucket sort often use insertion sort per bucket?

Because:

* buckets are small
* insertion sort works very well for small, nearly sorted data

---

### Q5. Can bucket sort handle negative numbers?

Yes, by shifting with minimum value, similar to counting sort.

---

## Summary

* Bucket sort distributes elements into buckets and sorts buckets
* Average time complexity ~ O(n), worst O(n²)
* Best for **uniformly distributed numbers in known range**
* Often used for **floating-point sorting and as radix/counting extensions**
* Choice of bucket size strongly affects performance

---