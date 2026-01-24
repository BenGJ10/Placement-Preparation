# Prefix Sum

**Prefix Sum** is a technique used to preprocess an array so that **range queries** (sum, count, frequency, balance) can be answered efficiently.

It converts repeated **O(n)** range computations into **O(1)** queries after **O(n)** preprocessing.

---

## Core Idea

Instead of recomputing sums repeatedly:

```
sum[l..r] = a[l] + a[l+1] + ... + a[r]
```

We precompute:

```
prefix[i] = a[0] + a[1] + ... + a[i]
```

Then:

```
sum[l..r] = prefix[r] − prefix[l−1]
```

This is the foundation of **range-based problem solving**.

---

## Why Prefix Sum Is Important

Prefix sum is used in:

* range sum queries
* subarray sum problems
* sliding window optimizations
* difference array technique
* matrix sum queries
* balancing problems (0s and 1s)
* competitive programming + interviews

Many “hard” problems reduce to prefix sums once identified.

---

## 1. Building Prefix Sum Array

---

### 1D Prefix Sum Construction

```cpp
vector<int> buildPrefixSum(vector<int>& a) {
    int n = a.size();
    vector<int> prefix(n);
    prefix[0] = a[0];

    for (int i = 1; i < n; i++)
        prefix[i] = prefix[i - 1] + a[i];

    return prefix;
}
```

---

### Range Sum Query (O(1))

```cpp
int rangeSum(vector<int>& prefix, int l, int r) {
    if (l == 0) return prefix[r];
    return prefix[r] - prefix[l - 1];
}
```

---

## Time Complexity

* preprocessing → O(n)
* each query → O(1)

---

## 2. Prefix Sum with HashMap

Used when:

* array has negative numbers
* sliding window fails
* need count of subarrays with condition

---

## Subarray Sum Equals K

### Brute Force

O(n²) — unacceptable.

### Prefix Sum + HashMap

O(n)

---

### Intuition

If:

```
prefix[j] − prefix[i] = k
```

Then subarray `(i+1 ... j)` sums to `k`.

So we track how many times a prefix sum has appeared.

---

### C++ Code

```cpp
int subarraySum(vector<int>& a, int k) {
    unordered_map<int, int> freq;
    freq[0] = 1;

    int prefixSum = 0, count = 0;

    for (int x : a) {
        prefixSum += x;

        if (freq.count(prefixSum - k))
            count += freq[prefixSum - k];

        freq[prefixSum]++;
    }
    return count;
}
```

---

## Why Sliding Window Fails Here

Sliding window **does not work** with negative numbers.
Prefix sum + hashmap **handles negatives safely**.

---

## 3. Longest Subarray with Given Sum

---

### Key Idea

Store **first occurrence** of each prefix sum.

```cpp
int longestSubarraySumK(vector<int>& a, int k) {
    unordered_map<int, int> first;
    int prefix = 0, ans = 0;

    for (int i = 0; i < a.size(); i++) {
        prefix += a[i];

        if (prefix == k)
            ans = i + 1;

        if (first.count(prefix - k))
            ans = max(ans, i - first[prefix - k]);

        if (!first.count(prefix))
            first[prefix] = i;
    }
    return ans;
}
```

---

## 4. Prefix Sum for Binary Arrays (0/1 Trick)

Convert:

```
0 → -1
1 → +1
```

Now problems become sum-based.

---

## Longest Subarray with Equal 0s and 1s

```cpp
int findMaxLength(vector<int>& a) {
    unordered_map<int,int> first;
    int prefix = 0, ans = 0;

    first[0] = -1;

    for (int i = 0; i < a.size(); i++) {
        prefix += (a[i] == 0 ? -1 : 1);

        if (first.count(prefix))
            ans = max(ans, i - first[prefix]);
        else
            first[prefix] = i;
    }
    return ans;
}
```

---

## 5. Prefix Sum in Strings

Used in:

* balanced parentheses
* vowel/consonant tracking
* frequency parity problems

---

## Example: Maximum Difference Between 0s and 1s

Same transformation logic applies.

---

## 6. Prefix Sum in 2D Matrix

---

## 2D Prefix Sum Definition

```
prefix[i][j] = sum of rectangle (0,0) → (i,j)
```

---

### Build 2D Prefix Sum

