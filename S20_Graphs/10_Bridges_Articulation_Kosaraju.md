# 395–397: Bridges, Articulation Points, Kosaraju's SCC

**LeetCode Bridges:** https://leetcode.com/problems/critical-connections-in-a-network/
**Difficulty:** 🔴 Hard | **Q# in sheet: 395–397**

---
## 🧠 Q395: Bridges in Graph (Critical Connections)
A **bridge** is an edge whose removal disconnects the graph.

**Tarjan's Algorithm:** Track discovery time and lowest reachable time.
Edge (u,v) is bridge if `low[v] > disc[u]` (v can't reach u's ancestors without this edge).

```java
int timer = 0;
int[] disc, low;
boolean[] visited;
List<List<Integer>> bridges = new ArrayList<>();

public void dfs(int u, int parent, List<List<Integer>> adj) {
    visited[u] = true;
    disc[u] = low[u] = timer++;

    for (int v : adj.get(u)) {
        if (!visited[v]) {
            dfs(v, u, adj);
            low[u] = Math.min(low[u], low[v]);
            if (low[v] > disc[u])  // Bridge condition!
                bridges.add(Arrays.asList(u, v));
        } else if (v != parent) {
            low[u] = Math.min(low[u], disc[v]);
        }
    }
}
```

---
## 🧠 Q396: Articulation Points
A node whose removal disconnects the graph.
Similar to bridges but condition: `low[v] >= disc[u]` for non-root.

```java
// Root is articulation point if it has > 1 DFS children
// Non-root u is AP if low[v] >= disc[u] for any child v
```

---
## 🧠 Q397: Kosaraju's Algorithm (Strongly Connected Components)
**SCC:** Maximal set of vertices mutually reachable.

**3 Steps:**
1. DFS on original graph → record finish order (push to stack)
2. Transpose (reverse) the graph
3. DFS on transposed graph in reverse finish order → each DFS tree = one SCC

```java
// Step 1: Finish-order DFS
void dfs1(int u, boolean[] vis, Deque<Integer> stack, List<List<Integer>> adj) {
    vis[u] = true;
    for (int v : adj.get(u))
        if (!vis[v]) dfs1(v, vis, stack, adj);
    stack.push(u);  // Push AFTER all neighbors
}

// Step 3: DFS on transposed graph
void dfs2(int u, boolean[] vis, List<List<Integer>> radj) {
    vis[u] = true;
    for (int v : radj.get(u))
        if (!vis[v]) dfs2(v, vis, radj);
}

public int countSCCs(int n, List<List<Integer>> adj, List<List<Integer>> radj) {
    boolean[] vis = new boolean[n];
    Deque<Integer> stack = new ArrayDeque<>();
    for (int i = 0; i < n; i++)
        if (!vis[i]) dfs1(i, vis, stack, adj);

    Arrays.fill(vis, false);
    int scc = 0;
    while (!stack.isEmpty()) {
        int u = stack.pop();
        if (!vis[u]) { scc++; dfs2(u, vis, radj); }
    }
    return scc;
}
```
**Time:** O(V+E) | **Space:** O(V+E)

---
## 🎯 Summary
- Bridges: low[v] > disc[u] → edge is bridge (Tarjan's)
- Articulation Points: low[v] >= disc[u] for non-root (Tarjan's variant)
- SCC (Kosaraju's): 2 DFS passes — finish order + transposed graph
- All use DFS with discovery/low times
