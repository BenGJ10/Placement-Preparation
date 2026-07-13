# Partition DP

Partition DP is a Dynamic Programming pattern where we:

* split a range into smaller parts
* try all possible partition points
* combine answers of left and right partitions
* take minimum / maximum / count based on the problem

This pattern appears in many interview problems where an expression, array, or matrix chain must be partitioned optimally.

---

## What Makes a Problem “Partition DP”?

If the problem statement feels like:

* “break the array/string/expression into parts”
* “choose a cut point `k`”
* “solve left subproblem + right subproblem”
* “take min/max over all cuts”

then it is likely Partition DP.

---

## Core State and Transition

Most Partition DP problems use:

```
dp[i][j] = best answer for subarray / substring from i to j
```

Transition usually:

```
dp[i][j] = min/max over all k in [i..j-1] of:
           dp[i][k] + dp[k+1][j] + mergeCost(i, k, j)
```

Where `mergeCost` depends on the specific problem.

---

## Why It Is Hard in Interviews

Candidates often know memoization, but miss:

1. Correct interval state (`i, j`)
2. Correct loop order in tabulation
3. Correct partition loop over `k`
4. Cost function and base case

Partition DP is less about syntax and more about correctly modeling intervals.

---

## Generic Recursive Template (Memoization)

```cpp
int f(int i, int j, vector<int>& arr, vector<vector<int>>& dp) {
    if (i == j) return 0; // base: single element interval

    if (dp[i][j] != -1) return dp[i][j];

    int ans = INT_MAX;

    for (int k = i; k < j; k++) {
        int left = f(i, k, arr, dp);
        int right = f(k + 1, j, arr, dp);
        int cost = left + right + mergeCost(i, k, j, arr);
        ans = min(ans, cost);
    }

    return dp[i][j] = ans;
}
```

---

## Generic Tabulation Order

For interval DP, fill by length:

```cpp
for (int len = 2; len <= n; len++) {
    for (int i = 0; i + len - 1 < n; i++) {
        int j = i + len - 1;
        dp[i][j] = INF;
        for (int k = i; k < j; k++) {
            dp[i][j] = min(dp[i][j],
                           dp[i][k] + dp[k + 1][j] + mergeCost(i, k, j));
        }
    }
}
```

Why length order?

Because `dp[i][j]` depends on smaller intervals.

---

## Example 1: Matrix Chain Multiplication (MCM)

### Problem

Given matrix dimensions array `arr`, where matrix `Ai` has size:

```
arr[i-1] x arr[i]
```

Find minimum scalar multiplications.

### State

```
dp[i][j] = minimum multiplication cost from matrix i to j
```

### Transition

For each partition `k` between `i` and `j`:

```
cost = dp[i][k] + dp[k+1][j] + arr[i-1] * arr[k] * arr[j]
```

### Memoized Code

```cpp
int f(int i, int j, vector<int>& arr, vector<vector<int>>& dp) {
    if (i == j) return 0;
    if (dp[i][j] != -1) return dp[i][j];

    int ans = INT_MAX;
    for (int k = i; k < j; k++) {
        int cost = f(i, k, arr, dp)
                 + f(k + 1, j, arr, dp)
                 + arr[i - 1] * arr[k] * arr[j];
        ans = min(ans, cost);
    }
    return dp[i][j] = ans;
}

int matrixMultiplication(vector<int>& arr) {
    int n = arr.size();
    vector<vector<int>> dp(n, vector<int>(n, -1));
    return f(1, n - 1, arr, dp);
}
```

Complexity:

* Time: `O(n^3)`
* Space: `O(n^2)`

---

## Example 2: Minimum Cost to Cut a Stick

### Idea

After sorting cuts and adding boundaries (`0` and `n`):

```
dp[i][j] = min cost to perform cuts from i to j in sorted cuts array
```

Transition:

```
cost = (cuts[j+1] - cuts[i-1]) + dp[i][k-1] + dp[k+1][j]
```

The segment length contributes to each cut cost.

---

## Example 3: Burst Balloons

### Key Transformation

Add boundaries:

```
nums = [1] + nums + [1]
```

Define:

```
dp[i][j] = max coins by bursting balloons in interval [i..j]
```

Choose last balloon to burst `k`:

```
coins = dp[i][k-1] + dp[k+1][j] + nums[i-1] * nums[k] * nums[j+1]
```

Take maximum over all `k`.

This “choose last operation” trick is common in partition DP.

---

## Example 4: Boolean Parenthesization

Given expression with `T/F` and operators (`&`, `|`, `^`), count number of ways to parenthesize to get `true`.

State:

```
dp[i][j][isTrue]
```

Partition at operators only.

Combine left and right counts based on operator truth table.

This is partition DP + counting.

---

## How to Identify Partition DP Quickly

Checklist:

* Input is an interval (`i..j`) in array/string/expression
* Need optimal/count answer for whole interval
* You can “try every cut k”
* Left and right are independent subproblems
* Final answer is min/max/sum over all cuts

If all match, think `dp[i][j]`.

---

## Common Problems Under Partition DP

1. Matrix Chain Multiplication
2. Minimum Cost to Cut a Stick
3. Burst Balloons
4. Palindrome Partitioning II (min cuts)
5. Boolean Parenthesization
6. Different Ways to Add Parentheses
7. Optimal BST (classical)
8. Merge Stones (advanced with prefix sums)

---

## Base Case Patterns

Different problems use different base conditions:

* `i == j` → one item interval
* `i > j` → empty interval
* for strings, `i >= j` often returns `0`

Always verify base case with problem meaning.

---

## Tabulation Loop Order (Critical)

For interval DP:

```cpp
for (int i = n; i >= 1; i--) {
    for (int j = i; j <= n; j++) {
        // compute dp[i][j]
    }
}
```

or

```cpp
for (int len = 1; len <= n; len++) {
    for (int i = 1; i + len - 1 <= n; i++) {
        int j = i + len - 1;
    }
}
```

If order is wrong, required subproblems won’t be ready.

---

## Common Mistakes

* using wrong interval indexing (0-based vs 1-based mix)
* forgetting to add boundaries (`1`, `0`, `n`) when needed
* partition loop using wrong range for `k`
* wrong base case (`i == j` vs `i > j`)
* not resetting `ans = INF / -INF` for each interval
* off-by-one in merge cost formula

---

## Interview Questions

### Q1. Why does Partition DP usually take O(n^3)?

Because:

* `O(n^2)` states (`i, j`)
* each state tries `O(n)` partitions `k`

So total `O(n^3)`.

---

### Q2. Why does `dp[i][j]` work well here?

Because subproblems are naturally interval-based, and every cut splits interval into two smaller intervals.

---

### Q3. How is Partition DP different from subsequence DP?

Subsequence DP:

* usually pick/not-pick at index
* state like `dp[i][target]`

Partition DP:

* split interval at cut `k`
* state like `dp[i][j]`

---

### Q4. Can we space-optimize Partition DP to 1D?

Usually no, because transitions require many 2D states across intervals.

---

## Practice Problems (LeetCode)

* Matrix Chain Multiplication
* [Burst Balloons](https://leetcode.com/problems/burst-balloons/)
* [Minimum Cost to Cut a Stick](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/)
* [Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)
* [Different Ways to Add Parentheses](https://leetcode.com/problems/different-ways-to-add-parentheses/)
* Boolean Parenthesization

---

## Summary

* Partition DP = solve intervals by trying all cuts
* Common state: `dp[i][j]`
* Transition: left + right + cost
* Fill table by interval length
* Most common complexity: `O(n^3)`
* High-frequency interview pattern after 1D/2D DP basics

---