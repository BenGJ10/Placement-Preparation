# Properties of Binary Trees

A **binary tree** is a tree in which each node has at most two children: left and right.

This topic is very important in placements because many problems rely on binary tree structural properties and formulas.

---

## Core Definitions

* **Root**: top node
* **Leaf**: node with no children
* **Internal node**: node with at least one child
* **Height of tree**: longest path from root to leaf
* **Level**: nodes at same distance from root

---

## Fundamental Properties

## 1) Maximum nodes at level `l`

Maximum number of nodes at level `l` is:

`2^l` (if level starts from `0` at root)

---

## 2) Maximum nodes in binary tree of height `h`

If height is counted in edges:

`maxNodes = 2^(h+1) - 1`

This is a **perfect binary tree**.

---

## 3) Minimum height for `n` nodes

Minimum possible height (best balanced case):

`h_min = ceil(log2(n + 1)) - 1`

---

## 4) Edges relation

For any tree with `n` nodes:

`edges = n - 1`

This always holds for binary trees too.

---

## 5) Leaves and internal nodes (strict/full binary tree)

In a strict (full) binary tree, every internal node has exactly 2 children.

If `I` = number of internal nodes, and `L` = number of leaves:

`L = I + 1`

---

## Types of Binary Trees

## 1) Full (Strict) Binary Tree

Every node has either 0 or 2 children.

## 2) Complete Binary Tree

All levels are full except possibly last, and last level is filled from left to right.

## 3) Perfect Binary Tree

All internal nodes have 2 children and all leaves are at same level.

## 4) Balanced Binary Tree

Height is approximately `O(log n)`.

## 5) Degenerate (Skewed) Binary Tree

Every node has only one child, behaving like linked list.

---

## Traversal Properties

For a tree with `n` nodes:

* Each DFS traversal visits exactly `n` nodes
* Total traversal complexity is usually `O(n)`

Important interpretation:

* Inorder of BST gives sorted sequence
* Postorder often helps when answer depends on children first
* Preorder useful for tree construction/serialization

---

## Height and Complexity Insights

Let `h` be height:

* Search in balanced BST: `O(log n)`
* Search in skewed BST: `O(n)`

So balancing directly affects performance.

---

## Useful Code Snippets

### Count nodes

```cpp
int countNodes(TreeNode* root) {
	if (!root) return 0;
	return 1 + countNodes(root->left) + countNodes(root->right);
}
```

### Height of binary tree

```cpp
int height(TreeNode* root) {
	if (!root) return -1; // edge-based height
	return 1 + max(height(root->left), height(root->right));
}
```

---

## Common Interview Mistakes

* Mixing level-numbering conventions (`0` vs `1` based)

* Confusing full, complete, and perfect trees

* Writing formulas without defining height convention

* Assuming all binary trees are balanced

---

## Quick Formula Sheet

* Max nodes at level `l`: `2^l`

* Max nodes for height `h`: `2^(h+1) - 1`

* Min height for `n` nodes: `ceil(log2(n+1)) - 1`

* Edges in any tree with `n` nodes: `n - 1`

* In strict binary tree: `L = I + 1`

---
