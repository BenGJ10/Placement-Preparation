# DP on Subsequences

DP on Subsequences is one of the most important Dynamic Programming categories.

It deals with problems where:

* we make a decision at each index
* we either **pick** or **do not pick** an element
* order of elements is preserved
* elements do not need to be contiguous

This is different from subarrays (which are contiguous).

---

## What is a Subsequence?

Given array:

```
[3, 1, 2]
```

Subsequences include:

```
[]
[3]
[1]
[2]
[3,1]
[3,2]
[1,2]
[3,1,2]
```

Subsequence preserves order but skips allowed.

---

## When to Recognize DP on Subsequences

Look for:

* subset problems
* knapsack-type problems
* partition problems
* count number of ways
* target sum
* LCS-style problems
* include/exclude logic

If brute force is:

```
for each element → pick or not pick
```

Then DP on subsequences is likely.

---

## Core Pattern: Pick / Not Pick

At index `i`, you have two choices:

1. Not pick element `i`
2. Pick element `i` (if allowed)

Recurrence often looks like:

```
f(i, target) =
    f(i-1, target)              // not pick
    +
    f(i-1, target - arr[i])     // pick
```

This is the backbone of subsequence DP.

---

## 1. Subset Sum (Classic Foundation Problem)

### Problem

Given array and target sum, determine if a subset sums to target.

---

### Recursive Relation

```
f(i, target) =
    f(i-1, target)
    OR
    f(i-1, target - arr[i])
```

Base cases:

* if target == 0 → true
* if i == 0 → arr[0] == target

---

### Memoization (Top-Down)

```cpp
bool f(int i, int target, vector<int>& arr, vector<vector<int>>& dp) {
    if (target == 0) return true;
    if (i == 0) return arr[0] == target;

    if (dp[i][target] != -1)
        return dp[i][target];

    bool notPick = f(i - 1, target, arr, dp);

    bool pick = false;
    if (arr[i] <= target)
        pick = f(i - 1, target - arr[i], arr, dp);

    return dp[i][target] = pick || notPick;
}
```

---

### Tabulation

```cpp
bool subsetSum(vector<int>& arr, int target) {
    int n = arr.size();
    vector<vector<bool>> dp(n, vector<bool>(target + 1, false));

    for (int i = 0; i < n; i++)
        dp[i][0] = true;

    if (arr[0] <= target)
        dp[0][arr[0]] = true;

    for (int i = 1; i < n; i++) {
        for (int t = 1; t <= target; t++) {
            bool notPick = dp[i - 1][t];
            bool pick = false;
            if (arr[i] <= t)
                pick = dp[i - 1][t - arr[i]];

            dp[i][t] = pick || notPick;
        }
    }

    return dp[n - 1][target];
}
```

---

### Space Optimization (1D)

Backward iteration required (0/1 nature).

```cpp
bool subsetSum(vector<int>& arr, int target) {
    vector<bool> dp(target + 1, false);
    dp[0] = true;

    for (int num : arr) {
        for (int t = target; t >= num; t--) {
            dp[t] = dp[t] || dp[t - num];
        }
    }
    return dp[target];
}
```

---

## 2. 0/1 Knapsack (Value Maximization)

---

### Recurrence

```
dp[i][w] =
    max(
        dp[i-1][w],
        value[i] + dp[i-1][w-weight[i]]
    )
```

---

### 1D Optimized Version

```cpp
int knapsack(vector<int>& wt, vector<int>& val, int W) {
    vector<int> dp(W + 1, 0);

    for (int i = 0; i < wt.size(); i++) {
        for (int w = W; w >= wt[i]; w--) {
            dp[w] = max(dp[w], val[i] + dp[w - wt[i]]);
        }
    }

    return dp[W];
}
```

---

## 3. Count of Subsets with Given Sum

Instead of boolean, count number of ways.

---

### Recurrence

