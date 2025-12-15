# Breadth-First Search (BFS)

## Definition

**Breadth-First Search (BFS)** is a traversal algorithm that explores a graph or tree **level by level**, starting from a source node and visiting all nodes at the current distance before moving to nodes at the next distance.

The defining property of BFS is that it guarantees the **minimum number of edges** from the source to any reachable node in an **unweighted graph**, making it the default choice for shortest-path problems where all edges have equal cost.

---

## Why BFS Is Important for Placements

BFS is not just a traversal algorithm; it represents a **problem-solving pattern** frequently used in interviews.

BFS is commonly required when:

* The problem asks for **minimum steps**, **minimum moves**, or **shortest distance**
* Data is structured in **levels, layers, or waves**
* Multiple sources expand simultaneously
* Graph or grid problems require controlled exploration

Interviewers expect candidates to **identify BFS naturally**, not force it.

---

## BFS vs DFS (Conceptual Comparison)

| Aspect          | BFS              | DFS                 |
| --------------- | ---------------- | ------------------- |
| Traversal Style | Level by level   | Depth first         |
| Data Structure  | Queue            | Stack / Recursion   |
| Shortest Path   | Yes (unweighted) | No                  |
| Memory Usage    | Higher           | Lower               |
| Typical Use     | Distance, layers | Exploration, cycles |

A common interview mistake is using DFS in problems that explicitly require the **shortest path**.

---

## Graph Representation Used with BFS

BFS works on both **directed and undirected graphs**.

The most interview-friendly representation is the **adjacency list**.

```cpp
vector<vector<int>> adj(n);
```

This representation ensures:

* Efficient traversal
* Lower memory usage
* Clean BFS implementation

---

## BFS Algorithm (Step-by-Step)

1. Initialize a queue
2. Mark the source node as visited
3. Push the source into the queue
4. While the queue is not empty:

   * Pop the front node
   * Visit all its unvisited neighbors
   * Mark them visited
   * Push them into the queue

The key invariant:
**Nodes enter the queue in increasing order of distance from the source.**

---

## BFS Pseudocode

```
BFS(source):
    queue q
    visited[source] = true
    q.push(source)

    while q is not empty:
        node = q.front()
        q.pop()

        for each neighbor of node:
            if neighbor not visited:
                visited[neighbor] = true
                q.push(neighbor)
```

---

## BFS Implementation in C++

### BFS on a Graph (Adjacency List)

```cpp
#include <bits/stdc++.h>
using namespace std;

void bfs(int start, vector<vector<int>>& adj, int n) {
    vector<bool> visited(n, false);
    queue<int> q;

    visited[start] = true;
    q.push(start);

    while (!q.empty()) {
        int node = q.front();
        q.pop();

        cout << node << " ";

        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}
```

This implementation ensures each node is visited **exactly once**, giving optimal time complexity.

---

### BFS for Shortest Path in Unweighted Graphs

The shortest path in an unweighted graph can be computed using BFS by maintaining a distance array.

The first time a node is reached, the distance stored is guaranteed to be minimal.

```cpp
vector<int> bfsShortestPath(int n, vector<vector<int>>& adj, int src) {
    vector<int> dist(n, -1);
    queue<int> q;

    dist[src] = 0;
    q.push(src);

    while (!q.empty()) {
        int node = q.front();
        q.pop();

        for (int nei : adj[node]) {
            if (dist[nei] == -1) {
                dist[nei] = dist[node] + 1;
                q.push(nei);
            }
        }
    }
    return dist;
}
```

---

## Multi-Source BFS

**Multi-source BFS** is used when multiple starting points expand simultaneously.

Instead of pushing one source, **all sources are pushed into the queue initially**.

This technique models problems where influence spreads from several points at the same time.

### Common Use Cases

* Rotten Oranges
* Nearest Zero in Matrix
* Fire or Infection Spread

---

## BFS on Grids and Matrices

Grid-based BFS treats each cell as a node.

Movement is typically controlled using direction arrays.

```cpp
int dx[4] = {1, -1, 0, 0};
int dy[4] = {0, 0, 1, -1};
```

This pattern is essential for almost all matrix BFS problems in interviews.

---

## Time and Space Complexity

### Time Complexity

* Graph BFS: **O(V + E)**
* Grid BFS: **O(N × M)**

### Space Complexity

* Queue + visited array: **O(V)**

Memory usage is higher than DFS because BFS stores all nodes of a level simultaneously.

---

## Common BFS Pitfalls in Interviews

* Marking nodes as visited **after popping** instead of before pushing
* Forgetting to handle **disconnected components**
* Using BFS for **weighted graphs**
* Incorrect boundary checks in grid problems
* Missing multi-source BFS opportunities

---

## BFS Interview Questions

### Q1. Why does BFS guarantee the shortest path in unweighted graphs?

**Answer:**
BFS explores nodes in increasing order of edge count. Since all edges have equal weight, the first time a node is visited corresponds to the shortest possible path.

---

### Q2. Why can BFS not be used for weighted graphs?

**Answer:**
BFS assumes every edge has equal cost. When edge weights differ, BFS may choose a path with fewer edges but higher total cost. Dijkstra’s algorithm is required instead.

---

### Q3. Is level-order traversal the same as BFS?

**Answer:**
Yes. Level-order traversal of a tree is BFS applied to a tree structure.

---

### Q4. How can cycles be detected using BFS?

**Answer:**

* Undirected graph: track parent node
* Directed graph: use indegree and Kahn’s algorithm

---

### Q5. When is BFS a poor choice?

**Answer:**

* Weighted graphs
* Memory-constrained environments
* Problems requiring deep backtracking

---

## Important BFS Variants to Master

* BFS with distance tracking

* Multi-source BFS

* BFS with state (node, steps)

* 0-1 BFS (deque based)

* BFS for bipartite checking

These variants frequently appear in medium and hard interview problems.

---

## LeetCode Problems to Practice (BFS)

1. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
2. [Number of Islands](https://leetcode.com/problems/number-of-islands/)
3. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)
4. [Flood Fill](https://leetcode.com/problems/flood-fill/)
5. [01 Matrix](https://leetcode.com/problems/01-matrix/)
6. [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/)
7. [Word Ladder](https://leetcode.com/problems/word-ladder/)
8. [Walls and Gates](https://leetcode.com/problems/walls-and-gates/)
9. [Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)
10. [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)

These problems collectively cover **basic BFS, grid BFS, multi-source BFS, and BFS with constraints**.

---

## Summary

* BFS is a **level-based traversal algorithm**

* It is the **default choice for shortest paths in unweighted graphs**

* Uses a **queue** to ensure controlled expansion

* Appears frequently in **graphs, trees, grids, and real-world modeling problems**

* Mastery of BFS is mandatory for **placements and system-level thinking**

---
