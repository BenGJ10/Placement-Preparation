# Minimum Spanning Tree (MST)

A **Minimum Spanning Tree** is a subset of edges in a connected, undirected, weighted graph that:

* connects all vertices
* contains exactly `V - 1` edges
* has minimum possible total edge weight
* contains no cycles

---

## Why MST Is Important

MST appears in placement interviews in forms like:

* minimum cost to connect all cities/computers
* network design with minimum cable cost
* reducing redundant expensive links

It is a classic greedy topic.

---

## MST Algorithms

Two standard algorithms are expected in interviews:

1. Kruskal's Algorithm (edge-based, DSU)
2. Prim's Algorithm (vertex-based, priority queue)

Both give the same minimum total cost for connected graphs.

---

## 1) Kruskal's Algorithm

### Idea

Sort edges by weight. Add the smallest edge that does not create a cycle.

Cycle detection is done using **Disjoint Set Union (Union-Find)**.

### C++ Implementation

```cpp
#include <bits/stdc++.h>
using namespace std;

struct DSU {
    vector<int> p, sz;
    DSU(int n) {
        p.resize(n);
        sz.assign(n, 1);
        iota(p.begin(), p.end(), 0);
    }
    int find(int x) {
        if (p[x] == x) return x;
        return p[x] = find(p[x]);
    }
    bool unite(int a, int b) {
        a = find(a), b = find(b);
        if (a == b) return false;
        if (sz[a] < sz[b]) swap(a, b);
        p[b] = a;
        sz[a] += sz[b];
        return true;
    }
};

long long kruskalMST(int n, vector<array<int,3>>& edges) {
    sort(edges.begin(), edges.end(), [](auto &a, auto &b) {
        return a[2] < b[2];
    });

    DSU dsu(n);
    long long cost = 0;
    int used = 0;

    for (auto &e : edges) {
        int u = e[0], v = e[1], w = e[2];
        if (dsu.unite(u, v)) {
            cost += w;
            used++;
            if (used == n - 1) break;
        }
    }

    if (used != n - 1) return -1; // graph disconnected
    return cost;
}
```

---

## 2) Prim's Algorithm

### Idea

Start from one node. Repeatedly pick the minimum-weight edge that connects a visited node to an unvisited node.

### C++ Implementation

```cpp
long long primMST(int n, vector<vector<pair<int,int>>>& adj) {
    vector<int> vis(n, 0);
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;

    pq.push({0, 0}); // {weight, node}
    long long cost = 0;
    int taken = 0;

    while (!pq.empty()) {
        auto [w, u] = pq.top();
        pq.pop();

        if (vis[u]) continue;
        vis[u] = 1;
        cost += w;
        taken++;

        for (auto &it : adj[u]) {
            int v = it.first;
            int wt = it.second;
            if (!vis[v]) pq.push({wt, v});
        }
    }

    if (taken != n) return -1; // disconnected
    return cost;
}
```

---

## Kruskal vs Prim

| Aspect            | Kruskal                        | Prim                          |
| ----------------- | ------------------------------ | ----------------------------- |
| Main approach     | Sort edges + DSU              | Grow tree from one node       |
| Data structure    | DSU                            | Min-heap + visited            |
| Better for        | Sparse graph, edge list input | Dense graph, adjacency input  |
| Complexity        | `O(E log E)`                  | `O((V+E) log V)`              |

---

## Common Interview Pitfalls

* Forgetting graph is undirected

* Not handling disconnected graph

* DSU without path compression / union by size

* Not skipping already visited node in Prim

* Using `int` for total MST cost when sum can overflow

---

## Related Problems

* Connect all points with minimum cost

* Minimum cost to connect cities

* Redundant connection (DSU idea)

* Number of operations to connect network

---
