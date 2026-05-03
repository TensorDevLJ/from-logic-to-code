# 01. BFS and DFS of Graph

**LeetCode BFS/DFS (Number of Provinces):** https://leetcode.com/problems/number-of-provinces/
**GFG BFS:** https://practice.geeksforgeeks.org/problems/bfs-traversal-of-graph/1
**Difficulty:** 🟡 Medium | **Pattern:** Graph Traversal

---

## 🧠 Intuition

**BFS (Breadth-First Search):**
Explore all neighbors at current level before going deeper.
Like ripples in water — spreading outward level by level.
Uses QUEUE.

**DFS (Depth-First Search):**
Go as deep as possible, backtrack when stuck.
Like exploring a maze — go down one path completely, then try another.
Uses STACK (or recursion).

---

## 📌 Graph Representation

```java
// Adjacency List (most common in interviews)
// n vertices, edges as list
List<List<Integer>> adj = new ArrayList<>();
for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
adj.get(u).add(v);  // Add edge u → v
```

---

## 🚀 BFS and DFS

```java
import java.util.*;

public class GraphTraversal {

    // BFS — Level by level
    public static List<Integer> bfs(int start, int n, List<List<Integer>> adj) {
        List<Integer> visited_order = new ArrayList<>();
        boolean[] visited = new boolean[n];

        Queue<Integer> queue = new LinkedList<>();
        queue.offer(start);
        visited[start] = true;

        while (!queue.isEmpty()) {
            int node = queue.poll();
            visited_order.add(node);

            // Add all unvisited neighbors
            for (int neighbor : adj.get(node)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.offer(neighbor);
                }
            }
        }

        return visited_order;
    }

    // DFS — Go deep, backtrack
    public static List<Integer> dfs(int start, int n, List<List<Integer>> adj) {
        List<Integer> visited_order = new ArrayList<>();
        boolean[] visited = new boolean[n];
        dfsHelper(start, adj, visited, visited_order);
        return visited_order;
    }

    private static void dfsHelper(int node, List<List<Integer>> adj,
                                   boolean[] visited, List<Integer> order) {
        visited[node] = true;
        order.add(node);

        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                dfsHelper(neighbor, adj, visited, order);
            }
        }
    }

    // Count connected components (Number of Provinces)
    public static int countComponents(int n, List<List<Integer>> adj) {
        boolean[] visited = new boolean[n];
        int count = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                count++;
                dfsHelper(i, adj, visited, new ArrayList<>());
            }
        }

        return count;
    }

    public static void main(String[] args) {
        // Graph: 0-1-2-3, 0-2
        int n = 4;
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) adj.add(new ArrayList<>());

        adj.get(0).addAll(Arrays.asList(1, 2));
        adj.get(1).addAll(Arrays.asList(0, 3));
        adj.get(2).addAll(Arrays.asList(0, 3));
        adj.get(3).addAll(Arrays.asList(1, 2));

        System.out.println("BFS from 0: " + bfs(0, n, adj)); // [0, 1, 2, 3]
        System.out.println("DFS from 0: " + dfs(0, n, adj)); // [0, 1, 3, 2]
    }
}
```

**BFS Time:** O(V + E) | **Space:** O(V)  
**DFS Time:** O(V + E) | **Space:** O(V) recursion/stack

**BFS Dry Run:** Graph: 0→{1,2}, 1→{0,3}, 2→{0,3}
```
Start: queue=[0], visited={0}
Pop 0 → add to result. Neighbors: 1,2 → queue=[1,2], visited={0,1,2}
Pop 1 → add. Neighbors: 0(visited), 3 → queue=[2,3], visited={0,1,2,3}
Pop 2 → add. Neighbors: 0,3 (both visited) → queue=[3]
Pop 3 → add. No new neighbors.
Result: [0,1,2,3] ✅
```

---

## 🔁 When to Use BFS vs DFS

| BFS | DFS |
|-----|-----|
| Shortest path (unweighted) | Detect cycles |
| Level-order processing | Topological sort |
| Nearest neighbor | Connected components |
| Word Ladder | Maze solving |
| Rotten oranges | Path existence |

---

## ⚠️ Common Mistakes

1. ❌ Not marking visited BEFORE adding to queue (duplicate processing!)
2. ❌ Forgetting disconnected components (need outer loop)
3. ❌ DFS: missing visited array causes infinite recursion in cycles

---

## 🎯 Final Summary

- BFS: Queue + visited array, process level by level
- DFS: Recursion/Stack + visited, go deep then backtrack
- ALWAYS mark visited when ADDING to queue (not when processing!)
- For disconnected graphs: outer loop over all nodes
- Both O(V+E) time
