# Tree Views and Advanced Patterns

After basic traversals, interview questions often move to “views” and structural patterns.

This note covers high-frequency patterns built on DFS/BFS.

---

## 1) Right View of Binary Tree

Right view = nodes visible when tree is seen from right side.

### BFS Approach (Level Order)

Take last node of each level.

```cpp
vector<int> rightSideView(TreeNode* root) {
    vector<int> ans;
    if (!root) return ans;

    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        int sz = q.size();
        for (int i = 0; i < sz; i++) {
            TreeNode* node = q.front(); q.pop();
            if (i == sz - 1) ans.push_back(node->val);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
    }
    return ans;
}
```

---

## 2) Left View

Same as right view, but take first node of each level (`i == 0`).

---

## 3) Top View

Top view shows first node seen at each horizontal distance (HD).

Technique:

* BFS with `(node, hd)`
* store first occurrence of each `hd`

```cpp
vector<int> topView(TreeNode* root) {
    vector<int> ans;
    if (!root) return ans;

    map<int, int> mp;
    queue<pair<TreeNode*, int>> q;
    q.push({root, 0});

    while (!q.empty()) {
        auto [node, hd] = q.front(); q.pop();
        if (!mp.count(hd)) mp[hd] = node->val;

        if (node->left) q.push({node->left, hd - 1});
        if (node->right) q.push({node->right, hd + 1});
    }

    for (auto &it : mp) ans.push_back(it.second);
    return ans;
}
```

---

## 4) Vertical Order Traversal

Group nodes by horizontal distance.

Technique:

* BFS with row/column tracking
* data structure often:

```
map<col, map<row, multiset<value>>>
```

This handles sorting requirements in strict variants.

---

## 5) Boundary Traversal

Order:

1. Root
2. Left boundary (excluding leaves)
3. All leaves (left to right)
4. Right boundary in reverse (excluding leaves)

Interview trap: avoid duplicating leaves.

---

## 6) Zigzag Level Order

Level order, but direction alternates each level.

Approach:

* BFS level by level
* fill array normally or reverse index

Complexity remains `O(n)`.

---

## 7) Maximum Width of Binary Tree

Assign index positions like heap:

* left child `2*i + 1`
* right child `2*i + 2`

Width at level = `lastIndex - firstIndex + 1`.

Need normalization by subtracting first index to avoid overflow.

---

## 8) Lowest Common Ancestor (General Binary Tree)

Not BST-based ordering.

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

## 9) Distance K from Target

Pattern:

1. Convert tree to undirected graph using parent pointers
2. BFS from target node
3. Stop at distance `k`

This is a classic “tree + BFS graph thinking” problem.

---

## 10) Construct Tree from Traversals

High-frequency:

* preorder + inorder
* inorder + postorder

Core idea:

* root found from preorder/postorder
* split inorder into left and right
* recurse on ranges

Use hashmap for inorder index to achieve `O(n)`.

---

## 11) Serialize and Deserialize Tree

Common interview format:

* preorder with null marker (`#`)
* or level-order representation

Must preserve structure, not only values.

---

## 12) Pattern Selection Cheat Sheet

| Requirement | Preferred Pattern |
|-------------|-------------------|
| Level-wise answer | BFS |
| Child-first aggregation | Postorder DFS |
| Root-to-leaf paths | Preorder DFS |
| Visibility by distance/column | BFS + map |
| Ancestor relation | DFS recursion |

---

## 13) Common Mistakes

* mixing up top view and vertical order
* forgetting to exclude leaves in boundary sides
* duplicate nodes in boundary traversal
* not sorting tie-cases in vertical traversal
* integer overflow in width indexing

---

## 14) Interview Questions

### Q1. Why is BFS commonly used for views?

Because views are mostly level or distance oriented.

---

### Q2. Why does LCA code work in binary tree?

If one target found in left and other in right, current node is first merge point.

---

### Q3. Why use map in top/vertical view?

To maintain horizontal-distance order for final output.

---

## 15) Practice Problems

* Binary Tree Right Side View
* Binary Tree Zigzag Level Order Traversal
* Vertical Order Traversal of Binary Tree
* Top View / Bottom View (GFG)
* Boundary Traversal (GFG)
* Lowest Common Ancestor of Binary Tree
* All Nodes Distance K in Binary Tree
* Construct Binary Tree from Traversals

---

## Summary

* Tree “view” problems are traversal + coordinate mapping

* BFS is dominant for level/visibility problems

* LCA, construction, and serialization are must-do interview topics

* Most advanced tree questions reduce to a known base pattern

---
