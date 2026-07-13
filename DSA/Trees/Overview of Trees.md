# Overview of Trees

A **tree** is a hierarchical, non-linear data structure used to represent parent-child relationships.

In DSA interviews, trees appear frequently because they model recursive structure naturally and support efficient searching, ordering, and divide-and-conquer operations.

---

## Basic Terminology

* **Node**: fundamental element storing value/data
* **Root**: topmost node
* **Edge**: connection between parent and child
* **Parent / Child**: direct relationship between nodes
* **Leaf node**: node with no children
* **Subtree**: tree formed by a node and all its descendants
* **Height**: number of edges on longest path from node to leaf
* **Depth**: number of edges from root to a node
* **Level**: depth + 1 (in most conventions)

---

## Why Trees Matter in Placements

Tree concepts are the foundation for:

* Binary Search Trees (BST)
* Heaps / Priority Queues
* Segment Trees / Fenwick Trees
* Tries
* Recursion-based interview problems

Many advanced DSA topics are specialized tree variants.

---

## Properties of a Tree

For a tree with `N` nodes:

* Number of edges is always `N - 1`
* There is exactly one simple path between any two nodes
* Tree is connected and acyclic

These properties are often used for quick reasoning in problems.

---

## Types of Trees (High-Level)

1. **General Tree**: any number of children
2. **Binary Tree**: at most two children per node
3. **Binary Search Tree (BST)**: ordered binary tree
4. **Balanced Trees**: height kept near `log n` (AVL, Red-Black)
5. **Heap**: complete binary tree with heap property
6. **Trie**: prefix tree for strings

---

## Binary Tree Traversals

The three depth-first traversals are:

* **Preorder**: Root, Left, Right
* **Inorder**: Left, Root, Right
* **Postorder**: Left, Right, Root

Level-order traversal uses BFS (queue).

### Recursive DFS Template

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

---

## Level Order (BFS) Template

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
			TreeNode* cur = q.front();
			q.pop();
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

## Complexity Basics

For most tree traversals:

* Time complexity: `O(n)` (visit each node once)
* Space complexity:
  * DFS recursion: `O(h)`
  * BFS queue: `O(w)` where `w` is max width

Here `h` is tree height.

---

## Common Interview Pitfalls

* Forgetting null checks

* Mixing edge-based and node-based definitions of height

* Writing `O(n^2)` solution when one-pass `O(n)` exists

* Not considering skewed trees (worst-case height `n`)

---

## What to Practice Next

1. Tree traversals (recursive + iterative)

2. Height, size, leaf count

3. Balanced tree, diameter, max path sum

4. LCA, path sum, tree views

5. BST insert/search/delete

---
