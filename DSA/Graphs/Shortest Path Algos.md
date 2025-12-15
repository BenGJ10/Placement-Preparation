# Shortest Path Algorithms

## What Is a Shortest Path?

In a graph, a **shortest path** between two nodes is the path whose **total cost is minimum**, where cost may represent:

* Number of edges
* Sum of edge weights
* Time, distance, or resource usage

Different shortest path algorithms exist because **graph constraints differ**:

* Weighted vs unweighted
* Negative weights or not
* Single source vs all pairs

Choosing the correct algorithm is a **key interview skill**.

---

## Classification of Shortest Path Problems

| Problem Type                         | Algorithm      |
| ------------------------------------ | -------------- |
| Unweighted graph                     | BFS            |
| Weights = 0 or 1                     | 0-1 BFS        |
| Positive weights                     | Dijkstra       |
| Negative weights (no negative cycle) | Bellman-Ford   |
| All-pairs shortest path              | Floyd-Warshall |

---

## 1. Shortest Path in Unweighted Graph (BFS)

### When to Use

* All edges have equal weight (implicitly 1)
* Minimum number of steps / edges required

### Why BFS Works

BFS explores nodes **level by level**, ensuring the first time a node is reached, it is via the shortest path.

### C++ Implementation

```cpp
vector<int> shortestPathBFS(int n, vector<vector<int>>& adj, int src) {
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

### Complexity

* Time: **O(V + E)**
* Space: **O(V)**

---

## 2. 0-1 BFS

### When to Use

* Edge weights are **only 0 or 1**
* Faster than Dijkstra in this specific case

### Key Idea

Use a **deque** instead of a queue:

* Weight 0 → push to front
* Weight 1 → push to back

### C++ Implementation

```cpp
vector<int> zeroOneBFS(int n, vector<vector<pair<int,int>>>& adj, int src) {
    deque<int> dq;
    vector<int> dist(n, INT_MAX);

    dist[src] = 0;
    dq.push_front(src);

    while (!dq.empty()) {
        int node = dq.front();
        dq.pop_front();

        for (auto [nei, wt] : adj[node]) {
            if (dist[node] + wt < dist[nei]) {
                dist[nei] = dist[node] + wt;
                if (wt == 0)
                    dq.push_front(nei);
                else
                    dq.push_back(nei);
            }
        }
    }
    return dist;
}
```

### Complexity

* Time: **O(V + E)**
* Space: **O(V)**

---

## 3. Dijkstra’s Algorithm

### When to Use

* **Non-negative edge weights**
* Single-source shortest path

### Core Idea

Always expand the node with the **currently known minimum distance** using a min-heap.

### Important Limitation

Dijkstra **fails with negative edge weights**.

### C++ Implementation (Using Min Heap)

```cpp
vector<int> dijkstra(int n, vector<vector<pair<int,int>>>& adj, int src) {
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    vector<int> dist(n, INT_MAX);

    dist[src] = 0;
    pq.push({0, src});

    while (!pq.empty()) {
        auto [d, node] = pq.top();
        pq.pop();

        if (d > dist[node]) continue;

        for (auto [nei, wt] : adj[node]) {
            if (dist[node] + wt < dist[nei]) {
                dist[nei] = dist[node] + wt;
                pq.push({dist[nei], nei});
            }
        }
    }
    return dist;
}
```

### Complexity

* Time: **O(E log V)**
* Space: **O(V)**

---

## 4. Bellman-Ford Algorithm

### When to Use

* Graph contains **negative edge weights**
* Need to **detect negative cycles**

### Key Idea

Relax all edges **V-1 times**.
If distance reduces on the Vth iteration → negative cycle exists.

### C++ Implementation

```cpp
vector<int> bellmanFord(int n, vector<vector<int>>& edges, int src) {
    vector<int> dist(n, 1e9);
    dist[src] = 0;

    for (int i = 0; i < n - 1; i++) {
        for (auto& e : edges) {
            int u = e[0], v = e[1], wt = e[2];
            if (dist[u] != 1e9 && dist[u] + wt < dist[v]) {
                dist[v] = dist[u] + wt;
            }
        }
    }

    // Negative cycle detection
    for (auto& e : edges) {
        if (dist[e[0]] + e[2] < dist[e[1]]) {
            cout << "Negative cycle detected\n";
        }
    }
    return dist;
}
```

### Complexity

* Time: **O(V × E)**
* Space: **O(V)**

---

## 5. Floyd-Warshall Algorithm

### When to Use

* **All-pairs shortest paths**
* Graph size is small (≤ 500)

### Core Idea

Try all nodes as intermediate points.

### C++ Implementation

```cpp
void floydWarshall(vector<vector<int>>& dist) {
    int n = dist.size();

    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (dist[i][k] < 1e9 && dist[k][j] < 1e9)
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
}
```

### Complexity

* Time: **O(V³)**
* Space: **O(V²)**

---

## Algorithm Selection Guide ⭐️

| Graph Condition          | Algorithm      |
| ------------------------ | -------------- |
| Unweighted               | BFS            |
| Weights = 0 or 1         | 0-1 BFS        |
| Positive weights         | Dijkstra       |
| Negative weights         | Bellman-Ford   |
| Negative cycle detection | Bellman-Ford   |
| All-pairs shortest path  | Floyd-Warshall |

---

## Common Interview Pitfalls

* Using BFS for weighted graphs

* Using Dijkstra when negative weights exist

* Forgetting priority queue optimization

* Missing negative cycle detection

* Using Floyd-Warshall on large graphs

---

## Interview Questions

### Q1. Why does Dijkstra fail with negative weights?

**Answer:**
Because it assumes that once a node’s shortest distance is finalized, it will never improve. Negative edges violate this assumption.

---

### Q2. When should Bellman-Ford be preferred?

**Answer:**
When negative weights are present or when negative cycle detection is required.

---

### Q3. Why is BFS considered a shortest path algorithm?

**Answer:**
Because in unweighted graphs, distance equals number of edges, and BFS explores nodes in increasing edge count order.

---

### Q4. Why is Floyd-Warshall rarely used in interviews?

**Answer:**
Due to its **O(V³)** complexity and high memory usage, making it impractical for large graphs.

---

### Q5. Difference between Dijkstra and 0-1 BFS?

**Answer:**
0-1 BFS is optimized for graphs with weights {0,1} and runs in linear time without a heap.

---

## LeetCode Problems to Practice (Shortest Path)

1. [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/)
2. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)
3. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)
4. [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)
5. [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)
6. [Bellman Ford – Negative Cycle](https://leetcode.com/problems/cheapest-flights-within-k-stops/)
7. [Floyd Warshall Application – Find the City](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)
8. [Word Ladder](https://leetcode.com/problems/word-ladder/)
9. [Minimum Cost to Make at Least One Valid Path](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/)
10. [Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

These cover **BFS, Dijkstra, Bellman-Ford concepts, and state-based shortest path problems**.

---

## Summary

* Shortest path problems vary by **graph constraints**

* No single algorithm fits all cases

* Correct algorithm selection is **frequently tested**

* BFS, Dijkstra, and Bellman-Ford form the core foundation

* Mastery of these algorithms is essential for placements

---
