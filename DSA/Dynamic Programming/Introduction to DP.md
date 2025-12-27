# Introduction to Dynamic Programming (DP)

Dynamic Programming is a **problem-solving paradigm** used to optimize algorithms that exhibit:

1. **Overlapping Subproblems**
2. **Optimal Substructure**

It transforms an **exponential brute-force recursion** into an efficient **polynomial-time** solution by storing the results of already solved subproblems.

---

## What Dynamic Programming Actually Means

At its core:

> Dynamic Programming is “recursion with storage”.

Instead of recomputing the same subproblems again and again, DP **remembers** the results and reuses them.

This prevents exponential blow-up.

---

## Why DP Exists (Real Intuition)

Brute force recursion:

* explores all possibilities
* recomputes the same answers repeatedly
* becomes exponential

Dynamic Programming:

* stores intermediate answers
* avoids recomputation
* reduces time drastically

For example, computing Fibonacci numbers:

* Recursive Fibonacci: **O(2ⁿ)**
* DP Fibonacci: **O(n)**

---

## Key Properties of DP Problems

### 1. Optimal Substructure

The optimal solution to a problem depends on **optimal solutions of its subproblems**.

Example: Shortest path, knapsack, LIS.

---

### 2. Overlapping Subproblems

The same subproblem is solved multiple times.

Example: Fibonacci calls `f(5)` leads to repeated `f(3)`, `f(2)` etc.

If subproblems **do not overlap**, use **divide and conquer**, not DP.

---

## Two Main Approaches in Dynamic Programming

### 1. Top-Down (Memoization)

* Write a recursive solution
* Add a cache (`dp[]`) to store results
* Reuse stored results

### 2. Bottom-Up (Tabulation)

* No recursion
* Build solution iteratively
* Typically uses loops and a DP table

---

## Memoization vs Tabulation (Interview Perspective)

| Feature        | Memoization (Top-Down) | Tabulation (Bottom-Up)  |
| -------------- | ---------------------- | ----------------------- |
| Technique      | Recursion + cache      | Iteration               |
| Code style     | Natural                | Requires thinking order |
| Stack overflow | Possible               | Safe                    |
| When preferred | Easier to write        | Faster & memory clear   |

---

## How to Recognize a DP Problem (Very Important)

Ask yourself:

* Is the problem asking:

  * maximum/minimum
  * counting ways
  * longest/shortest
  * yes/no with choices?
* Does brute force recursion overlap?
* Can I express it as recurrence?

If yes → likely DP.

---

## First DP Example: Fibonacci

### Recursive (brute force, exponential)

```cpp
int fib(int n) {
    if (n <= 1) return n;
    return fib(n-1) + fib(n-2);
}
```

This recomputes values repeatedly.

---

## Memoized Fibonacci (Top-Down DP)

```cpp
int dp[100005];

int fib(int n) {
    if (n <= 1) return n;

    if (dp[n] != -1) return dp[n];

    return dp[n] = fib(n-1) + fib(n-2);
}
```

---

## Tabulated Fibonacci (Bottom-Up DP)

```cpp
int fib(int n) {
    vector<int> dp(n+1);
    dp[0] = 0;
    dp[1] = 1;

    for (int i = 2; i <= n; i++)
        dp[i] = dp[i-1] + dp[i-2];

    return dp[n];
}
```

---

## Canonical Example 2: 0/1 Knapsack (Choice-Based DP)

### Problem Theme

Given:

* weights[]
* values[]
* capacity W

Maximize profit without exceeding capacity.

### Recurrence Intuition

For each item `i`, we either:

* **take it**
* **do not take it**

If taking:

```
value(i) + best remaining capacity
```

If skipping:

```
same capacity, next item
```

---

### 0/1 Knapsack Recursion (Memoization)

```cpp
int dp[105][1005];

int knapsack(int i, int w, vector<int>& wt, vector<int>& val) {
    if (i == 0 || w == 0) return 0;

    if (dp[i][w] != -1) return dp[i][w];

    if (wt[i-1] > w)
        return dp[i][w] = knapsack(i-1, w, wt, val);

    return dp[i][w] = max(
        val[i-1] + knapsack(i-1, w-wt[i-1], wt, val),
        knapsack(i-1, w, wt, val)
    );
}
```

---

## Pattern: How to Solve Any DP Problem Systematically

1. Define state
   Example: `dp[i][w]` = best value for first `i` items with capacity `w`

2. Identify choices
   Example: take or skip item

3. Write recurrence
   Express bigger problem in terms of smaller ones

4. Initialize base cases
   Example: capacity 0 or items 0

5. Choose memoization / tabulation

6. Optimize space if needed

---

## Common Mistakes in Dynamic Programming

* Writing recursion without memoization

* Forgetting to define base conditions

* Incorrect recurrence relation

* Wrong iteration order in tabulation

* Mixing up indexes in string / grid DP

* Trying to memorize before understanding recursion

---

## Interview Questions

### Q1. Why is DP faster than recursion?

Because it **stores and reuses results** of overlapping subproblems instead of recomputing them.

---

### Q2. When should we not use DP?

When subproblems **do not overlap**. Use divide-and-conquer instead.

---

### Q3. Why does memoization use recursion?

Because memoization extends recursion by adding **caching**.

---

### Q4. Why is tabulation iterative?

It builds the solution **from small problems to larger ones**.

---

### Q5. Is greedy algorithm a type of DP?

No, but many greedy algorithms are **special cases of DP** when local optimum = global optimum.

---

## LeetCode Problems to Practice (Introductory DP)

1. **Climbing Stairs**
   [https://leetcode.com/problems/climbing-stairs/](https://leetcode.com/problems/climbing-stairs/)

2. **House Robber**
   [https://leetcode.com/problems/house-robber/](https://leetcode.com/problems/house-robber/)

3. **Fibonacci Number**
   [https://leetcode.com/problems/fibonacci-number/](https://leetcode.com/problems/fibonacci-number/)

4. **Min Cost Climbing Stairs**
   [https://leetcode.com/problems/min-cost-climbing-stairs/](https://leetcode.com/problems/min-cost-climbing-stairs/)

5. **Unique Paths**
   [https://leetcode.com/problems/unique-paths/](https://leetcode.com/problems/unique-paths/)

6. **Coin Change**
   [https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)

7. **Longest Increasing Subsequence**
   [https://leetcode.com/problems/longest-increasing-subsequence/](https://leetcode.com/problems/longest-increasing-subsequence/)

8. **Partition Equal Subset Sum**
   [https://leetcode.com/problems/partition-equal-subset-sum/](https://leetcode.com/problems/partition-equal-subset-sum/)

9. **Longest Common Subsequence**
   [https://leetcode.com/problems/longest-common-subsequence/](https://leetcode.com/problems/longest-common-subsequence/)

10. **Edit Distance**
    [https://leetcode.com/problems/edit-distance/](https://leetcode.com/problems/edit-distance/)

These progress from **easy → medium → foundational hard**, ideal for placements.

---

## Summary

* DP converts exponential recursion into polynomial solutions

* Two pillars: **overlapping subproblems + optimal substructure**

* Two approaches:

  * memoization (top-down)
  * tabulation (bottom-up)

* Recognize DP by patterns:

  * maximize / minimize
  * count ways
  * longest / shortest

* Foundation for advanced topics: knapsack, LCS, LIS, trees, graphs

---
