# Problems on Binary Trees

Binary tree questions in placements are mostly pattern-based. Once you identify traversal or structural requirements, the implementation becomes straightforward.

This note focuses on problem types, templates, and common mistakes.

---

## Problem Classification

Most binary tree problems fall into one of these categories:

1. Traversal based (preorder/inorder/postorder/level order)

2. Path based (root-to-leaf, path sum, max path)

3. Structural checks (balanced, symmetric, identical)

4. Transformations (invert, flatten, serialize)

5. Construction/reconstruction (from traversals)

6. Ancestor/Distance queries (LCA, nodes at distance k)

---

## Core Node Definition

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

---

## Pattern 1: DFS Recursion Template

Use this for most tree questions.

```cpp
void dfs(TreeNode* node) {
    if (!node) return;

    // preorder work
    dfs(node->left);
    // inorder work
    dfs(node->right);
    // postorder work
}
```

When result depends on children, postorder is usually the right choice.

---

## Pattern 2: Level Order (BFS)

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> ans;
    if (!root) return ans;

    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        int sz = q.size();
        vector<int> level;
        while (sz--) {
            TreeNode* cur = q.front(); q.pop();
            level.push_back(cur->val);
            if (cur->left) q.push(cur->left);
            if (cur->right) q.push(cur->right);
        }
        ans.push_back(level);
    }
    return ans;
}
```

Used in level-wise traversal, left/right view, zig-zag style variants.

---

## Important Interview Problems

## 1) Height / Maximum Depth

```cpp
int maxDepth(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
}
```

Complexity: `O(n)` time, `O(h)` recursion stack.

---

## 2) Check Balanced Binary Tree

```cpp
int heightOrFail(TreeNode* node) {
    if (!node) return 0;
    int lh = heightOrFail(node->left);
    if (lh == -1) return -1;
    int rh = heightOrFail(node->right);
    if (rh == -1) return -1;
    if (abs(lh - rh) > 1) return -1;
    return 1 + max(lh, rh);
}

bool isBalanced(TreeNode* root) {
    return heightOrFail(root) != -1;
}
```

Key trick: combine check + height in one traversal.

---

## 3) Diameter of Binary Tree

```cpp
int diameter = 0;

int height(TreeNode* node) {
    if (!node) return 0;
    int l = height(node->left);
    int r = height(node->right);
    diameter = max(diameter, l + r);
    return 1 + max(l, r);
}

int diameterOfBinaryTree(TreeNode* root) {
    height(root);
    return diameter;
}
```

---

## 4) Lowest Common Ancestor (LCA)

```cpp
TreeNode* lca(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q) return root;

    TreeNode* left = lca(root->left, p, q);
    TreeNode* right = lca(root->right, p, q);

    if (left && right) return root;
    return left ? left : right;
}
```

---

## 5) Root to Leaf Path Sum

```cpp
bool hasPathSum(TreeNode* root, int targetSum) {
    if (!root) return false;
    if (!root->left && !root->right) return targetSum == root->val;
    return hasPathSum(root->left, targetSum - root->val) ||
           hasPathSum(root->right, targetSum - root->val);
}
```

---

## Common Mistakes

* Not handling `root == nullptr`

* Confusing node count vs edge count (diameter/height)

* Global variable not reset between test cases

* Doing repeated height calculations leading to `O(n^2)`

* Forgetting leaf condition in path sum problems

---

## Complexity Cheat Sheet

* Most traversal/check problems: `O(n)` time

* Recursion stack: `O(h)` where `h` is tree height

* Worst-case skew tree: `h = n`

* Balanced tree: `h = log n`

---

## Practice Sequence

1. Traversals (recursive + iterative)

2. Height, size, max value

3. Balanced tree, diameter, identical trees

4. LCA, path sum, views (left/right/top)

5. Serialization and reconstruction

---
