# Radix Sort

**Radix Sort** is a **non-comparison sorting algorithm** that sorts numbers **digit by digit** (or bit by bit), from least significant digit to most significant digit or vice versa.

Instead of comparing elements directly, it uses **stable sorting on each digit**, typically **Counting Sort** as a subroutine.

Radix sort can run in **O(d · (n + k))** time, where:

* `n` = number of elements
* `d` = number of digits
* `k` = base/range of digits (e.g., 0–9 in decimal)

---

## Core Intuition

Example numbers:

```
170 45 75 90 802 24 2 66
```

Sort by:

1. units digit
2. tens digit
3. hundreds digit

After processing each digit with a **stable sort**, the full list becomes sorted.

---

## Why Radix Sort Works

Radix sort relies on:

* **stability** of the intermediate sorting algorithm
* sorting from least significant digit upward

If each digit is stably sorted, previously ordered lower digits **remain consistent**, producing a correct global order.

---

## LSD vs MSD Radix Sort

### LSD — Least Significant Digit First (most common)

* process digits from **right → left**
* preferred for integers
* uses counting sort per digit

### MSD — Most Significant Digit First

* process from **left → right**
* used sometimes for strings
* recursive bucketing method

Most interview questions implicitly mean **LSD Radix Sort** unless specified.

---

## When to Use Radix Sort

Use radix sort when:

* input size is large
* numbers have bounded digits
* keys are integers
* performance requirement is near linear
* comparison-based lower bound O(n log n) is not desired

Avoid when:

* numbers have very large range (too many digits)
* floating points without preprocessing
* memory is highly constrained

---

## Radix Sort vs Counting Sort vs Bucket Sort

| Algorithm     | Idea                      | When Used                       |
| ------------- | ------------------------- | ------------------------------- |
| Counting Sort | Count occurrences         | Small range integers            |
| Bucket Sort   | Group values into buckets | Uniform distribution            |
| Radix Sort    | Digit-wise sorting        | Large lists with bounded digits |

---

## Radix Sort Using Counting Sort (Base 10)

We repeatedly apply **stable counting sort on each digit**.

Digit extraction:

```
digit = (number / exp) % 10
exp = 1, 10, 100, ...
```

---

## C++ Implementation (Non-negative integers)

```cpp
void countingSortByDigit(vector<int>& a, int exp) {
    int n = a.size();
    vector<int> output(n);
    int count[10] = {0};

    // Count frequency of digits
    for (int x : a) {
        int digit = (x / exp) % 10;
        count[digit]++;
    }

    // Prefix sums
    for (int i = 1; i < 10; i++)
        count[i] += count[i - 1];

    // Build output array (iterate backward for stability)
    for (int i = n - 1; i >= 0; i--) {
        int digit = (a[i] / exp) % 10;
        output[count[digit] - 1] = a[i];
        count[digit]--;
    }

    a = output;
}

void radixSort(vector<int>& a) {
    if (a.empty()) return;

    int mx = *max_element(a.begin(), a.end());

    for (int exp = 1; mx / exp > 0; exp *= 10)
        countingSortByDigit(a, exp);
}
```

This version handles **non-negative integers** only.

---

## Handling Negative Numbers

Split into:

* negatives
* non-negatives

Sort separately, then combine:

1. Convert negatives to positive
2. Apply radix sort
3. Reverse negatives part
4. Append positive sorted part

This is generally acceptable for interview answers.

---

## Time and Space Complexity

### Time Complexity

```
O(d · (n + k))
```

Commonly simplified to:

```
O(n log_k M)
```

Where:

* `M` = maximum value
* `k` = base (e.g., 10 or 2)

### Space Complexity

```
O(n + k)
```

Required due to counting array and output array.

---

## Base Choice (Important Detail)

Most common bases:

* base 10 → decimal digits
* base 2^8 or 2^16 → fast bitwise radix sort
* base 2 (bitwise radix sort), but deeper passes

Trade-off:

* larger base reduces number of passes
* but increases per-pass bucket size

---

## Radix Sort on Strings

MSD Radix Sort is used for:

* equal-length strings
* lexicographical order

Bucket according to characters (base = alphabet size).

---

## Why Radix Sort Beats Comparison Sorts Sometimes

Comparison sorting lower bound:

```
Ω(n log n)
```

Radix sort is **non-comparison**:

* avoids lower bound
* achieves near linear time
* ideal for large keys with fixed digit length

However, memory and digit count matter.

---

## Advantages

* near-linear performance for bounded digit size
* non-comparison sorting
* stable
* good for integers and strings

---

## Disadvantages

* extra memory required
* digit-by-digit processing
* performance depends on number of digits
* not suitable for very large range integers without optimization

---

## Common Interview Pitfalls

* forgetting stability requirement
* assuming always O(n)
* ignoring memory overhead
* using unstable intermediate sorting
* mixing negative/positive numbers incorrectly

---

## Interview Questions

### Q1. Why must counting sort be stable inside radix sort?

Without stability, previously sorted lower digits get scrambled, breaking overall ordering.

---

### Q2. Is radix sort comparison-based?

No. Sorting decision is based on **digit extraction**, not element comparison.

---

### Q3. Why does radix sort fail when digit size is unbounded?

Because number of passes d becomes very large → performance degrades.

---

### Q4. Which is faster: radix or quicksort?

Depends:

* small, fixed key range → radix faster
* large general-purpose data → quicksort wins

---

### Q5. Can radix sort work with floating numbers?

Yes, after mapping them to integers or fixed-point representation.

---

## Summary

* Radix sort sorts numbers digit by digit
* Uses stable counting sort as a subroutine
* Time complexity O(d·(n+k))
* Works best when number of digits is small and fixed
* Non-comparison method → can beat O(n log n)

---
