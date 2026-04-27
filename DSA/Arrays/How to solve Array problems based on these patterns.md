# How to Solve Array Problems Using Core Patterns

Array problems become easy when you stop thinking in terms of random tricks and start mapping each question to a known pattern.

This file gives a practical framework to identify and solve array questions using:

* Two Pointers
* Sliding Window
* Prefix Sum
* Difference Array

---

## Step 1: Identify the Problem Type

Before writing code, classify the question.

### If the question says

* "pair", "triplet", "sorted array" -> think **Two Pointers**
* "longest/shortest subarray", "at most k", "exact sum in window" -> think **Sliding Window**
* "many range sum queries" -> think **Prefix Sum**
* "many range updates" -> think **Difference Array**

Most interview questions hide one of these keywords.

---

## Step 2: Pick the Right Pattern

## A) Two Pointers

### Use when

* array is sorted (or can be sorted)
* need pair/triplet relation
* need in-place compaction/partition

### Template

```cpp
int l = 0, r = n - 1;
while (l < r) {
    // evaluate a[l], a[r]
    // move l or r based on condition
}
```

### Common examples

* Two Sum in sorted array
* 3Sum / 4Sum after sorting
* Remove duplicates from sorted array

---

## B) Sliding Window

### Use when

* question asks about contiguous subarray/substring
* maintain dynamic segment while moving right

### Fixed window template

```cpp
int k = ...;
long long windowSum = 0;
for (int i = 0; i < n; i++) {
    windowSum += a[i];
    if (i >= k) windowSum -= a[i - k];
    if (i >= k - 1) {
        // process current window
    }
}
```

### Variable window template

```cpp
int l = 0;
for (int r = 0; r < n; r++) {
    // include a[r]
    while (/* window invalid */) {
        // remove a[l]
        l++;
    }
    // best answer using [l..r]
}
```

### Common examples

* Longest subarray with sum <= k
* Minimum length subarray with sum >= target
* Longest substring with at most k distinct characters

---

## C) Prefix Sum

### Use when

* repeated range sum queries are needed
* subarray sum relation is easier with cumulative totals

### Formula

Let `pref[i]` be sum of first `i` elements (1-based):

`sum(l..r) = pref[r] - pref[l - 1]`

### Template

```cpp
vector<long long> pref(n + 1, 0);
for (int i = 1; i <= n; i++) {
    pref[i] = pref[i - 1] + a[i - 1];
}

auto rangeSum = [&](int l, int r) {
    return pref[r + 1] - pref[l];
};
```

### Common examples

* Range sum queries
* Count subarrays with sum = k (with hash map)
* Equilibrium index

---

## D) Difference Array

### Use when

* many range increment/decrement operations
* final array needed after all operations

Instead of updating every index in `[l..r]`, do:

* `diff[l] += val`
* `diff[r + 1] -= val` (if in bounds)

Then prefix sum over diff to reconstruct final values.

### Template

```cpp
vector<long long> diff(n + 1, 0);

for (auto &q : queries) {
    int l = q[0], r = q[1], val = q[2];
    diff[l] += val;
    if (r + 1 < n) diff[r + 1] -= val;
}

vector<long long> ans(n);
long long cur = 0;
for (int i = 0; i < n; i++) {
    cur += diff[i];
    ans[i] = a[i] + cur;
}
```

---

## Step 3: Dry Run with 5-6 Elements

Before coding final solution:

* dry run on small input
* verify pointer/window movement
* check boundaries (`0`, `n-1`, empty, single element)
* verify duplicate handling if sorting involved

This catches most bugs early.

---

## Step 4: State Complexity Clearly

In interviews always end with:

* Time complexity
* Space complexity
* Why this is better than brute force

Example:

"Brute force is O(n^2). Using sliding window we process each element at most twice, so O(n) time and O(1) extra space."

---

## Common Mistakes

* Forcing sliding window where negatives break monotonicity

* Forgetting to sort before two pointers where required

* Off-by-one errors in prefix/difference arrays

* Updating answer before shrinking invalid window

* Ignoring integer overflow (`int` vs `long long`)

---

## Quick Decision Checklist

1. Is the subarray/segment contiguous? -> Sliding Window / Prefix

2. Are there many range sums? -> Prefix Sum

3. Are there many range updates? -> Difference Array

4. Need pair/triplet relation? -> Two Pointers

5. Input sorted or sortable? -> Two Pointers after sorting

---
