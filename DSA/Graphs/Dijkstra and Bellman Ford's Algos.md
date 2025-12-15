# Dijkstra’s Algorithm and Bellman–Ford Algorithm

## Problem Context

Both **Dijkstra** and **Bellman–Ford** solve the **single-source shortest path problem**, meaning:

> Given a graph and a source node, find the minimum distance from the source to every other node.

The key difference lies in **edge weight constraints** and **guarantees provided by each algorithm**.

---

## Dijkstra’s Algorithm

### Definition

**Dijkstra’s Algorithm** computes the shortest path from a single source node to all other nodes in a graph **with non-negative edge weights**.

It works by **greedily selecting the node with the smallest known distance** and relaxing its outgoing edges.

---

## When to Use Dijkstra

Use Dijkstra when:

* All edge weights are **non-negative**
* You need **fast shortest path computation**
* The graph is large and performance matters

Do **not** use Dijkstra if negative weights exist.

---

## Core Intuition

Dijkstra is based on a greedy invariant:

> Once the shortest distance to a node is finalized, it will never improve.

This invariant holds **only when edge weights are non-negative**.

---

## Algorithm Steps

1. Initialize distance array with infinity
2. Set source distance = 0
3. Use a **min-priority queue**
4. Repeatedly:

   * Pick the node with minimum distance
   * Relax all adjacent edges
   * Update distances if a shorter path is found

---

## Edge Relaxation Concept

For an edge `u → v` with weight `w`:

```
if dist[u] + w < dist[v]:
    dist[v] = dist[u] + w
```

This is the core operation of all shortest path algorithms.

---

## C++ Implementation (Using Min Heap)

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> dijkstra(int n, vector<vector<pair<int,int>>>& adj, int src) {
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    vector<int> dist(n, INT_MAX);

    dist[src] = 0;
    pq.push({0, src});

    while (!pq.empty()) {
        auto [currDist, node] = pq.top();
        pq.pop();

        if (currDist > dist[node]) continue;

        for (auto [neighbor, weight] : adj[node]) {
            if (dist[node] + weight < dist[neighbor]) {
                dist[neighbor] = dist[node] + weight;
                pq.push({dist[neighbor], neighbor});
            }
        }
    }
    return dist;
}
```

---

## Time and Space Complexity

* **Time:** `O(E log V)`
* **Space:** `O(V)`

Using a priority queue ensures optimal performance for large graphs.

---

## Why Dijkstra Fails with Negative Weights

Dijkstra assumes that once a node is popped from the priority queue, its shortest distance is finalized.

Negative weights can later produce a **shorter path**, violating this assumption.

---

## Common Interview Pitfalls (Dijkstra)

* Using it with negative weights

* Forgetting the `if (currDist > dist[node]) continue;` optimization

* Using adjacency matrix instead of adjacency list

* Confusing BFS with Dijkstra

---

## Bellman–Ford Algorithm

### Definition

**Bellman–Ford Algorithm** computes the shortest path from a single source node in a graph that **may contain negative edge weights**.

It can also **detect negative weight cycles**, which Dijkstra cannot.

---

## When to Use Bellman–Ford

Use Bellman–Ford when:

* Negative edge weights exist
* You must detect negative cycles
* Graph size is relatively small

Bellman–Ford is slower but more powerful.

---

## Core Intuition

The shortest path between two nodes can contain **at most V − 1 edges** (without cycles).

Bellman–Ford:

* Relaxes **all edges**
* Repeats this process **V − 1 times**

---

## Algorithm Steps

1. Initialize distance array
2. Repeat `V − 1` times:

   * Relax every edge
3. Perform one extra relaxation:

   * If any distance reduces → negative cycle exists

---

## C++ Implementation

```cpp
#include <bits/stdc++.h>
using namespace std;

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
        if (dist[e[0]] != 1e9 && dist[e[0]] + e[2] < dist[e[1]]) {
            cout << "Negative weight cycle detected\n";
            break;
        }
    }
    return dist;
}
```

---

## Time and Space Complexity

* **Time:** `O(V × E)`
* **Space:** `O(V)`

Because every edge is processed multiple times, Bellman–Ford is slower than Dijkstra.

---

## Negative Weight Cycle Detection

A **negative cycle** is a cycle whose total weight is negative.

If a distance can still be relaxed after `V − 1` iterations:

* No shortest path exists
* Distances can decrease indefinitely

This is a critical interview concept.

---

## Dijkstra vs Bellman–Ford

| Feature                  | Dijkstra              | Bellman–Ford        |
| ------------------------ | --------------------- | ------------------- |
| Negative weights         | Not allowed           | Allowed             |
| Negative cycle detection | No                    | Yes                 |
| Time complexity          | `O(E log V)`          | `O(V × E)`          |
| Approach                 | Greedy                | Dynamic Programming |
| Performance              | Fast                  | Slow                |
| Use case                 | Most practical graphs | Special constraints |

---

## Decision Rule

* **All weights ≥ 0 → Dijkstra**
* **Negative weights → Bellman–Ford**
* **Negative cycle detection required → Bellman–Ford**

Stating this clearly in interviews shows algorithmic maturity.

---

## Interview Questions

### Q1. Why can Bellman–Ford handle negative weights?

**Answer:**
Because it relaxes all edges multiple times and does not assume distance finalization at any step.

---

### Q2. Why is Bellman–Ford slower than Dijkstra?

**Answer:**
It processes every edge `V − 1` times, whereas Dijkstra prioritizes minimal distance nodes efficiently.

---

### Q3. Can Dijkstra be modified to handle negative weights?

**Answer:**
No. Negative weights fundamentally break its greedy assumption.

---

### Q4. What happens if a negative cycle exists?

**Answer:**
No finite shortest path exists because distances can decrease infinitely.

---

### Q5. Why do we run Bellman–Ford exactly V − 1 times?

**Answer:**
Because the longest possible simple path contains at most `V − 1` edges.

---

## LeetCode Problems to Practice

### Dijkstra-Based

1. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)
2. [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)
3. [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)
4. [Minimum Cost to Make at Least One Valid Path](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/)
5. [Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

### Bellman–Ford Based

6. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)
7. [Find the City With the Smallest Number of Neighbors](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)
8. [Evaluate Division](https://leetcode.com/problems/evaluate-division/)
9. [Network Delay Time (BF variant)](https://leetcode.com/problems/network-delay-time/)
10. [Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

---

## Summary

* **Dijkstra** is fast and efficient for non-negative weighted graphs

* **Bellman–Ford** is slower but supports negative weights and cycle detection

* Algorithm selection depends entirely on **graph constraints**

* Understanding the failure cases is as important as knowing the algorithm

---
