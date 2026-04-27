# How to Identify and Solve Graph Problems

Graph problems look different on the surface, but most of them map to a small set of standard algorithms.

This guide helps you quickly identify the correct approach in interviews.

---

## Step 1: Detect Whether It Is a Graph Problem

Even if the word "graph" is not present, treat it as graph when you see:

* entities + relationships/connections
* cities/roads, courses/prerequisites, users/followers
* transformations (word ladder style)
* matrix movement (grid can be treated as graph)
* "minimum steps", "reachability", "dependency order"

---

## Step 2: Ask These 6 Questions

1. Directed or undirected?

2. Weighted or unweighted?

3. Need traversal only or shortest path?

4. Need cycle detection?

5. Need ordering based on dependencies?

6. Need minimum connection cost?

Your answers map directly to an algorithm.

---

## Algorithm Selection Map

* Traversal / connected components -> DFS or BFS

* Shortest path (unweighted) -> BFS

* Shortest path (weighted, non-negative) -> Dijkstra

* Shortest path with negative edges -> Bellman-Ford

* All-pairs shortest path -> Floyd-Warshall

* Dependency ordering (DAG) -> Topological Sort

* Minimum connection cost -> MST (Kruskal/Prim)

* Cycle detection:
  * Undirected -> DFS parent check or DSU
  * Directed -> DFS recursion stack / Kahn's indegree

---

## Step 3: Choose Representation

### Adjacency List (default for interviews)

```cpp
vector<vector<int>> adj(n);
```

Weighted:

```cpp
vector<vector<pair<int,int>>> adj(n); // {neighbor, weight}
```

Use adjacency matrix only for very dense graph or Floyd-Warshall style problems.

---

## Step 4: Use Standard Templates

## DFS Template

```cpp
void dfs(int u, vector<vector<int>>& adj, vector<int>& vis) {
    vis[u] = 1;
    for (int v : adj[u]) {
        if (!vis[v]) dfs(v, adj, vis);
    }
}
```

## BFS Template

```cpp
void bfs(int src, vector<vector<int>>& adj, vector<int>& vis) {
    queue<int> q;
    q.push(src);
    vis[src] = 1;

    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int v : adj[u]) {
            if (!vis[v]) {
                vis[v] = 1;
                q.push(v);
            }
        }
    }
}
```

---

## Step 5: Handle Disconnected Graphs

Many candidates miss this. Always run traversal from every unvisited node.

```cpp
for (int i = 0; i < n; i++) {
    if (!vis[i]) {
        dfs(i, adj, vis); // or bfs(i, ...)
    }
}
```

---

## Step 6: Complexity Statement

Always mention complexity based on adjacency list:

* DFS/BFS: `O(V + E)`

* Dijkstra (heap): `O((V + E) log V)`

* Topological sort: `O(V + E)`

* Kruskal: `O(E log E)`

* Floyd-Warshall: `O(V^3)`

---

## Quick Pattern Recognition

* "Can we reach X from Y?" -> DFS/BFS

* "Minimum number of moves/edges" -> BFS

* "Prerequisites ordering" -> Topological sort

* "Cheapest route" + non-negative weights -> Dijkstra

* "Connect all nodes with minimum total cost" -> MST

* "Is there a cycle?" -> cycle detection pattern

---

## Common Interview Mistakes

* Applying BFS for weighted graph shortest path

* Forgetting direction while building edges

* Missing disconnected components

* Wrong indegree updates in Kahn's algorithm

* Not using long long when path sums are large

---

## Grid as Graph Trick

For matrix problems, treat each cell as a node and neighbors via direction arrays:

```cpp
int dx[4] = {1, -1, 0, 0};
int dy[4] = {0, 0, 1, -1};
```

Then apply BFS/DFS directly.

---