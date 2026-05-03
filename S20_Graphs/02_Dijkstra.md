# 02. Dijkstra's Shortest Path Algorithm

**LeetCode (Network Delay):** https://leetcode.com/problems/network-delay-time/
**GFG:** https://practice.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1
**Difficulty:** 🔴 Hard | **Pattern:** Greedy + Priority Queue

---

## 🧠 Intuition

**In Simple Words:**
Find shortest distance from source to ALL other vertices in a weighted graph (non-negative weights).

**Real-Life Analogy:**
GPS navigation. Finding fastest route from your location to all destinations. Always exploring the currently-closest unexplored city.

**Core Idea:**
Greedy approach: always process the node with the SMALLEST current distance.
Use Min-Heap (Priority Queue) to efficiently get the minimum distance node.

**Why Priority Queue?**
Instead of O(V²) linear scan for minimum, PQ gives O(log V) for minimum extraction.

---

## 🚀 Solution

```java
import java.util.*;

public class Dijkstra {
    public static int[] dijkstra(int n, List<List<int[]>> adj, int src) {
        // dist[i] = shortest distance from src to i
        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[src] = 0;

        // Min heap: [distance, node]
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[]{0, src});

        while (!pq.isEmpty()) {
            int[] curr = pq.poll();
            int d = curr[0], u = curr[1];

            // Skip if we already found a better path
            if (d > dist[u]) continue;

            // Relax all edges from u
            for (int[] edge : adj.get(u)) {
                int v = edge[0], weight = edge[1];

                if (dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;   // Relax edge
                    pq.offer(new int[]{dist[v], v}); // Add to PQ
                }
            }
        }

        return dist;
    }

    public static void main(String[] args) {
        int n = 5;
        List<List<int[]>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) adj.add(new ArrayList<>());

        // Add weighted edges (directed)
        adj.get(0).add(new int[]{1, 4});
        adj.get(0).add(new int[]{2, 1});
        adj.get(2).add(new int[]{1, 2});
        adj.get(1).add(new int[]{3, 1});
        adj.get(2).add(new int[]{3, 5});

        int[] dist = dijkstra(n, adj, 0);
        System.out.println(Arrays.toString(dist));
        // [0, 3, 1, 4, MAX] (from source 0)
    }
}
```

**Time:** O((V + E) log V) | **Space:** O(V + E)

**Dry Run:**
```
Graph: 0→1(4), 0→2(1), 2→1(2), 1→3(1), 2→3(5)
dist = [0, INF, INF, INF, INF]

PQ = [(0, 0)]

Pop (0, 0): process node 0
  Edge to 1: dist[1] = 0+4 = 4 < INF → dist[1]=4, pq.add(4,1)
  Edge to 2: dist[2] = 0+1 = 1 < INF → dist[2]=1, pq.add(1,2)
PQ = [(1,2), (4,1)]

Pop (1, 2): process node 2, dist=1
  Edge to 1: dist[1] = 1+2=3 < 4 → dist[1]=3, pq.add(3,1)
  Edge to 3: dist[3] = 1+5=6 < INF → dist[3]=6, pq.add(6,3)
PQ = [(3,1), (4,1), (6,3)]

Pop (3, 1): process node 1, dist=3
  Edge to 3: dist[3] = 3+1=4 < 6 → dist[3]=4, pq.add(4,3)
PQ = [(4,1), (4,3), (6,3)]

Pop (4, 1): d=4, dist[1]=3. d > dist[1] → SKIP
Pop (4, 3): d=4, dist[3]=4. Process node 3 (no outgoing edges)
Pop (6, 3): d=6, dist[3]=4. d > dist[3] → SKIP

Final dist = [0, 3, 1, 4, INF] ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Greedy + Min-Heap
**Key constraint:** Non-negative weights only (use Bellman-Ford for negative weights)

**Why `if (d > dist[u]) continue`?**
Stale entries in PQ — we may add same node multiple times with different distances.
Skip if we already processed it with a better distance.

---

## ⚠️ Common Mistakes

1. ❌ Not skipping stale PQ entries (d > dist[u])
2. ❌ Using Dijkstra with negative edge weights (use Bellman-Ford)
3. ❌ Not initializing dist with MAX_VALUE

---

## 🧩 Variations

1. **Shortest Path in Binary Maze** — 0-1 BFS or Dijkstra
2. **Path with Minimum Effort** — Dijkstra variant (minimize max edge)
3. **Cheapest Flights within K Stops** — Bellman-Ford variation
4. **Network Delay Time** — direct Dijkstra

---

## 🎯 Final Summary

- Greedy: always process closest unprocessed node
- Min-heap PQ for O(log V) minimum extraction
- Relax edges: dist[v] = min(dist[v], dist[u] + w)
- Skip stale PQ entries: if d > dist[u], skip
- O((V+E) log V) — works only for non-negative weights
