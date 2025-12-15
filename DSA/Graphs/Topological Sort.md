# Topological Sort and Kahn’s Algorithm

## What Is a Topological Ordering?

A **topological order** of a directed graph is a **linear ordering of vertices** such that:

> For every directed edge `u → v`, vertex `u` appears **before** vertex `v` in the ordering.

This ordering represents **dependency constraints**:

* `u` must be completed before `v`
* `v` depends on `u`

---

## What Is Topological Sort?

**Topological Sort** is an algorithmic process that produces a valid topological ordering of a graph **if and only if** the graph is a **Directed Acyclic Graph (DAG)**.

If the graph contains a cycle:

* No valid topological ordering exists
* Topological sort must fail

This property is frequently used for **cycle detection**.

---

## Where Topological Sort Is Used (Very Important)

Topological sort models **real-world dependency systems**:

* Task scheduling
* Course prerequisites
* Build systems (Maven, Gradle)
* Package dependency resolution
* Job pipelines and workflows
* Backend service startup ordering

Interviewers often test whether you can **recognize DAG problems**.

---

## DAG Requirement ⭐️

Topological sorting is **only defined for Directed Acyclic Graphs**.

* Directed → dependencies have direction
* Acyclic → no circular dependencies

If a cycle exists, topological sort is impossible.

---

## Two Ways to Perform Topological Sort

1. **DFS-based Topological Sort**
2. **BFS-based Topological Sort (Kahn’s Algorithm)**

Both produce valid orderings but use different mechanisms.

---

## DFS-Based Topological Sort

### Core Idea

DFS-based topological sort relies on **post-order traversal**.

A node is added to the ordering **after all its dependencies are processed**.

---

## Algorithm Intuition

1. Perform DFS for each unvisited node
2. After exploring all neighbors of a node, push it onto a stack
3. Reverse the stack to get topological order

This works because deeper dependencies finish first.

---

## DFS Topological Sort Implementation (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

void dfs(int node, vector<vector<int>>& adj,
         vector<bool>& visited, stack<int>& st) {
    visited[node] = true;

    for (int nei : adj[node]) {
        if (!visited[nei]) {
            dfs(nei, adj, visited, st);
        }
    }
    st.push(node);
}

vector<int> topoSortDFS(int n, vector<vector<int>>& adj) {
    vector<bool> visited(n, false);
    stack<int> st;

    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            dfs(i, adj, visited, st);
        }
    }

    vector<int> topo;
    while (!st.empty()) {
        topo.push_back(st.top());
        st.pop();
    }
    return topo;
}
```

---

## Limitations of DFS Toposort

* Does **not directly detect cycles**
* Requires additional recursion-stack tracking
* Risk of stack overflow for deep graphs

---

## Kahn’s Algorithm (BFS-Based Topological Sort)

### Definition

**Kahn’s Algorithm** is a BFS-based algorithm that constructs a topological ordering by repeatedly removing nodes with **zero in-degree**.

It is also a **cycle detection algorithm**.

---

## Core Intuition

* Nodes with **in-degree 0** have no dependencies
* Such nodes can be safely placed in the ordering
* Removing them reduces in-degree of dependent nodes

If at any point:

* No node has in-degree 0
* And unprocessed nodes remain → cycle exists

---

## Algorithm Steps

1. Compute in-degree of every node
2. Push all nodes with in-degree 0 into a queue
3. While queue is not empty:

   * Pop node
   * Add to ordering
   * Reduce in-degree of neighbors
   * Push neighbors whose in-degree becomes 0
4. If processed nodes < total nodes → cycle exists

---

## Kahn’s Algorithm Implementation (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> kahnTopoSort(int n, vector<vector<int>>& adj) {
    vector<int> indegree(n, 0);

    for (int u = 0; u < n; u++) {
        for (int v : adj[u]) {
            indegree[v]++;
        }
    }

    queue<int> q;
    for (int i = 0; i < n; i++) {
        if (indegree[i] == 0)
            q.push(i);
    }

    vector<int> topo;
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        topo.push_back(node);

        for (int nei : adj[node]) {
            indegree[nei]--;
            if (indegree[nei] == 0) {
                q.push(nei);
            }
        }
    }

    if (topo.size() != n) {
        cout << "Cycle detected\n";
    }

    return topo;
}
```

---

## Why Kahn’s Algorithm Is Preferred in Interviews ⭐️

* Explicitly models **dependencies**
* Naturally detects cycles
* Avoids deep recursion
* Easier to reason and debug

Many interviewers prefer Kahn’s over DFS.

---

## Cycle Detection Using Topological Sort

### Key Rule

> If topological sort processes **fewer than V nodes**, the graph contains a cycle.

This rule is often tested directly.

---

## DFS vs Kahn’s Algorithm 

| Aspect                | DFS Toposort       | Kahn’s Algorithm |
| --------------------- | ------------------ | ---------------- |
| Approach              | DFS + stack        | BFS + in-degree  |
| Cycle detection       | Extra logic needed | Built-in         |
| Recursion             | Yes                | No               |
| Ease of understanding | Moderate           | High             |
| Interview preference  | Medium             | High             |

---

## Common Interview Pitfalls

* Attempting to topologically sort an undirected graph
* Forgetting that cycles invalidate toposort
* Incorrect in-degree calculation
* Pushing nodes multiple times into queue
* Confusing BFS traversal with Kahn’s algorithm

---

## Interview Questions

### Q1. Why is topological sort only possible for DAGs?

**Answer:**
Because cycles introduce circular dependencies, making it impossible to order nodes linearly.

---

### Q2. How does Kahn’s algorithm detect cycles?

**Answer:**
If after processing all nodes with in-degree zero, some nodes remain unprocessed, a cycle exists.

---

### Q3. Can multiple topological orders exist?

**Answer:**
Yes. If multiple nodes have in-degree zero at the same time, different valid orderings are possible.

---

### Q4. Difference between BFS traversal and Kahn’s algorithm?

**Answer:**
Kahn’s algorithm uses in-degree constraints, not simple adjacency traversal.

---

### Q5. When would you prefer DFS toposort?

**Answer:**
When recursion is acceptable and cycle detection is not required explicitly.

---

## LeetCode Problems to Practice (Toposort)

1. [Course Schedule](https://leetcode.com/problems/course-schedule/)
2. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
3. [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)
4. [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)
5. [Sequence Reconstruction](https://leetcode.com/problems/sequence-reconstruction/)
6. [Parallel Courses](https://leetcode.com/problems/parallel-courses/)
7. [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)
8. [Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)
9. [Build a Matrix With Conditions](https://leetcode.com/problems/build-a-matrix-with-conditions/)
10. [Largest Color Value in a Directed Graph](https://leetcode.com/problems/largest-color-value-in-a-directed-graph/)

These problems cover **basic DAG ordering, cycle detection, and real dependency graphs**.

---

## Summary

* Topological sort orders nodes based on **dependencies**

* Works **only on DAGs**

* DFS and Kahn’s are two valid approaches

* Kahn’s algorithm is preferred for **cycle detection**

* Critical for scheduling, prerequisites, and backend workflows

---