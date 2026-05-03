# 385–388: Minimum Spanning Tree (Prim's & Kruskal's)

**GFG:** https://practice.geeksforgeeks.org/problems/minimum-spanning-tree/1
**Difficulty:** 🔴 Hard | **Q# in sheet: 385–388**

---
## 🧠 Intuition
**MST:** Spanning tree with minimum total edge weight.
N nodes → N-1 edges connecting all nodes with minimum weight, no cycles.

**Two Classic Algorithms:**
1. **Prim's:** Greedy — grow MST from one node, always pick cheapest edge to new node
2. **Kruskal's:** Sort edges by weight, add if it doesn't form cycle (use DSU)

---
## 🚀 Prim's Algorithm (Priority Queue)
```java
public int primsMST(int n, List<List<int[]>> adj) {
    // adj[u] = list of {v, weight}
    boolean[] inMST = new boolean[n];
    PriorityQueue<int[]> pq = new PriorityQueue<>((a,b) -> a[0]-b[0]); // {weight, node}
    pq.offer(new int[]{0, 0}); // Start from node 0 with cost 0
    int totalWeight = 0;

    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int wt = curr[0], u = curr[1];

        if (inMST[u]) continue;  // Already in MST
        inMST[u] = true;
        totalWeight += wt;

        for (int[] edge : adj.get(u)) {
            int v = edge[0], w = edge[1];
            if (!inMST[v]) pq.offer(new int[]{w, v});
        }
    }
    return totalWeight;
}
```

---
## 🚀 Kruskal's Algorithm (DSU)
```java
public int kruskalsMST(int n, int[][] edges) {
    // edges[i] = {weight, u, v}
    Arrays.sort(edges, (a, b) -> a[0] - b[0]); // Sort by weight

    DisjointSet ds = new DisjointSet(n);
    int totalWeight = 0, edgesUsed = 0;

    for (int[] edge : edges) {
        int w = edge[0], u = edge[1], v = edge[2];
        if (ds.union(u, v)) {   // No cycle formed
            totalWeight += w;
            edgesUsed++;
            if (edgesUsed == n-1) break; // MST complete
        }
    }
    return totalWeight;
}
```

---
## 📊 Comparison
| | Prim's | Kruskal's |
|--|--------|-----------|
| **Time** | O((V+E) log V) | O(E log E) |
| **Better for** | Dense graphs | Sparse graphs |
| **Approach** | Vertex-based greedy | Edge-based greedy |
| **Data Structure** | Priority Queue | DSU |

---
## ⚠️ Common Mistakes
- ❌ Prim's: not checking `inMST[u]` → adds node multiple times
- ❌ Kruskal's: forgetting to sort edges first

---
## 🎯 Summary
- MST: minimum weight tree connecting all nodes
- Prim's: grow from one vertex using min-heap, O((V+E)logV)
- Kruskal's: sort edges + DSU for cycle check, O(E log E)
- Both give same MST weight (different structures possible)
