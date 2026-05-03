# 444–450: Matrix Chain Multiplication & Partition DP

---
## 🧠 Q444: Matrix Chain Multiplication
**GFG:** https://practice.geeksforgeeks.org/problems/matrix-chain-multiplication0303/1

**Problem:** Parenthesize N matrices A1×A2×...×AN to minimize multiplication cost.
Cost of multiplying (p×q) × (q×r) = p×q×r.

**Intuition:** Split at position k: cost = MCM(i,k) + MCM(k+1,j) + cost_of_merging
dp[i][j] = min multiplications to compute matrix chain i..j

```java
public int matrixMultiplication(int[] arr, int n) {
    // arr[i-1]×arr[i] is dimension of matrix i
    int[][] dp = new int[n][n];
    // dp[i][i] = 0 (single matrix, no cost)

    // Fill by chain length
    for (int len = 2; len < n; len++) {          // chain length
        for (int i = 1; i < n - len + 1; i++) {  // start index
            int j = i + len - 1;                   // end index
            dp[i][j] = Integer.MAX_VALUE;

            for (int k = i; k < j; k++) {          // split point
                int cost = dp[i][k] + dp[k+1][j] + arr[i-1] * arr[k] * arr[j];
                dp[i][j] = Math.min(dp[i][j], cost);
            }
        }
    }
    return dp[1][n-1];
}
```
**Time:** O(N³) | **Space:** O(N²)

---
## 🧠 Q446: Minimum Cost to Cut a Stick (LeetCode 1547)
https://leetcode.com/problems/minimum-cost-to-cut-a-stick/

**Intuition:** Same as MCM! dp[i][j] = min cost to make all cuts between positions i and j.

```java
public int minCost(int n, int[] cuts) {
    Arrays.sort(cuts);
    int m = cuts.length;
    // Add boundary 0 and n
    int[] c = new int[m + 2];
    c[0] = 0; c[m+1] = n;
    for (int i = 1; i <= m; i++) c[i] = cuts[i-1];

    int[][] dp = new int[m+2][m+2];

    for (int len = 2; len <= m+1; len++) {
        for (int i = 0; i+len <= m+1; i++) {
            int j = i + len;
            dp[i][j] = Integer.MAX_VALUE;
            for (int k = i+1; k < j; k++) {
                dp[i][j] = Math.min(dp[i][j],
                    dp[i][k] + dp[k][j] + c[j] - c[i]);
            }
        }
    }
    return dp[0][m+1];
}
```

---
## 🧠 Q447: Burst Balloons (LeetCode 312)
https://leetcode.com/problems/burst-balloons/

**Key trick:** Think of k as the LAST balloon burst (not first).
dp[i][j] = max coins if we burst all balloons between i and j.

```java
public int maxCoins(int[] nums) {
    int n = nums.length;
    int[] arr = new int[n+2];
    arr[0] = arr[n+1] = 1;  // Add virtual balloons with value 1
    for (int i = 1; i <= n; i++) arr[i] = nums[i-1];

    int[][] dp = new int[n+2][n+2];

    for (int len = 1; len <= n; len++) {
        for (int i = 1; i+len-1 <= n; i++) {
            int j = i + len - 1;
            for (int k = i; k <= j; k++) {  // k = last burst
                dp[i][j] = Math.max(dp[i][j],
                    dp[i][k-1] + arr[i-1]*arr[k]*arr[j+1] + dp[k+1][j]);
            }
        }
    }
    return dp[1][n];
}
```

---
## 🔁 Partition DP Pattern
**Identify:** "Partition sequence into parts, minimize/maximize cost of each part"
**Structure:** dp[i][j] = try all split points k between i and j
**Problems:** MCM, Cut Stick, Burst Balloons, Palindrome Partition II, Boolean Parenthesization

---
## 🎯 Summary
- MCM: dp[i][j] = min cost, split at every k in [i,j-1]
- Same pattern: Cut Stick, Burst Balloons
- Burst Balloons trick: think of last burst (reverse logic)
- All O(N³) time, O(N²) space