```cpp
vector<vector<int>> build2DPrefix(vector<vector<int>>& mat) {
    int m = mat.size(), n = mat[0].size();
    vector<vector<int>> prefix(m, vector<int>(n));

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            prefix[i][j] = mat[i][j];
            if (i > 0) prefix[i][j] += prefix[i - 1][j];
            if (j > 0) prefix[i][j] += prefix[i][j - 1];
            if (i > 0 && j > 0) prefix[i][j] -= prefix[i - 1][j - 1];
        }
    }
    return prefix;
}
```

---

## Rectangle Sum Query (O(1))

```cpp
int rectangleSum(vector<vector<int>>& p, int r1, int c1, int r2, int c2) {
    int res = p[r2][c2];
    if (r1 > 0) res -= p[r1 - 1][c2];
    if (c1 > 0) res -= p[r2][c1 - 1];
    if (r1 > 0 && c1 > 0) res += p[r1 - 1][c1 - 1];
    return res;
}
```

---

## 7. Difference Array (Advanced Prefix Sum)

Used for **range updates** efficiently.

---

## Range Update Trick

Instead of updating full range:

```cpp
diff[l] += val;
diff[r + 1] -= val;
```

Final array obtained by prefix sum of `diff`.

---

## Example Code

```cpp
vector<int> applyRangeUpdates(int n, vector<vector<int>>& updates) {
    vector<int> diff(n + 1, 0);

    for (auto& u : updates) {
        int l = u[0], r = u[1], val = u[2];
        diff[l] += val;
        diff[r + 1] -= val;
    }

    vector<int> a(n);
    a[0] = diff[0];
    for (int i = 1; i < n; i++)
        a[i] = a[i - 1] + diff[i];

    return a;
}
```

---

## Common Prefix Sum Patterns

| Pattern               | Tool                 |
| --------------------- | -------------------- |
| Range sum             | Prefix array         |
| Subarray sum equals K | Prefix + HashMap     |
| Longest subarray      | First occurrence map |
| 0/1 balance           | Transform + prefix   |
| 2D rectangle sum      | 2D prefix            |
| Range updates         | Difference array     |

---

## Common Mistakes

* forgetting prefix[0] initialization
* off-by-one errors in range queries
* not handling l = 0 case
* using sliding window with negatives
* overflow (use long long when needed)
* not initializing hashmap with `{0:1}`

---

## Interview Questions 

### Q1. Why prefix sum works for subarray problems?

Because any subarray sum can be represented as **difference of two prefix sums**.

---

### Q2. Why hashmap is needed?

To track how many times a prefix sum has appeared for counting and longest-length problems.

---

### Q3. Why sliding window fails with negatives?

Because window expansion/shrinking no longer preserves monotonicity.

---

### Q4. Time complexity of prefix sum approach?

Usually **O(n)** or **O(n²)** for 2D variants.

---

### Q5. Why difference array is useful?

It converts **range updates** from O(n) to O(1).

---

## LeetCode Problems to Practice Prefix Sum

* [https://leetcode.com/problems/range-sum-query-immutable/](https://leetcode.com/problems/range-sum-query-immutable/)
* [https://leetcode.com/problems/subarray-sum-equals-k/](https://leetcode.com/problems/subarray-sum-equals-k/)
* [https://leetcode.com/problems/continuous-subarray-sum/](https://leetcode.com/problems/continuous-subarray-sum/)
* [https://leetcode.com/problems/find-pivot-index/](https://leetcode.com/problems/find-pivot-index/)
* [https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/)
* [https://leetcode.com/problems/contiguous-array/](https://leetcode.com/problems/contiguous-array/)
* [https://leetcode.com/problems/range-sum-query-2d-immutable/](https://leetcode.com/problems/range-sum-query-2d-immutable/)
* [https://leetcode.com/problems/corporate-flight-bookings/](https://leetcode.com/problems/corporate-flight-bookings/)
* [https://leetcode.com/problems/matrix-block-sum/](https://leetcode.com/problems/matrix-block-sum/)
* [https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/](https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/)

---

## Summary

* Prefix sum is a preprocessing technique

* Converts repeated range calculations to O(1)

* Essential for subarray and matrix problems

* HashMap + prefix sum is extremely powerful

* Foundation for sliding window, DP, and advanced tricks

---

