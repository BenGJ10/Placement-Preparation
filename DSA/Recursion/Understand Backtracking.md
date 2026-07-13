# Understand Backtracking

Backtracking is a recursive technique where we:

* build solution step by step
* check validity at each step
* if invalid, undo the choice (backtrack)

It is used when we need to explore all valid combinations/permutations/configurations.

---

## Backtracking vs Recursion

All backtracking uses recursion, but not all recursion is backtracking.

Recursion:

* solves smaller subproblems

Backtracking:

* tries choices
* validates constraints
* undoes choices

---

## Core Pattern

```cpp
void backtrack(state) {
    if (goal reached) {
        save answer;
        return;
    }

    for (choice in possible choices) {
        if (!isValid(choice, state)) continue;

        apply(choice, state);      // choose
        backtrack(updated state);  // explore
        undo(choice, state);       // un-choose
    }
}
```

The `undo` step is the heart of backtracking.

---

## Mental Model

Think of decision tree:

* each level = decision position
* each branch = one choice
* leaves = complete configurations

Backtracking = DFS on this decision tree with pruning.

---

## Example 1: Generate All Subsets

### Problem

Given array `nums`, return all subsets.

### Code

```cpp
void solve(int idx, vector<int>& nums, vector<int>& cur, vector<vector<int>>& ans) {
    if (idx == nums.size()) {
        ans.push_back(cur);
        return;
    }

    // not pick
    solve(idx + 1, nums, cur, ans);

    // pick
    cur.push_back(nums[idx]);
    solve(idx + 1, nums, cur, ans);
    cur.pop_back(); // undo
}
```

Complexity:

* Time: `O(2^n * n)` (to store subsets)
* Space: `O(n)` recursion stack (excluding output)

---

## Example 2: Permutations

### Idea

At each index, choose one unused element.

### Code

```cpp
void permute(vector<int>& nums, vector<int>& cur, vector<int>& used, vector<vector<int>>& ans) {
    if (cur.size() == nums.size()) {
        ans.push_back(cur);
        return;
    }

    for (int i = 0; i < nums.size(); i++) {
        if (used[i]) continue;

        used[i] = 1;
        cur.push_back(nums[i]);

        permute(nums, cur, used, ans);

        cur.pop_back();   // undo
        used[i] = 0;      // undo
    }
}
```

Complexity: `O(n! * n)`

---

## Example 3: N-Queens

### Problem

Place `n` queens on `n x n` board such that no two attack each other.

Choices:

* place queen in one valid column of current row

Constraints:

* same column
* main diagonal
* anti-diagonal

Pruning invalid placements is critical.

---

## Backtracking + Pruning

Backtracking without pruning explores huge search space.

Pruning means:

* reject impossible branch early
* avoid exploring full subtree

For N-Queens, if a queen placement is unsafe, stop immediately.

---

## Example 4: Sudoku Solver

Backtracking approach:

1. find empty cell
2. try digits 1..9
3. check row/col/subgrid validity
4. recurse
5. if failure, undo and try next digit

This is classic constraint satisfaction.

---

## Backtracking State Design

Good state design improves speed:

* current path / board
* index/position
* constraint tracking arrays/sets

Examples:

* `used[]` for permutations
* `col[]`, `diag1[]`, `diag2[]` for N-Queens
* bitmasks for fast validity checks

---

## Duplicate Handling Pattern

In problems with duplicates (e.g., Subsets II, Permutations II):

1. sort input
2. skip duplicates carefully

Example condition:

```cpp
if (i > idx && nums[i] == nums[i - 1]) continue;
```

This prevents repeated outputs.

---

## Backtracking vs DP

| Aspect | Backtracking | DP |
|--------|--------------|----|
| Goal | enumerate/search | optimize/count |
| State reuse | usually little | explicit memoization |
| Nature | tree exploration | overlapping subproblems |
| Typical output | all valid answers | best value / number of ways |

Sometimes both combine (memoized DFS).

---

## How to Identify Backtracking Problems

Look for:

* “Generate all...”
* “Find all possible...”
* “Can we place/arrange... under constraints?”
* “Return all combinations/permutations/subsets”

If output size itself is exponential, backtracking is often expected.

---

## Common Mistakes

* forgetting undo step
* wrong base condition
* modifying shared container incorrectly
* not pruning invalid states
* duplicate outputs due to no sorting/skip logic
* copying large state too often (slow)

---

## Interview Questions

### Q1. Why is backtracking usually exponential?

Because it explores a branching decision tree.

---

### Q2. What is pruning?

Cutting branches early when constraints fail.

---

### Q3. Why do we undo after recursive call?

To restore state before trying next choice.

---

### Q4. When should we use backtracking vs DP?

Use backtracking for enumeration/constraints; use DP for repeated subproblems with optimal/count objective.

---

## Practice Problems

* [Subsets](https://leetcode.com/problems/subsets/)
* [Subsets II](https://leetcode.com/problems/subsets-ii/)
* [Permutations](https://leetcode.com/problems/permutations/)
* [Permutations II](https://leetcode.com/problems/permutations-ii/)
* [Combination Sum](https://leetcode.com/problems/combination-sum/)
* [N-Queens](https://leetcode.com/problems/n-queens/)
* [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)
* [Word Search](https://leetcode.com/problems/word-search/)

---

## Summary

* Backtracking = choose -> explore -> undo
* Best modeled as DFS over decision tree
* Pruning is essential for performance
* State design and duplicate handling are key
* One of the most frequently asked recursion topics

---