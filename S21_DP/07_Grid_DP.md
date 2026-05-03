# 404–409: 2D Grid DP (Unique Paths, Triangle, Min Falling Path)

---
## 🧠 Q405: Unique Paths (LeetCode 62)
https://leetcode.com/problems/unique-paths/

**Problem:** Robot at top-left of m×n grid. Can only move right or down. Count paths to bottom-right.

**Intuition:** dp[i][j] = ways to reach (i,j) = from above + from left

```java
public int uniquePaths(int m, int n) {
    int[][] dp = new int[m][n];
    // First row and column = 1 (only one way to reach each)
    for (int i = 0; i < m; i++) dp[i][0] = 1;
    for (int j = 0; j < n; j++) dp[0][j] = 1;

    for (int i = 1; i < m; i++)
        for (int j = 1; j < n; j++)
            dp[i][j] = dp[i-1][j] + dp[i][j-1];

    return dp[m-1][n-1];
}
// Time: O(M×N) | Space: O(M×N) → can optimize to O(N)
```

**Dry Run:** m=3, n=3
```
1 1 1
1 2 3
1 3 6   → answer: 6
```

---
## 🧠 Q406: Unique Paths II (LeetCode 63 — with obstacles)
https://leetcode.com/problems/unique-paths-ii/

```java
public int uniquePathsWithObstacles(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    if (grid[0][0] == 1 || grid[m-1][n-1] == 1) return 0;
    int[][] dp = new int[m][n];
    dp[0][0] = 1;

    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++) {
            if (i == 0 && j == 0) continue;
            if (grid[i][j] == 1) { dp[i][j] = 0; continue; }  // Obstacle!
            int up = i > 0 ? dp[i-1][j] : 0;
            int left = j > 0 ? dp[i][j-1] : 0;
            dp[i][j] = up + left;
        }
    return dp[m-1][n-1];
}
```

---
## 🧠 Q407: Minimum Falling Path Sum (LeetCode 931)
https://leetcode.com/problems/minimum-falling-path-sum/

**Problem:** Start anywhere in top row, reach bottom row. Each step: go to same/adjacent column below. Minimize sum.

```java
public int minFallingPathSum(int[][] matrix) {
    int n = matrix.length;
    int[][] dp = new int[n][n];

    // Base: first row = matrix values
    for (int j = 0; j < n; j++) dp[0][j] = matrix[0][j];

    for (int i = 1; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int up = dp[i-1][j];
            int upLeft = j > 0 ? dp[i-1][j-1] : Integer.MAX_VALUE;
            int upRight = j < n-1 ? dp[i-1][j+1] : Integer.MAX_VALUE;
            dp[i][j] = matrix[i][j] + Math.min(up, Math.min(upLeft, upRight));
        }
    }

    int ans = Integer.MAX_VALUE;
    for (int j = 0; j < n; j++) ans = Math.min(ans, dp[n-1][j]);
    return ans;
}
```

---
## 🧠 Q408: Triangle (LeetCode 120)
https://leetcode.com/problems/triangle/

**Problem:** Triangle of numbers. Find min path sum from top to bottom (each step: adjacent in next row).

**Trick:** Start from BOTTOM row, work upward!
```java
public int minimumTotal(List<List<Integer>> triangle) {
    int n = triangle.size();
    int[] dp = new int[triangle.get(n-1).size()];

    // Fill with bottom row
    for (int i = 0; i < dp.length; i++) dp[i] = triangle.get(n-1).get(i);

    // Bottom-up
    for (int i = n-2; i >= 0; i--)
        for (int j = 0; j <= i; j++)
            dp[j] = triangle.get(i).get(j) + Math.min(dp[j], dp[j+1]);

    return dp[0];
}
// Time: O(N²) | Space: O(N)
```

---
## 🎯 Grid DP Summary
| Problem | Recurrence | Direction |
|---------|-----------|-----------|
| Unique Paths | dp[i][j] = dp[i-1][j] + dp[i][j-1] | Top-down |
| Min Falling | dp[i][j] = matrix[i][j] + min(3 above) | Top-down |
| Triangle | dp[j] = tri[i][j] + min(dp[j], dp[j+1]) | Bottom-up |
