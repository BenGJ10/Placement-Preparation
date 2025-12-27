# 1D Dynamic Programming

**1D Dynamic Programming** refers to DP solutions where the state can be represented using a **single dimension**, typically:

```
dp[x] → answer for state x
```

Instead of `dp[i][j]`, we compress to **one dimension** to save memory and often improve performance.

The main idea:

> If a DP transition depends only on **one previous dimension (or previous row)**, we can reduce the storage to 1D.

---

## Why 1D DP Matters

1D DP is important because:

* reduces space from O(n·m) or O(n²) → **O(n)**
* simplifies code
* helps in competitive programming constraints
* shows optimization thinking in interviews

Many classic problems that look like 2D DP actually work with 1D.

Example problem classes:

* knapsack family
* subset & partition DP
* coin change
* climbing stairs / Fibonacci
* house robber
* LIS (optimized variants)

---

## Key Idea Behind 1D Compression

Start from a **2D DP relation**:

```
dp[i][t] depends on dp[i-1][...]
```

Then realize:

* row `i` only depends on row `i−1`
* therefore we only need **two rows**
* or a **single row updated in correct order**

Thus compress 2D → 1D.

---

## When Can We Use 1D DP?

You can switch to 1D DP when:

* state transition depends only on the **immediately previous row**
* order of updates is carefully maintained
* each state does not depend on itself after update

---

## VERY IMPORTANT RULE

### Forward vs Backward Iteration

* **Unbounded choices** (coin change, unlimited use):
  → iterate **forward**

* **0/1 choice** (each item used once):
  → iterate **backward**

Reason: to avoid overwriting useful states.

---

## Example 1 — Climbing Stairs

Problem:
Number of ways to reach `n`th stair taking 1 or 2 steps.

### Recurrence

```
dp[n] = dp[n-1] + dp[n-2]
```

### 1D DP Implementation

```cpp
int climbStairs(int n) {
    vector<int> dp(n+1);
    dp[0] = 1;
    dp[1] = 1;

    for (int i = 2; i <= n; i++)
        dp[i] = dp[i-1] + dp[i-2];

    return dp[n];
}
```

### Space-optimized version (constant space)

```cpp
int climbStairs(int n) {
    int prev2 = 1, prev1 = 1;
    for (int i = 2; i <= n; i++) {
        int cur = prev1 + prev2;
        prev2 = prev1;
        prev1 = cur;
    }
    return prev1;
}
```

This is also **1D DP**, further compressed.

---

## Example 2 — 0/1 Knapsack

We want maximum value within capacity `W`.

### Standard 2D State

```
dp[i][w] = best using first i items with capacity w
```

We compress since only `i−1` row matters.

### 1D DP Code (Backward Iteration)

```cpp
int knapsack(int n, int W, vector<int>& wt, vector<int>& val) {
    vector<int> dp(W+1, 0);

    for (int i = 0; i < n; i++) {
        for (int w = W; w >= wt[i]; w--) {
            dp[w] = max(dp[w], val[i] + dp[w - wt[i]]);
        }
    }
    return dp[W];
}
```

### Why backward?

Because each item must be used **only once**.

If iterated forward, same item would be counted multiple times.

---

## Example 3 — Unbounded Knapsack (1D DP, Forward Iteration)

Here each item can be used **multiple times**.

### 1D Implementation

```cpp
int unboundedKnapsack(int n, int W, vector<int>& wt, vector<int>& val) {
    vector<int> dp(W+1, 0);

    for (int i = 0; i < n; i++) {
        for (int w = wt[i]; w <= W; w++) {
            dp[w] = max(dp[w], val[i] + dp[w - wt[i]]);
        }
    }
    return dp[W];
}
```

### Why forward now?

Because same item **can be reused**.

---

## Example 4 — Subset Sum / Partition DP

### Problem

Check if subset with sum `target` exists.

### 1D DP Code

```cpp
bool subsetSum(vector<int>& arr, int target) {
    vector<bool> dp(target+1, false);
    dp[0] = true;

    for (int num : arr) {
        for (int t = target; t >= num; t--) {
            dp[t] = dp[t] || dp[t-num];
        }
    }
    return dp[target];
}
```

Backward loop again because each element used once.

---

## Example 5 — Coin Change

Count number of ways to make sum.

### Recurrence

Ways to make sum:

* include coin
* exclude coin

### 1D Implementation (Forward Iteration)

```cpp
int change(int amount, vector<int>& coins) {
    vector<int> dp(amount+1, 0);
    dp[0] = 1;

    for (int coin : coins) {
        for (int x = coin; x <= amount; x++) {
            dp[x] += dp[x - coin];
        }
    }
    return dp[amount];
}
```

Forward because coins are unlimited.

---

## How To Convert 2D DP → 1D DP

1. Define original DP state
   `dp[i][j]`

2. Check transition dependency
   does it depend only on previous i−1?

3. Replace 2D by 1D
   `dp[j]`

4. Choose correct direction

   * backward → 0/1
   * forward → unbounded

5. Validate with small dry run

This reasoning is heavily tested in interviews.

---

## Common Mistakes in 1D DP

* using forward loop instead of backward
* overwriting dp values required later
* assuming 1D is always possible
* forgetting base initialization
* mixing unbounded and 0/1 transition logic

---

## Interview Questions (With Answers)

### Q1. When is backward iteration necessary?

When each element must be **used at most once** (0/1 knapsack, subset sum).

### Q2. When is forward iteration necessary?

When elements may be **reused unlimited times** (unbounded knapsack, coin change).

### Q3. Does 1D always work?

No. If a transition depends on more than one previous row/column, 2D is required.

### Q4. Why does 1D DP save time?

Time is same; **memory reduces drastically**.

---

## LeetCode Problems for 1D DP Practice

* [https://leetcode.com/problems/house-robber/](https://leetcode.com/problems/house-robber/)
* [https://leetcode.com/problems/house-robber-ii/](https://leetcode.com/problems/house-robber-ii/)
* [https://leetcode.com/problems/climbing-stairs/](https://leetcode.com/problems/climbing-stairs/)
* [https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)
* [https://leetcode.com/problems/coin-change-2/](https://leetcode.com/problems/coin-change-2/)
* [https://leetcode.com/problems/partition-equal-subset-sum/](https://leetcode.com/problems/partition-equal-subset-sum/)
* [https://leetcode.com/problems/last-stone-weight-ii/](https://leetcode.com/problems/last-stone-weight-ii/)
* [https://leetcode.com/problems/target-sum/](https://leetcode.com/problems/target-sum/)
* [https://leetcode.com/problems/minimum-cost-for-tickets/](https://leetcode.com/problems/minimum-cost-for-tickets/)
* [https://leetcode.com/problems/delete-and-earn/](https://leetcode.com/problems/delete-and-earn/)

---

## Summary

* 1D DP reduces memory from O(n²) → O(n)

* use when only previous row/state is required

* direction of iteration is critical:

  * backward → 0/1 use once
  * forward → unbounded reuse

* many classic problems are secretly 1D DP problems

---

