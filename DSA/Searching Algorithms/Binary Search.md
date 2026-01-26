# Binary Search

**Binary Search** is an efficient searching algorithm used on a **sorted search space** to find an element (or its position) in **O(log n)** time.

Instead of scanning linearly, binary search repeatedly **divides the search space into half**, discarding the irrelevant half at each step.

---

## Core Requirement

Binary Search works only when the **search space is monotonic**.

That means:

* sorted array (ascending / descending)
* monotonic condition (true → false or false → true)

If monotonicity is missing, binary search is invalid.

---

## Basic Idea (Intuition)

Given a sorted array:

```
[2, 4, 6, 8, 10, 12, 14]
```

To search `10`:

1. Check middle → `8`
2. Since `10 > 8`, discard left half
3. Search right half
4. Repeat until found or exhausted

Each step cuts the search space in half.

---

## Binary Search Template (Ascending Order)

```cpp
int binarySearch(vector<int>& a, int target) {
    int low = 0, high = a.size() - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (a[mid] == target)
            return mid;
        else if (a[mid] < target)
            low = mid + 1;
        else
            high = mid - 1;
    }
    return -1;
}
```

---

## Why `mid = low + (high - low) / 2` ?

Avoids integer overflow.

Bad:

```cpp
mid = (low + high) / 2;
```

Good:

```cpp
mid = low + (high - low) / 2;
```

This is **interview-important**.

---

## Binary Search on Descending Array

Only comparison directions change.

```cpp
int binarySearchDesc(vector<int>& a, int target) {
    int low = 0, high = a.size() - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (a[mid] == target)
            return mid;
        else if (a[mid] > target)
            low = mid + 1;
        else
            high = mid - 1;
    }
    return -1;
}
```

---

## Variations of Classic Binary Search

These are **core interview variants** (not binary search on answer).

---

### 1. First Occurrence of an Element

Used when duplicates exist.

### Idea

* when target found, **continue searching left**

```cpp
int firstOccurrence(vector<int>& a, int target) {
    int low = 0, high = a.size() - 1;
    int ans = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (a[mid] == target) {
            ans = mid;
            high = mid - 1;
        }
        else if (a[mid] < target)
            low = mid + 1;
        else
            high = mid - 1;
    }
    return ans;
}
```

---

### 2. Last Occurrence of an Element

### Idea

* when target found, **continue searching right**

```cpp
int lastOccurrence(vector<int>& a, int target) {
    int low = 0, high = a.size() - 1;
    int ans = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (a[mid] == target) {
            ans = mid;
            low = mid + 1;
        }
        else if (a[mid] < target)
            low = mid + 1;
        else
            high = mid - 1;
    }
    return ans;
}
```

---

### 3. Count Occurrences of an Element

```cpp
int countOccurrences(vector<int>& a, int target) {
    int first = firstOccurrence(a, target);
    if (first == -1) return 0;

    int last = lastOccurrence(a, target);
    return last - first + 1;
}
```

---

## Binary Search on Infinite / Unknown Size Array 

We don’t know `high`.

### Trick

1. Start with range `[0, 1]`
2. Double `high` until `a[high] >= target`
3. Apply normal binary search

This tests **search space identification**, not binary logic.

---

## Binary Search on Rotated Sorted Array

Search in array like:

```
[4,5,6,7,0,1,2]
```

### Key observation

* one half is always sorted

```cpp
int searchRotated(vector<int>& a, int target) {
    int low = 0, high = a.size() - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (a[mid] == target) return mid;

        // left half sorted
        if (a[low] <= a[mid]) {
            if (a[low] <= target && target < a[mid])
                high = mid - 1;
            else
                low = mid + 1;
        }
        // right half sorted
        else {
            if (a[mid] < target && target <= a[high])
                low = mid + 1;
            else
                high = mid - 1;
        }
    }
    return -1;
}
```

---

## Binary Search on Condition (NOT Answer Space)

Still classic binary search, but condition-based.

Example:

* find first element ≥ X
* find smallest index satisfying condition

This is **lower_bound / upper_bound logic**.

---

### Lower Bound (First ≥ Target)

```cpp
int lowerBound(vector<int>& a, int target) {
    int low = 0, high = a.size();

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (a[mid] < target)
            low = mid + 1;
        else
            high = mid;
    }
    return low;
}
```

---

### Upper Bound (First > Target)

```cpp
int upperBound(vector<int>& a, int target) {
    int low = 0, high = a.size();

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (a[mid] <= target)
            low = mid + 1;
        else
            high = mid;
    }
    return low;
}
```

---

## Binary Search Loop Conditions (Very Important)

| Loop                  | Use case          |
| --------------------- | ----------------- |
| `while (low <= high)` | exact search      |
| `while (low < high)`  | boundary finding  |
| inclusive bounds      | typical           |
| exclusive bounds      | lower/upper bound |

Choosing wrong condition → infinite loop or wrong answer.

---

## Time and Space Complexity

* Time: **O(log n)**
* Space:

  * Iterative: **O(1)**
  * Recursive: **O(log n)** (call stack)

---

## Common Binary Search Mistakes

* array not sorted
* incorrect mid calculation
* infinite loop (`low` or `high` not moving)
* mixing `low <= high` with `low < high`
* forgetting edge cases
* ignoring duplicates
* off-by-one errors

---

## When to Think of Binary Search

Clues in problem statement:

* “sorted”
* “minimum / maximum”
* “first / last occurrence”
* “monotonic”
* “logarithmic”
* “efficient search”

Binary search is about **shrinking search space**, not just arrays.

---

## Interview Questions 

### Q1. Why binary search is O(log n)?

Because search space halves at every step.

---

### Q2. Why binary search fails on unsorted arrays?

Because monotonicity is required to discard half safely.

---

### Q3. Difference between lower_bound and binary search?

Binary search finds an element.
Lower bound finds a **position** based on condition.

---

### Q4. Why iterative binary search preferred?

Avoids recursion stack overhead and is simpler.

---

### Q5. What is the most common binary search bug?

Wrong loop condition or not updating bounds properly.

---

## LeetCode Problems

* [https://leetcode.com/problems/binary-search/](https://leetcode.com/problems/binary-search/)
* [https://leetcode.com/problems/first-bad-version/](https://leetcode.com/problems/first-bad-version/)
* [https://leetcode.com/problems/search-in-rotated-sorted-array/](https://leetcode.com/problems/search-in-rotated-sorted-array/)
* [https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
* [https://leetcode.com/problems/search-insert-position/](https://leetcode.com/problems/search-insert-position/)
* [https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
* [https://leetcode.com/problems/peak-index-in-a-mountain-array/](https://leetcode.com/problems/peak-index-in-a-mountain-array/)
* [https://leetcode.com/problems/guess-number-higher-or-lower/](https://leetcode.com/problems/guess-number-higher-or-lower/)

---

## Summary

* Binary search works on **monotonic search space**

* Reduces time from O(n) to O(log n)

* Core patterns:

  * exact search
  * first/last occurrence
  * lower/upper bound
  * rotated arrays

* Precision in boundaries is crucial

* Foundation for many advanced techniques (handled separately)

---

