# 351–353: Rotten Oranges (BFS Multi-Source)

**LeetCode:** https://leetcode.com/problems/rotting-oranges/
**Difficulty:** 🟡 Medium | **Q# in sheet: 353**

---
## 🧠 Intuition
Grid of 0(empty), 1(fresh), 2(rotten). Each minute, rotten spreads to 4-adjacent fresh.
Return min minutes until all fresh rot, or -1 if impossible.

**Key:** Multi-source BFS — all rotten oranges spread SIMULTANEOUSLY.
Start BFS from ALL rotten oranges at once (add all 2s to queue initially).

---
## 📌 Example
```
Grid:
2 1 1
1 1 0
0 1 1
Answer: 4 minutes
```

---
## 🚀 Solution
```java
public int orangesRotting(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    Queue<int[]> q = new LinkedList<>();
    int fresh = 0;

    // Step 1: Add ALL rotten oranges to queue, count fresh
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 2) q.offer(new int[]{i, j});
            if (grid[i][j] == 1) fresh++;
        }

    if (fresh == 0) return 0;  // No fresh oranges

    int[][] dirs = {{0,1},{0,-1},{1,0},{-1,0}};
    int minutes = 0;

    // Step 2: BFS level by level (each level = 1 minute)
    while (!q.isEmpty() && fresh > 0) {
        minutes++;
        int size = q.size();
        for (int k = 0; k < size; k++) {
            int[] cell = q.poll();
            for (int[] d : dirs) {
                int r = cell[0] + d[0], c = cell[1] + d[1];
                if (r >= 0 && r < m && c >= 0 && c < n && grid[r][c] == 1) {
                    grid[r][c] = 2;   // Rot it
                    fresh--;
                    q.offer(new int[]{r, c});
                }
            }
        }
    }
    return fresh == 0 ? minutes : -1;
}
```
**Time:** O(M×N) | **Space:** O(M×N)

---
## ⚠️ Common Mistakes
- ❌ BFS from one rotten orange at a time (wrong — all spread simultaneously)
- ❌ Forgetting `-1` when fresh > 0 after BFS

---
## 🎯 Summary
- Multi-source BFS: add ALL rotten cells to queue first
- Level-by-level BFS = minutes elapsed
- If fresh > 0 after BFS → -1 (isolated fresh oranges)
