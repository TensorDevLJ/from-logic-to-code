# 387: Disjoint Set (Union-Find)

**LeetCode:** https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/
**Difficulty:** 🔴 Hard | **Q# in sheet: 387**

---
## 🧠 Intuition
Data structure to efficiently:
- **Union:** Merge two groups/sets
- **Find:** Which group does an element belong to?

**Real-Life Analogy:** Social network friend circles. Merge two circles when two people connect. Quickly check if two people are in the same circle.

**Two Optimizations:**
1. **Path Compression:** When finding root, flatten the tree (point directly to root)
2. **Union by Rank:** Attach smaller tree under larger tree

---
## 🚀 Full Implementation
```java
public class DisjointSet {
    private int[] parent, rank;

    public DisjointSet(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i; // Each node is its own parent
    }

    // Find with Path Compression
    public int find(int x) {
        if (parent[x] != x)
            parent[x] = find(parent[x]);  // Path compression!
        return parent[x];
    }

    // Union by Rank
    public boolean union(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return false;  // Already in same set

        if (rank[px] < rank[py]) { int t = px; px = py; py = t; }
        parent[py] = px;             // Attach smaller under larger
        if (rank[px] == rank[py]) rank[px]++;
        return true;
    }

    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}
```
**Time:** O(α(N)) ≈ O(1) amortized (inverse Ackermann — practically constant!)
**Space:** O(N)

---
## 📌 Example: Number of Provinces (LeetCode 547)
```java
public int findCircleNum(int[][] isConnected) {
    int n = isConnected.length;
    DisjointSet ds = new DisjointSet(n);
    int components = n;

    for (int i = 0; i < n; i++)
        for (int j = i+1; j < n; j++)
            if (isConnected[i][j] == 1 && ds.union(i, j))
                components--;  // Merged two components

    return components;
}
```

---
## 🔁 When to Use DSU
- Detect cycle in undirected graph
- Kruskal's MST algorithm
- Count connected components
- Number of Islands II (dynamic)
- Accounts Merge

---
## ⚠️ Common Mistakes
- ❌ Forgetting path compression (O(log N) without it)
- ❌ Not checking if already connected before union

---
## 🎯 Summary
- parent[i]=i initially. find() returns root with path compression.
- union() attaches by rank — keep trees flat
- connected(x,y) → find(x)==find(y)
- O(α(N)) ≈ constant time — extremely fast
