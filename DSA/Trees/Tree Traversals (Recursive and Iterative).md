# Tree Traversals (Recursive and Iterative)

Tree traversal means visiting every node in a specific order.

In interviews, traversal mastery is mandatory because many advanced problems are direct extensions of traversal patterns.

---

## 1) DFS Traversals

### Preorder

Order:

```
Root -> Left -> Right
```

Use cases:

* serialization
* copying tree
* root-first processing

### Inorder

Order:

```
Left -> Root -> Right
```

Use cases:

* BST gives sorted output
* validating BST

### Postorder

Order:

```
Left -> Right -> Root
```

Use cases:

* deleting/freeing tree
* when parent answer depends on children first (height/diameter)

---

## 2) Recursive Templates

```cpp
void preorder(TreeNode* root, vector<int>& ans) {
    if (!root) return;
    ans.push_back(root->val);
    preorder(root->left, ans);
    preorder(root->right, ans);
}
```

```cpp
void inorder(TreeNode* root, vector<int>& ans) {
    if (!root) return;
    inorder(root->left, ans);
    ans.push_back(root->val);
    inorder(root->right, ans);
}
```

```cpp
void postorder(TreeNode* root, vector<int>& ans) {
    if (!root) return;
    postorder(root->left, ans);
    postorder(root->right, ans);
    ans.push_back(root->val);
}
```

Complexity of each:

* Time: `O(n)`
* Recursion stack: `O(h)`

---

## 3) Iterative Traversals

### Iterative Preorder (Stack)

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> ans;
    if (!root) return ans;

    stack<TreeNode*> st;
    st.push(root);

    while (!st.empty()) {
        TreeNode* node = st.top();
        st.pop();
        ans.push_back(node->val);

        if (node->right) st.push(node->right);
        if (node->left) st.push(node->left);
    }
    return ans;
}
```

Push right first, then left, so left is processed first.

---

### Iterative Inorder (Stack + Pointer)

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

        cur = st.top();
        st.pop();
        ans.push_back(cur->val);
        cur = cur->right;
    }
    return ans;
}
```

---

### Iterative Postorder (Two Stacks)

```cpp
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> ans;
    if (!root) return ans;

    stack<TreeNode*> s1, s2;
    s1.push(root);

    while (!s1.empty()) {
        TreeNode* node = s1.top(); s1.pop();
        s2.push(node);

        if (node->left) s1.push(node->left);
        if (node->right) s1.push(node->right);
    }

    while (!s2.empty()) {
        ans.push_back(s2.top()->val);
        s2.pop();
    }
    return ans;
}
```

---

### Iterative Postorder (One Stack, Advanced)

```cpp
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> ans;
    stack<TreeNode*> st;
    TreeNode* cur = root;
    TreeNode* lastVisited = nullptr;

    while (cur || !st.empty()) {
        if (cur) {
            st.push(cur);
            cur = cur->left;
        } else {
            TreeNode* node = st.top();
            if (node->right && lastVisited != node->right) {
                cur = node->right;
            } else {
                ans.push_back(node->val);
                lastVisited = node;
                st.pop();
            }
        }
    }
    return ans;
}
```

---

## 4) BFS / Level Order Traversal

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

---

## 5) Traversal Order Selection Strategy

Use this in interviews:

* Need sorted order in BST -> **Inorder**
* Need children before parent -> **Postorder**
* Need root-first path/state -> **Preorder**
* Need level-wise answer -> **BFS**

---

## 6) Common Variants Based on Traversals

Built on BFS:

* zigzag level order
* right view / left view
* average of levels
* maximum width

Built on DFS:

* root-to-leaf paths
* max path sum
* flatten tree
* tree diameter

---

## 7) Common Mistakes

* forgetting null root handling
* wrong push order in iterative preorder
* infinite loop in iterative inorder due incorrect pointer update
* confusion between postorder one-stack and two-stack logic
* not separating levels correctly in BFS

---

## 8) Interview Questions

### Q1. Why is inorder of BST sorted?

Because BST property enforces left subtree values < root < right subtree values.

---

### Q2. Which traversal is best for deleting/freeing tree?

Postorder, because children are processed before parent.

---

### Q3. Why use queue in level order?

Queue preserves FIFO order, which naturally processes nodes level by level.

---

## 9) Practice Problems

* Binary Tree Inorder Traversal
* Binary Tree Preorder Traversal
* Binary Tree Postorder Traversal
* Binary Tree Level Order Traversal
* Zigzag Level Order Traversal
* Right Side View
* Vertical Order Traversal

---

## Summary

* Traversals are the foundation of all tree problem solving

* Learn both recursive and iterative forms

* DFS and BFS serve different interview requirements

* Most advanced tree problems are traversal + state design

---
