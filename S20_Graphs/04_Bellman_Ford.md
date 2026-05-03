# 04. Bellman-Ford Algorithm

**LeetCode:** https://leetcode.com/problems/cheapest-flights-within-k-stops/
**GFG:** https://practice.geeksforgeeks.org/problems/distance-from-the-source-bellman-ford-algorithm/1
**Difficulty:** 🔴 Hard | **Pattern:** Dynamic Programming on Graphs

---

## 🧠 Intuition

**In Simple Words:**
Find shortest path from source to all vertices, even with NEGATIVE edge weights.
Also DETECTS negative weight cycles.

**Why not Dijkstra?**
Dijkstra fails with negative edges (greedy assumption breaks).
Bellman-Ford relaxes ALL edges V-1 times → guaranteed shortest paths.

**Core Idea:**
A shortest path in V-vertex graph has at most V-1 edges.
Relax all edges V-1 times → after each iteration, shortest paths using at most k edges are computed.

---

## 🚀 Solution

```java
public class BellmanFord {
    public static int[] bellmanFord(int n, int[][] edges, int src) {
        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[src] = 0;

        // Relax all edges n-1 times
        for (int i = 1; i <= n - 1; i++) {
            boolean updated = false;  // Optimization: early exit

            for (int[] edge : edges) {
                int u = edge[0], v = edge[1], w = edge[2];

                if (dist[u] != Integer.MAX_VALUE && dist[u] + w < dist[v]) {
                    dist[v] = dist[u] + w;
                    updated = true;
                }
            }

            if (!updated) break;  // No updates → already optimal
        }

        // Check for negative cycle: if still updating in n-th iteration → negative cycle
        for (int[] edge : edges) {
            int u = edge[0], v = edge[1], w = edge[2];
            if (dist[u] != Integer.MAX_VALUE && dist[u] + w < dist[v]) {
                System.out.println("Negative cycle detected!");
                return new int[]{};
            }
        }

        return dist;
    }
}
```

**Time:** O(V × E) | **Space:** O(V)

**vs Dijkstra:**
| | Dijkstra | Bellman-Ford |
|--|---------|-------------|
| Negative weights | ❌ | ✅ |
| Time | O((V+E) log V) | O(VE) |
| Cycle detection | ❌ | ✅ (negative) |

---

## 🎯 Final Summary

- Relax all edges V-1 times to find shortest paths
- V-th relaxation: if still updates → negative cycle exists
- Works with negative edges (Dijkstra does NOT)
- O(VE) time — slower than Dijkstra but more general
