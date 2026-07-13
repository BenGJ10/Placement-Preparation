# Introduction to Recursion

Recursion is a technique where a function calls itself to solve smaller instances of the same problem.

It is one of the most important foundations for:

* backtracking
* divide and conquer
* dynamic programming
* tree and graph traversals

---

## What Recursion Actually Means

A recursive function must have:

1. **Base case** (stopping condition)
2. **Recursive case** (reducing problem size)

If either is wrong, recursion fails (wrong answer or infinite calls).

---

## Real Intuition

Think of recursion as:

> “I know how to solve the current problem if I can trust that smaller subproblems are already solved.”

Example:

```
factorial(n) = n * factorial(n-1)
```

You trust `factorial(n-1)` and focus on current step.

---

## Anatomy of Recursive Function

```cpp
returnType solve(parameters) {
    // 1) Base condition
    if (base) return answer;

    // 2) Recursive call on smaller input
    return combine(current, solve(smaller_input));
}
```

---

## Example 1: Factorial

### Recursive Definition

```
n! = n * (n-1)!
0! = 1
```

### Code

```cpp
long long fact(int n) {
    if (n == 0) return 1;
    return 1LL * n * fact(n - 1);
}
```

Complexity:

* Time: `O(n)`
* Space: `O(n)` recursion stack

---

## Example 2: Fibonacci

```cpp
int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}
```

Complexity:

* Time: `O(2^n)` (very slow due to overlap)
* Space: `O(n)`

This example explains why DP is needed.

---

## Recursion Call Stack (Very Important)

Recursive calls are stored in stack frames.

For `fact(4)`:

```
fact(4)
  -> fact(3)
     -> fact(2)
        -> fact(1)
           -> fact(0) returns 1
        <- returns 1
     <- returns 2
  <- returns 6
returns 24
```

Calls go down, answers come back up.

---

## Tail vs Non-Tail Recursion

### Tail Recursion

Recursive call is the last operation.

```cpp
int sumTail(int n, int acc = 0) {
    if (n == 0) return acc;
    return sumTail(n - 1, acc + n);
}
```

### Non-Tail Recursion

Work happens after recursive call.

```cpp
int sum(int n) {
    if (n == 0) return 0;
    return n + sum(n - 1);
}
```

Tail recursion can be optimized by compilers in some languages.

---

## Recursion Tree Concept

Many recursion problems can be visualized as a tree:

* each node = function call
* each edge = recursive branch
* leaves = base cases

For Fibonacci, this tree grows exponentially.

For binary choice problems, tree has branching factor 2.

---

## Where Recursion Is Commonly Used

1. Tree traversals (pre/in/post-order)
2. DFS in graphs
3. Backtracking (subsets, permutations, N-Queens)
4. Divide and conquer (merge sort, quick sort)
5. DP top-down memoization
6. Math definitions (gcd, power, factorial)

---

## How to Write Recursive Solutions in Interviews

Use this order:

1. Define function meaning clearly
2. Write base case(s)
3. Write recursive relation
4. Dry run with small input
5. Check stack depth and complexity

This makes your explanation structured and convincing.

---

## Common Mistakes

* missing base case
* wrong base case value
* not reducing problem size
* stack overflow due to deep recursion
* exponential recursion without memoization
* off-by-one in index recursion

---

## Interview Questions

### Q1. What are the two essential parts of recursion?

Base case and recursive case.

---

### Q2. Why does recursion use extra space?

Because each recursive call creates a new stack frame.

---

### Q3. Why is Fibonacci recursion slow?

It recomputes overlapping subproblems repeatedly.

---

### Q4. Can every recursion be converted to iteration?

Yes in principle, by explicitly simulating stack/state.

---

## Practice Problems

* Factorial
* Power (`x^n`)
* Fibonacci
* Sum of digits
* Reverse string
* Check palindrome
* Binary search (recursive)
* Tree traversals

---

## Summary

* Recursion solves problems by reducing to smaller same-type problems
* Must have correct base case and reduction
* Call stack is central to understanding recursion
* Recursion is foundation for backtracking and DP
* Always analyze both time and stack space

---