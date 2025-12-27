# From Recursion to Optimized Dynamic Programming

Dynamic Programming usually starts as a **simple recursive solution**, which is then systematically optimized in 3 stages:

1. Recursion (pick / not pick)
2. Memoization (Top-Down DP)
3. Tabulation (Bottom-Up DP)
4. Space Optimization

Understanding the transitions is **more important than memorizing formulas** in interviews.

---

## Core Recurrence Pattern: Pick / Not Pick

Many DP problems involve making a **choice**:

* pick something
* do not pick it

Examples:

* subset sum
* knapsack
* coin change
* partition problems

We will demonstrate using the **subset sum / knapsack-style pattern**:

> Determine whether there exists a subset with sum = target

---

## 1. Recursion (Pick / Not Pick)

### Intuition

For each index `i` you can:

* **not pick** the element → move to previous index
* **pick** it → reduce target by value

---

### Recursive Relation

```
f(i, target):
    true if target can be formed using first i elements
```

Choices:

* **not pick** element i
* **pick** element i (if value ≤ target)

---

### Recursive Code (Exponential)

```cpp
bool f(int i, int target, vector<int> &arr) {
    if (target == 0) return true;
    if (i == 0) return arr[0] == target;

    bool notPick = f(i-1, target, arr);

    bool pick = false;
    if (arr[i] <= target)
        pick = f(i-1, target - arr[i], arr);

    return pick || notPick;
}
```

### Time Complexity

* **O(2^n)** — explores all subsets
* Recomputes the same states again and again

This is the point where interviewer asks:

> Can you optimize this?

---

## 2. Memoization (Top-Down DP)

We observe overlapping subproblems such as:

```
f(4, 7)
f(3, 7)
f(2, 7)
```

So we **store results** in a dp table.

---

### DP State

```
dp[i][target] = result of f(i, target)
```

---

### Memoized Code

```cpp
int dp[205][20005];

bool f(int i, int target, vector<int> &arr) {
    if (target == 0) return true;
    if (i == 0) return arr[0] == target;

    if (dp[i][target] != -1) return dp[i][target];

    bool notPick = f(i-1, target, arr);

    bool pick = false;
    if (arr[i] <= target)
        pick = f(i-1, target - arr[i], arr);

    return dp[i][target] = pick || notPick;
}
```

### Complexity

* Time → **O(n × target)**
* Space → recursion stack + dp table

Memoization converts exponential recursion to **polynomial time**.

---

## 3. Tabulation (Bottom-Up DP)

Memoization uses recursion.
Tabulation **removes recursion**.

We fill DP iteratively from **base case upward**.

---

### DP Table Meaning

```
dp[i][t] = true if subset sum t achievable using first i elements
```

---

### Base Case

1. Target = 0 is always possible

```
dp[i][0] = true
```

2. Only first element case

```
dp[0][arr[0]] = true
```

---

### Tabulation Code

```cpp
bool subsetSum(int n, int target, vector<int> &arr) {
    vector<vector<bool>> dp(n, vector<bool>(target+1, false));

    // target = 0 always true
    for (int i = 0; i < n; i++)
        dp[i][0] = true;

    // first element handling
    if (arr[0] <= target)
        dp[0][arr[0]] = true;

    for (int i = 1; i < n; i++) {
        for (int t = 1; t <= target; t++) {

            bool notPick = dp[i-1][t];

            bool pick = false;
            if (arr[i] <= t)
                pick = dp[i-1][t-arr[i]];

            dp[i][t] = pick || notPick;
        }
    }
    return dp[n-1][target];
}
```

### Complexity

* Time → **O(n × target)**
* Space → **O(n × target)**

---

## 4. Space Optimization

Observe dependency:

```
dp[i][t] depends only on dp[i-1][…]
```

Thus instead of 2D dp:

* keep one previous row
* or even one 1D array (iterating backward)

---

### Space Optimized Code (1D DP)

```cpp
bool subsetSum(int n, int target, vector<int> &arr) {
    vector<bool> prev(target+1, false);

    prev[0] = true;

    if (arr[0] <= target)
        prev[arr[0]] = true;

    for (int i = 1; i < n; i++) {
        vector<bool> cur(target+1, false);
        cur[0] = true;

        for (int t = 1; t <= target; t++) {

            bool notPick = prev[t];

            bool pick = false;
            if (arr[i] <= t)
                pick = prev[t-arr[i]];

            cur[t] = pick || notPick;
        }
        prev = cur;
    }
    return prev[target];
}
```

### Further Optimization

Even 1 array works if iterated **backwards**:

```cpp
for (int i = 1; i < n; i++) {
    for (int t = target; t >= 0; t--) {
        ...
    }
}
```

This reduces space to:

* **O(target)**

---

## What You Should Be Able to Do After This

For any DP question now:

1. Write recursive pick/not-pick
2. Add memoization table
3. Convert to bottom-up
4. Optimize space consciously

This exact flow is expected in interviews.

---

## Interview Questions

### Q1. Why do we go from recursion to DP?

Because recursion recomputes same states and becomes exponential.

---

### Q2. Why does memoization use recursion but tabulation does not?

Memoization = top-down recursion
Tabulation = bottom-up iteration

---

### Q3. When is space optimization possible?

When transition depends only on:

* previous row
* previous column
* fixed number of states

---

### Q4. Why iterate backwards in 1D DP?

To avoid overwriting values **needed later in the same iteration**.

---

## Summary

* Start from **recursion (choice thinking)**

* Add **memoization** to remove recomputation

* Convert to **tabulation** to remove recursion

* Reduce memory with **space optimization**

* Core pattern = **pick vs not pick**

---