```
f(i, target) =
    f(i-1, target)
    +
    f(i-1, target - arr[i])
```

---

### Code (Tabulation)

```cpp
int countSubsets(vector<int>& arr, int target) {
    int n = arr.size();
    vector<vector<int>> dp(n, vector<int>(target + 1, 0));

    for (int i = 0; i < n; i++)
        dp[i][0] = 1;

    if (arr[0] <= target)
        dp[0][arr[0]] = 1;

    for (int i = 1; i < n; i++) {
        for (int t = 0; t <= target; t++) {
            int notPick = dp[i - 1][t];
            int pick = 0;
            if (arr[i] <= t)
                pick = dp[i - 1][t - arr[i]];

            dp[i][t] = pick + notPick;
        }
    }

    return dp[n - 1][target];
}
```

---

## 4. Target Sum Problem

Transform into subset sum:

Assign + or − to each element.

Trick:

```
S1 - S2 = target
S1 + S2 = total
```

Solve:

```
S1 = (target + total) / 2
```

Then count subsets with sum = S1.

---

## 5. Partition Equal Subset Sum

Check if array can be divided into 2 equal sum subsets.

```
total % 2 != 0 → false
```

Solve subset sum with target = total / 2.

---

## Core Differences from Subarray DP

| Subarray DP             | Subsequence DP       |
| ----------------------- | -------------------- |
| contiguous              | non-contiguous       |
| sliding window possible | not possible         |
| prefix sum useful       | pick/not-pick useful |

---

## Common State Definitions

For subsequence DP:

```
dp[i][target]
dp[i][weight]
dp[i][sum]
dp[i][count]
```

Always depends on previous index `i-1`.

---

## Recognizing 0/1 vs Unbounded

If each element used once → backward iteration
If element reusable → forward iteration

This is critical.

---

## Common Mistakes

* forward loop in 0/1 DP
* wrong base initialization
* forgetting dp[i][0] = true
* integer overflow
* forgetting modulo in count problems
* mixing pick and not-pick incorrectly

---

## Interview Questions (With Answers)

### Q1. Why backward iteration in 1D DP?

To avoid reusing the same element multiple times.

---

### Q2. Why subsequence DP is exponential without memo?

Because every element has 2 choices → 2ⁿ states.

---

### Q3. How do you convert pick/not-pick recursion to tabulation?

Replace recursion tree by dp table filled row-wise.

---

### Q4. How to recognize knapsack-type problem?

When problem involves:

* weights
* capacity
* maximize/minimize
* pick items

---

### Q5. Why does subset sum require 2D DP?

Because state depends on index and target.

---

## LeetCode Problems

* [https://leetcode.com/problems/partition-equal-subset-sum/](https://leetcode.com/problems/partition-equal-subset-sum/)
* [https://leetcode.com/problems/target-sum/](https://leetcode.com/problems/target-sum/)
* [https://leetcode.com/problems/coin-change-2/](https://leetcode.com/problems/coin-change-2/)
* [https://leetcode.com/problems/last-stone-weight-ii/](https://leetcode.com/problems/last-stone-weight-ii/)
* [https://leetcode.com/problems/subset-sum/](https://leetcode.com/problems/subset-sum/)
* [https://leetcode.com/problems/combination-sum-iv/](https://leetcode.com/problems/combination-sum-iv/)
* [https://leetcode.com/problems/ones-and-zeroes/](https://leetcode.com/problems/ones-and-zeroes/)
* [https://leetcode.com/problems/number-of-subsets-with-sum-k/](https://leetcode.com/problems/number-of-subsets-with-sum-k/)
* [https://leetcode.com/problems/partition-to-k-equal-sum-subsets/](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)

---

## Summary

* DP on subsequences is built on pick/not-pick logic

* Used for subset, knapsack, partition, counting problems

* Often 2D → can optimize to 1D

* Requires backward iteration in 0/1 problems

* Extremely common in interviews

---
