# Binary Search Trees (BST)

A **Binary Search Tree (BST)** is a binary tree with ordering property:

* all values in left subtree < root value
* all values in right subtree > root value

This property makes search, insert, and delete efficient in balanced cases.

---

## 1) Why BST Is Important

BST questions are among the most frequent placement topics because they test:

* recursion
* ordering invariants
* pointer manipulation
* edge-case handling

---

## 2) BST Property and Inorder

Key fact:

> Inorder traversal of BST gives sorted sequence.

This fact is used in:

* BST validation
* kth smallest/largest
* recovering swapped BST

---

## 3) Search in BST

```cpp
TreeNode* searchBST(TreeNode* root, int key) {
    while (root) {
        if (root->val == key) return root;
        if (key < root->val) root = root->left;
        else root = root->right;
    }
    return nullptr;
}
```

Complexity:

* Average: `O(log n)`
* Worst (skewed): `O(n)`

---

## 4) Insert in BST

```cpp
TreeNode* insertIntoBST(TreeNode* root, int val) {
    if (!root) return new TreeNode(val);

    if (val < root->val)
        root->left = insertIntoBST(root->left, val);
    else
        root->right = insertIntoBST(root->right, val);

    return root;
}
```

---

## 5) Delete in BST (Most Asked)

Cases for deleting node `x`:

1. No child -> remove directly
2. One child -> connect parent to child
3. Two children -> replace with inorder successor/predecessor, then delete replacement node

```cpp
TreeNode* findMin(TreeNode* node) {
    while (node->left) node = node->left;
    return node;
}

TreeNode* deleteNode(TreeNode* root, int key) {
    if (!root) return nullptr;

    if (key < root->val) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->val) {
        root->right = deleteNode(root->right, key);
    } else {
        if (!root->left) return root->right;
        if (!root->right) return root->left;

        TreeNode* succ = findMin(root->right);
        root->val = succ->val;
        root->right = deleteNode(root->right, succ->val);
    }
    return root;
}
```

---

## 6) Validate BST

Use range constraints, not only local child checks.

```cpp
bool isValid(TreeNode* node, long long low, long long high) {
    if (!node) return true;
    if (node->val <= low || node->val >= high) return false;
    return isValid(node->left, low, node->val) &&
           isValid(node->right, node->val, high);
}

bool isValidBST(TreeNode* root) {
    return isValid(root, LLONG_MIN, LLONG_MAX);
}
```

---

## 7) Kth Smallest in BST

Since inorder is sorted, kth smallest = kth node in inorder.

```cpp
int kthSmallest(TreeNode* root, int k) {
    stack<TreeNode*> st;
    TreeNode* cur = root;

    while (cur || !st.empty()) {
        while (cur) {
            st.push(cur);
            cur = cur->left;
        }
        cur = st.top(); st.pop();
        if (--k == 0) return cur->val;
        cur = cur->right;
    }
    return -1;
}
```

---

## 8) Lowest Common Ancestor in BST

Use ordering to move one direction.

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    while (root) {
        if (p->val < root->val && q->val < root->val) root = root->left;
        else if (p->val > root->val && q->val > root->val) root = root->right;
        else return root;
    }
    return nullptr;
}
```

---

## 9) BST vs Binary Tree

| Aspect | Binary Tree | BST |
|--------|-------------|-----|
| Ordering | No order required | Strict ordering property |
| Search | `O(n)` | Avg `O(log n)` |
| Inorder result | Arbitrary | Sorted |
| Use case | General hierarchy | Ordered map/set |

---

## 10) Balanced vs Skewed BST

Balanced BST:

* height `O(log n)`
* operations fast

Skewed BST:

* height `O(n)`
* operations degrade to linear

This is why AVL / Red-Black Trees exist.

---

## 11) Common Mistakes

* validating BST using only immediate children
* forgetting duplicate handling policy
* incorrect deletion for two-children case
* not updating parent links during deletion
* assuming BST always balanced

---

## 12) Interview Questions

### Q1. Why is inorder of BST sorted?

Because left subtree values are always smaller and right subtree values larger recursively.

---

### Q2. Which node replaces deleted node with two children?

Usually inorder successor (minimum in right subtree), or predecessor.

---

### Q3. Why can BST operations become `O(n)`?

Because skewed shape increases height to `n`.

---

## 13) Practice Problems

* Search in a Binary Search Tree
* Insert into a BST
* Delete Node in a BST
* Validate Binary Search Tree
* Kth Smallest Element in BST
* Lowest Common Ancestor of BST
* Convert Sorted Array to BST
* Recover Binary Search Tree

---

## Summary

* BST adds ordering on top of binary tree

* Inorder traversal is the central property

* Search/insert/delete depend on tree height

* Balanced BST gives `O(log n)` operations

* Delete and validation are high-frequency interview topics

---
