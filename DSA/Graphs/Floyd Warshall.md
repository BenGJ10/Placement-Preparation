# Floyd-Warshall Algorithm

Floyd-Warshall is an all-pairs shortest path algorithm.

It computes shortest distances between every pair of vertices in a weighted graph, and it supports negative edge weights (but no negative cycle).

---

## When to Use

Use Floyd-Warshall when:

* graph is dense
* number of vertices is small to medium (`n` up to around 400 in interviews/CP)
* shortest paths are needed for many source-destination pairs
* transitive closure or reachability matrix style problems appear

If only one source shortest path is needed, prefer Dijkstra or Bellman-Ford.

---

## Core Idea (Dynamic Programming)

Let `dist[i][j]` represent the shortest distance from `i` to `j` considering intermediate vertices from a set.

Transition:

`dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`

Interpretation:

For each vertex `k`, decide whether going through `k` improves the path from `i` to `j`.

---

## Initialization Rules

* `dist[i][i] = 0`
* For edge `u -> v` with weight `w`, `dist[u][v] = min(dist[u][v], w)`
* Missing edges initialized as infinity (`INF`)

---

## C++ Implementation

```cpp

const long long INF = (long long)4e18;

vector<vector<long long>> floydWarshall(int n, vector<tuple<int,int,int>>& edges) {
    vector<vector<long long>> dist(n, vector<long long>(n, INF));

    for (int i = 0; i < n; i++) dist[i][i] = 0;

    for (auto &e : edges) {
        int u, v, w;
        tie(u, v, w) = e;
        dist[u][v] = min(dist[u][v], (long long)w);
    }

    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            if (dist[i][k] == INF) continue;
            for (int j = 0; j < n; j++) {
                if (dist[k][j] == INF) continue;
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }

    return dist;
}
```

---

## Detecting Negative Cycles

After running Floyd-Warshall:

* if any `dist[i][i] < 0`, a negative cycle exists and shortest paths are undefined for affected nodes.

```cpp
bool hasNegativeCycle(const vector<vector<long long>>& dist) {
    int n = dist.size();
    for (int i = 0; i < n; i++) {
        if (dist[i][i] < 0) return true;
    }
    return false;
}
```

---

## Complexity

* Time: `O(n^3)`
* Space: `O(n^2)`

This is why it is practical only for limited `n`.

---

## Floyd-Warshall vs Other Shortest Path Algorithms

| Algorithm      | Handles Negative Weights | All Pairs | Time Complexity |
| -------------- | ------------------------ | --------- | --------------- |
| Dijkstra       | No                       | No        | `O((V+E)logV)`  |
| Bellman-Ford   | Yes                      | No        | `O(VE)`         |
| Floyd-Warshall | Yes                      | Yes       | `O(V^3)`        |

---

## Interview Pitfalls

* Using `int` and causing overflow during `dist[i][k] + dist[k][j]`

* Forgetting to skip INF states before addition

* Assuming it works with negative cycles

* Wrong indexing (0-based vs 1-based)

---

## Typical Questions

* Find shortest path between every pair of cities

* Detect negative cycle in directed weighted graph

* Find the city with smallest number of neighbors within threshold distance

* Compute transitive closure (reachability)

---

## Transitive Closure Variant

If you only need reachability (not weighted distances), use boolean matrix and update:

`reach[i][j] = reach[i][j] OR (reach[i][k] AND reach[k][j])`

This is the same structure as Floyd-Warshall.

---