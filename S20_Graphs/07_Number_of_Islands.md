# 362: Number of Islands

**LeetCode:** https://leetcode.com/problems/number-of-islands/
**Difficulty:** 🟡 Medium | **Q# in sheet: 362**

---
## 🧠 Intuition
Count connected components of '1' (land) in 2D grid.
Each time we find an unvisited '1', it's a new island. BFS/DFS to mark all connected land.

---
## 🚀 Solution (DFS)
```java
public int numIslands(char[][] grid) {
    int m = grid.length, n = grid[0].length;
    int count = 0;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == '1') {
                count++;
                dfs(grid, i, j);  // Sink entire island
            }
        }
    }
    return count;
}

private void dfs(char[][] grid, int r, int c) {
    if (r < 0 || r >= grid.length || c < 0 || c >= grid[0].length
        || grid[r][c] != '1') return;

    grid[r][c] = '0';  // Sink (mark visited in-place)
    dfs(grid, r+1, c); dfs(grid, r-1, c);
    dfs(grid, r, c+1); dfs(grid, r, c-1);
}
```
**Time:** O(M×N) | **Space:** O(M×N) recursion stack

---
## 🎯 Summary
- Scan grid: when '1' found → count++, DFS to sink entire island
- Mark visited by changing '1' to '0' (in-place, no extra array)
- Pattern reused in: Flood Fill, Surrounded Regions, Enclaves
