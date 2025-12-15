# Depth-First Search (DFS)

## Definition

**Depth-First Search (DFS)** is a traversal algorithm that explores a graph or tree by going **as deep as possible along a path** before backtracking.

Starting from a source node, DFS explores one neighbor completely (including all of its descendants) before moving to the next neighbor. This behavior makes DFS ideal for problems that require **complete exploration, recursion, or decision trees**.

---

## Why DFS Is Important for Placements

DFS is a **core problem-solving pattern**, not just a traversal technique.

DFS is commonly used when:

* The problem requires **exploring all possibilities**
* Backtracking is involved
* Cycles need to be detected
* Connectivity or components are analyzed
* Hierarchical or recursive structures are present

Interviewers expect candidates to **recognize DFS naturally**, especially in recursive problem statements.

---

## DFS vs BFS (Conceptual Comparison)

| Aspect          | DFS                               | BFS              |
| --------------- | --------------------------------- | ---------------- |
| Traversal Style | Depth first                       | Level by level   |
| Data Structure  | Stack / Recursion                 | Queue            |
| Shortest Path   | No                                | Yes (unweighted) |
| Memory Usage    | Lower                             | Higher           |
| Typical Use     | Exploration, cycles, backtracking | Distance, layers |

DFS is preferred when **depth matters more than distance**.

---

## Graph Representation Used with DFS

DFS works with both **directed and undirected graphs**.

The most common representation is the **adjacency list**, which supports efficient traversal.

```cpp
vector<vector<int>> adj(n);
```

DFS can also be applied directly to **trees and grids**.

---

## DFS Algorithm (Core Idea)

1. Start from a node
2. Mark it as visited
3. Recursively visit all unvisited neighbors
4. Backtrack when no unvisited neighbors remain

DFS forms a **recursive call stack** that represents the current exploration path.

---

## DFS Pseudocode

```
DFS(node):
    mark node as visited

    for each neighbor of node:
        if neighbor not visited:
            DFS(neighbor)
```

---

## DFS Implementation in C++ (Recursive)

### DFS on a Graph

```cpp
#include <bits/stdc++.h>
using namespace std;

void dfs(int node, vector<vector<int>>& adj, vector<bool>& visited) {
    visited[node] = true;
    cout << node << " ";

    for (int neighbor : adj[node]) {
        if (!visited[neighbor]) {
            dfs(neighbor, adj, visited);
        }
    }
}
```

This implementation ensures:

* Each node is visited once
* Natural backtracking via recursion
* Clean and readable code

---

## DFS Using Stack (Iterative)

DFS can also be implemented without recursion using an explicit stack.

```cpp
void dfsIterative(int start, vector<vector<int>>& adj, int n) {
    vector<bool> visited(n, false);
    stack<int> st;

    st.push(start);

    while (!st.empty()) {
        int node = st.top();
        st.pop();

        if (visited[node]) continue;

        visited[node] = true;
        cout << node << " ";

        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                st.push(neighbor);
            }
        }
    }
}
```

Iterative DFS is useful when recursion depth may exceed system limits.

---

## DFS on Trees

DFS naturally fits trees due to their hierarchical structure.

Tree traversals are DFS variants:

* Preorder
* Inorder
* Postorder

```cpp
void preorder(TreeNode* root) {
    if (!root) return;
    cout << root->val << " ";
    preorder(root->left);
    preorder(root->right);
}
```

---

## DFS on Grids (Matrix DFS)

Grid DFS treats each cell as a node.

Movement is handled using direction arrays.

```cpp
int dx[4] = {1, -1, 0, 0};
int dy[4] = {0, 0, 1, -1};

void dfsGrid(int x, int y, vector<vector<int>>& grid, vector<vector<bool>>& visited) {
    visited[x][y] = true;

    for (int d = 0; d < 4; d++) {
        int nx = x + dx[d];
        int ny = y + dy[d];

        if (nx >= 0 && ny >= 0 && nx < grid.size() &&
            ny < grid[0].size() && !visited[nx][ny]) {
            dfsGrid(nx, ny, grid, visited);
        }
    }
}
```

This pattern is essential for island and component problems.

---

## Time and Space Complexity

### Time Complexity

* Graph DFS: **O(V + E)**
* Grid DFS: **O(N × M)**

### Space Complexity

* Visited array: **O(V)**
* Recursion stack: **O(V)** (worst case)

Deep recursion may cause stack overflow in skewed graphs or trees.

---

## Cycle Detection Using DFS

### Undirected Graph

A cycle exists if a visited node is encountered **that is not the parent**.

### Directed Graph

A cycle exists if a node is visited that is already in the **current recursion stack**.

This distinction is commonly tested in interviews.

---

## DFS for Connected Components

DFS can be used to count or label connected components.

```cpp
int components = 0;
for (int i = 0; i < n; i++) {
    if (!visited[i]) {
        dfs(i, adj, visited);
        components++;
    }
}
```

---

## DFS in Backtracking Problems

DFS forms the base for backtracking problems such as:

* Permutations
* Combinations
* Sudoku solver
* N-Queens

In these problems, DFS explores a choice, then **backtracks** to try alternatives.

---

## Common DFS Pitfalls in Interviews

* Forgetting to mark visited before recursion
* Stack overflow due to deep recursion
* Using DFS when shortest path is required
* Missing base conditions in recursive DFS
* Revisiting nodes in cyclic graphs

---

## DFS Interview Questions (With Answers)

### Q1. When should DFS be preferred over BFS?

**Answer:**
DFS is preferred when the problem requires deep exploration, backtracking, or checking connectivity rather than shortest distance.

---

### Q2. Why does DFS not guarantee the shortest path?

**Answer:**
DFS explores depth first and may take a long path before discovering a shorter one. It does not explore paths in increasing order of length.

---

### Q3. What causes stack overflow in DFS?

**Answer:**
Deep or skewed recursion, especially in graphs with long paths or trees with large height.

---

### Q4. How is cycle detection different in directed and undirected graphs?

**Answer:**
Undirected graphs use parent tracking, while directed graphs use a recursion stack or color marking.

---

### Q5. Can DFS be implemented without recursion?

**Answer:**
Yes. DFS can be implemented iteratively using an explicit stack.

---

## Important DFS Variants to Master

* Recursive DFS

* Iterative DFS

* DFS with backtracking

* DFS for cycle detection

* DFS for connected components

* DFS with state tracking

These patterns appear repeatedly in medium and hard interview problems.

---

## LeetCode Problems to Practice (DFS)

1. [Number of Islands](https://leetcode.com/problems/number-of-islands/)
2. [Flood Fill](https://leetcode.com/problems/flood-fill/)
3. [Clone Graph](https://leetcode.com/problems/clone-graph/)
4. [Max Area of Island](https://leetcode.com/problems/max-area-of-island/)
5. [Course Schedule](https://leetcode.com/problems/course-schedule/)
6. [Path Sum](https://leetcode.com/problems/path-sum/)
7. [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)
8. [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)
9. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)
10. [Evaluate Division](https://leetcode.com/problems/evaluate-division/)

These problems cover **graph DFS, grid DFS, tree DFS, and DFS with recursion stack logic**.

---

## Summary

* DFS explores **deep before wide**

* Uses **recursion or stack**

* Ideal for **exploration, backtracking, and structure analysis**

* Not suitable for shortest path problems

* A core foundation for many advanced algorithms

---
