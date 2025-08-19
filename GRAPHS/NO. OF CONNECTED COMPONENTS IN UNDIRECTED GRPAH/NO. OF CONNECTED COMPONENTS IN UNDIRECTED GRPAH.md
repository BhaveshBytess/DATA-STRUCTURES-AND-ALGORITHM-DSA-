# **Problem: Number of Connected Components in an Undirected Graph**

📄 **LeetCode Style Format**

---

**You are given an integer `n` representing the number of nodes in an undirected graph, numbered from `0` to `n - 1`, and a 2D array `edges` where each `edges[i] = [uᵢ, vᵢ]` represents an undirected edge between nodes `uᵢ` and `vᵢ`.**

Return the **number of connected components** in the graph.

---

### 🔸 **Example 1:**

```
Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
```

### 🔸 **Example 2:**

```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
Output: 1
```

---

### 🔹 **Constraints:**

* `1 <= n <= 2000`
* `0 <= edges.length <= 5000`
* `edges[i].length == 2`
* `0 <= uᵢ, vᵢ < n`
* `uᵢ ≠ vᵢ`
* There are no repeated edges.

---
Absolutely, Bhavesh! Here's your **mini note** for **"Number of Connected Components in an Undirected Graph"** — following your custom format for consistent DSA revision:

---

### **Mini Note**

🔹 **Problem:** Number of Connected Components in an Undirected Graph
🔹 **Graph Type:** Undirected Graph (Adjacency List)
🔹 **Technique:** DFS (or BFS) Traversal + Visited Tracking
🔹 **Core Template:** Count how many times DFS starts → each time = new component
🔹 **Pattern Tag:** #GraphTraversal #ConnectedComponents

---

🔸 **Logic Summary:**

* Build the adjacency list from the edge list
* Maintain a `visited[]` array
* For every unvisited node, perform DFS to mark the entire connected component
* Count how many such independent DFS traversals were done — that’s the number of connected components

---

🔸 **Trick:**

* Each DFS traversal discovers one full component
* The total number of DFS calls = number of components

---

🔸 **Why Union Find can be used here:**

* Union Find is a more optimal approach if the graph is very large
* Especially useful in dynamic graphs (when edges are being added in real time)

---

🔸 **Final Insight:**
This is the **foundation problem** for any graph question involving **connected components**. Whether grid-based, string-based, or DSU-based, this logic will come up repeatedly.


---

# SOLUTION

```cpp
class Solution {
public:
    void dfs(int node, vector<vector<int>>& adj, vector<bool>& visited) {
        visited[node] = true;
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                dfs(neighbor, adj, visited);
            }
        }
    }

    int countComponents(int n, vector<vector<int>>& edges) {
        vector<vector<int>> adj(n); // adjacency list

        // Build graph
        for (auto& edge : edges) {
            int u = edge[0];
            int v = edge[1];
            adj[u].push_back(v);
            adj[v].push_back(u); // because undirected
        }

        vector<bool> visited(n, false);
        int components = 0;

        for (int i = 0; i < n; ++i) {
            if (!visited[i]) {
                components++;        // new component found
                dfs(i, adj, visited);
            }
        }

        return components;
    }
};
```

---

### Quick Intuition Recap:

- Each unvisited node → start of a new component.
    
- DFS marks all reachable nodes from it.
    
- Every time you enter DFS from the main loop, that’s a new component.
    
---


## DSU METHOD


```cpp
class DSU {
    vector<int> parent, rank;
public:
    DSU(int n) {
        parent.resize(n);
        rank.resize(n, 0);
        for (int i = 0; i < n; i++) parent[i] = i;
    }

    int find(int x) {
        if (parent[x] != x)
            parent[x] = find(parent[x]); // path compression
        return parent[x];
    }

    bool unite(int u, int v) {
        int pu = find(u), pv = find(v);
        if (pu == pv) return false; // already connected

        // union by rank
        if (rank[pu] < rank[pv]) {
            parent[pu] = pv;
        } else if (rank[pu] > rank[pv]) {
            parent[pv] = pu;
        } else {
            parent[pu] = pv;
            rank[pv]++;
        }
        return true;
    }
};

class Solution {
public:
    int countComponents(int n, vector<vector<int>>& edges) {
        DSU dsu(n);
        int components = n;  // initially each node is its own component

        for (auto &e : edges) {
            if (dsu.unite(e[0], e[1])) {
                components--;  // successfully merged → 1 less component
            }
        }

        return components;
    }
};
```

---

## 🔹 Pattern Tag

- **Union-Find**
    
- **Connected Components**
    

---

## 🔸 Logic Summary

1. Start with `n` components.
    
2. For each edge `(u, v)`, if they belong to different components → merge them → `components--`.
    
3. Final answer = `components`.
    

---

## 🔸 Trick

- Key idea: DSU naturally merges connected nodes into same “set”.
    
- Avoids writing BFS/DFS manually.
    

---

## 🔸 Why DFS/BFS works but DSU is better here?

- DFS/BFS: O(N + E), must maintain visited array.
    
- DSU: Clean, especially if graph is given in **edge list** form.
    

---

## 🔸 Final Insight

DSU shines whenever you want to **count or check groups/components** without explicitly traversing adjacency lists.


