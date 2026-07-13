# Recursion to Iteration

In interviews, writing recursion is good. Converting it to iteration is better.

This shows:

* deeper understanding of call stack
* control over memory usage
* ability to avoid stack overflow

---

## Why Convert Recursion to Iteration?

Recursion drawbacks:

* stack overflow for deep recursion
* hidden call-stack overhead
* sometimes slower in strict constraints

Iteration benefits:

* explicit control of state
* usually lower overhead
* easier to optimize memory

---

## Core Idea

Recursion uses **implicit stack**.
Iteration uses **explicit stack/queue/variables**.

So conversion means:

1. identify recursive state
2. store that state explicitly
3. process in equivalent order

---

## Pattern 1: Linear Recursion -> Loop

### Recursive Sum

```cpp
int sumRec(int n) {
    if (n == 0) return 0;
    return n + sumRec(n - 1);
}
```

### Iterative Sum

```cpp
int sumItr(int n) {
    int ans = 0;
    for (int i = 1; i <= n; i++) ans += i;
    return ans;
}
```

This is the easiest conversion.

---

## Pattern 2: Tail Recursion -> Loop (Direct)

### Tail Recursive Factorial

```cpp
long long factTail(int n, long long acc = 1) {
    if (n == 0) return acc;
    return factTail(n - 1, acc * n);
}
```

### Iterative Equivalent

```cpp
long long factItr(int n) {
    long long acc = 1;
    while (n > 0) {
        acc *= n;
        n--;
    }
    return acc;
}
```

Tail recursion maps cleanly to loops.

---

## Pattern 3: Tree/Graph DFS Recursion -> Explicit Stack

### Recursive DFS (Tree)

```cpp
void dfsRec(int u, vector<vector<int>>& g, vector<int>& vis) {
    vis[u] = 1;
    for (int v : g[u]) {
        if (!vis[v]) dfsRec(v, g, vis);
    }
}
```

### Iterative DFS

```cpp
void dfsItr(int start, vector<vector<int>>& g, vector<int>& vis) {
    stack<int> st;
    st.push(start);

    while (!st.empty()) {
        int u = st.top();
        st.pop();
        if (vis[u]) continue;

        vis[u] = 1;

        for (int i = (int)g[u].size() - 1; i >= 0; i--) {
            int v = g[u][i];
            if (!vis[v]) st.push(v);
        }
    }
}
```

Using reverse push preserves traversal order close to recursive DFS.

---

## Pattern 4: Backtracking Recursion -> Stack of States

For subsets/permutations, recursive state usually contains:

* index
* current path
* visited mask/set

To convert, push full state objects onto stack.

Pseudo:

```cpp
struct State {
    int idx;
    vector<int> path;
};

stack<State> st;
st.push({0, {}});

while (!st.empty()) {
    auto cur = st.top();
    st.pop();

    if (cur.idx == n) {
        ans.push_back(cur.path);
        continue;
    }

    // not pick
    st.push({cur.idx + 1, cur.path});

    // pick
    auto nextPath = cur.path;
    nextPath.push_back(arr[cur.idx]);
    st.push({cur.idx + 1, nextPath});
}
```

This works but may be less readable than recursion.

---

## Pattern 5: DP Recursion -> Tabulation

Many recursive DP solutions are converted by:

1. identify state dimensions
2. identify transition
3. decide filling order

Example:

Recursive Fibonacci:

```cpp
int fibRec(int n) {
    if (n <= 1) return n;
    return fibRec(n-1) + fibRec(n-2);
}
```

Iterative DP:

```cpp
int fibItr(int n) {
    if (n <= 1) return n;
    vector<int> dp(n + 1);
    dp[0] = 0, dp[1] = 1;
    for (int i = 2; i <= n; i++) dp[i] = dp[i-1] + dp[i-2];
    return dp[n];
}
```

---

## How to Simulate Call Stack Correctly

Recursive frame stores:

* parameters
* local variables
* which step to execute next (program counter)

For complex conversions, store all of these in custom struct.

This is important for:

* postorder traversal
* expression evaluation
* recursion with multiple phases

---

## Example: Recursive Binary Tree Inorder -> Iterative

### Recursive

```cpp
void inorder(TreeNode* root, vector<int>& ans) {
    if (!root) return;
    inorder(root->left, ans);
    ans.push_back(root->val);
    inorder(root->right, ans);
}
```

### Iterative

```cpp
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> ans;
    stack<TreeNode*> st;
    TreeNode* cur = root;

    while (cur || !st.empty()) {
        while (cur) {
            st.push(cur);
            cur = cur->left;
        }
        cur = st.top(); st.pop();
        ans.push_back(cur->val);
        cur = cur->right;
    }
    return ans;
}
```

---

## Conversion Checklist (Interview)

When asked “convert recursion to iteration”, do this:

1. Explain recursive state
2. Mention base condition
3. Choose data structure (stack/queue/array vars)
4. Write iterative processing loop
5. Ensure same order as recursion
6. Validate with dry run

---

## Common Mistakes

* using queue when stack semantics required
* wrong push order (changes traversal)
* forgetting to carry local state
* mutating shared vectors incorrectly
* mismatch in base-case handling

---

## Interview Questions

### Q1. Is recursion always slower than iteration?

Not always, but recursion has call-stack overhead and depth limits.

---

### Q2. Which recursive patterns are easiest to convert?

Tail recursion and linear recursion.

---

### Q3. Which are hardest?

Backtracking with complex local state, and multi-branch recursion with post-processing.

---

### Q4. Why use recursion then?

Cleaner expression of tree-like logic; faster to write and reason in many problems.

---

## Practice Problems

* Recursive DFS -> Iterative DFS
* Binary tree traversals (all 3) iterative
* Generate subsets recursively and iteratively
* N-Queens recursion (analyze why iterative is complex)
* Fibonacci recursion -> DP tabulation

---

## Summary

* Recursion uses implicit stack; iteration uses explicit state
* Tail and linear recursion convert directly to loops
* DFS/backtracking need explicit stack of states
* DP recursion converts to tabulation via state transitions
* Conversion skill is highly valued in interviews

---