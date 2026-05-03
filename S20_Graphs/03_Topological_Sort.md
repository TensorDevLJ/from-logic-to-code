# 03. Topological Sort (DFS + Kahn's Algorithm)

**LeetCode (Course Schedule):** https://leetcode.com/problems/course-schedule/
**GFG:** https://practice.geeksforgeeks.org/problems/topological-sort/1
**Difficulty:** 🔴 Hard | **Pattern:** DAG / DFS / BFS

---

## 🧠 Intuition

**In Simple Words:**
Linear ordering of vertices in a DIRECTED ACYCLIC GRAPH (DAG) such that for every directed edge u → v, vertex u comes before v.
Used for: task scheduling, course prerequisites, build systems.

**Real-Life Analogy:**
Getting dressed. Shoes go on after socks. Shirt before jacket. Topological sort gives a valid order to dress!

**Two Approaches:**
1. **DFS-based:** Finish order reversed = topological order (uses stack)
2. **Kahn's Algorithm (BFS):** Process nodes with in-degree 0 first, remove them, repeat

---

## 🚀 Solution — Both Approaches

```java
import java.util.*;

public class TopologicalSort {

    // APPROACH 1: DFS-based
    public static int[] topoSortDFS(int n, List<List<Integer>> adj) {
        boolean[] visited = new boolean[n];
        Deque<Integer> stack = new ArrayDeque<>();

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(i, adj, visited, stack);
            }
        }

        int[] order = new int[n];
        int idx = 0;
        while (!stack.isEmpty()) order[idx++] = stack.pop();
        return order;
    }

    private static void dfs(int node, List<List<Integer>> adj,
                             boolean[] visited, Deque<Integer> stack) {
        visited[node] = true;
        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) dfs(neighbor, adj, visited, stack);
        }
        stack.push(node);  // Push AFTER all descendants are processed
    }

    // APPROACH 2: Kahn's Algorithm (BFS) — also detects cycles!
    public static int[] topoSortKahn(int n, List<List<Integer>> adj) {
        int[] inDegree = new int[n];  // Count incoming edges

        // Calculate in-degrees
        for (int u = 0; u < n; u++)
            for (int v : adj.get(u)) inDegree[v]++;

        // Start with all nodes with in-degree 0
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++)
            if (inDegree[i] == 0) queue.offer(i);

        int[] order = new int[n];
        int idx = 0;

        while (!queue.isEmpty()) {
            int node = queue.poll();
            order[idx++] = node;

            // Reduce in-degree of neighbors
            for (int neighbor : adj.get(node)) {
                inDegree[neighbor]--;
                if (inDegree[neighbor] == 0) queue.offer(neighbor);  // Now ready!
            }
        }

        // If idx < n → cycle exists (not a DAG)
        if (idx < n) return new int[]{};  // Empty = has cycle

        return order;
    }

    // COURSE SCHEDULE (Detect cycle using Kahn's)
    public static boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) adj.add(new ArrayList<>());

        for (int[] pre : prerequisites) adj.get(pre[1]).add(pre[0]);

        int[] result = topoSortKahn(numCourses, adj);
        return result.length == numCourses;  // All courses reachable
    }

    public static void main(String[] args) {
        // DAG: 5→2, 5→0, 4→0, 4→1, 2→3, 3→1
        int n = 6;
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
        adj.get(5).addAll(Arrays.asList(2, 0));
        adj.get(4).addAll(Arrays.asList(0, 1));
        adj.get(2).add(3);
        adj.get(3).add(1);

        System.out.println("DFS Topo: " + Arrays.toString(topoSortDFS(n, adj)));
        // One valid order: [5, 4, 2, 3, 1, 0] or similar

        System.out.println("Kahn's Topo: " + Arrays.toString(topoSortKahn(n, adj)));
    }
}
```

**Time:** O(V + E) both methods | **Space:** O(V + E)

**Kahn's Algorithm Dry Run:**
```
DAG: 5→2, 5→0, 4→0, 4→1, 2→3, 3→1
inDegree: [2, 2, 1, 1, 0, 0]

Start: queue = [4, 5] (in-degree 0)

Process 4: order=[4], reduce inDegree[0]=1, inDegree[1]=1
Process 5: order=[4,5], reduce inDegree[2]=0→queue, inDegree[0]=0→queue

Process 2: order=[4,5,2], reduce inDegree[3]=0→queue
Process 0: order=[4,5,2,0]
Process 3: order=[4,5,2,0,3], reduce inDegree[1]=0→queue
Process 1: order=[4,5,2,0,3,1]

idx=6==n=6 → no cycle!
Result: [4,5,2,0,3,1] ✅ (one valid topological order)
```

---

## 🔁 Pattern Recognition

**Pattern:** DAG processing — crucial for:
- Course Schedule I & II
- Alien Dictionary (find character order)
- Build system dependencies
- Task scheduling

---

## 🧠 How to Think (Interview View)

1. "DAG ordering" → Topological Sort
2. "Check if cycle in directed graph" → Kahn's: if result.length < n → cycle!
3. DFS: push after ALL descendants processed (naturally gets reverse finish order)

---

## ⚠️ Common Mistakes

1. ❌ Applying topological sort to undirected graph
2. ❌ Not handling disconnected components (need outer loop in DFS)
3. ❌ Wrong cycle detection: Kahn's detects cycle if not all nodes processed

---

## 🎯 Final Summary

- Topological sort: valid linear order of DAG nodes (u before v if u→v)
- DFS: push node AFTER all neighbors processed, pop stack = topo order
- Kahn's: BFS from in-degree-0 nodes, reduce in-degrees iteratively
- Kahn's bonus: if result.length < n → graph has cycle!
- Both O(V+E) time
