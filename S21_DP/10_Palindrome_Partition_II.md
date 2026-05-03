# 449: Palindrome Partitioning II (LeetCode 132)

**LeetCode:** https://leetcode.com/problems/palindrome-partitioning-ii/
**Difficulty:** 🔴 Hard | **Q# in sheet: 449**

---
## 🧠 Intuition
Minimum cuts to partition string into all palindromes.
"aab" → "a|a|b" = 1 cut (or "aa|b" = 1 cut). Answer: 1.

**2-Step DP:**
1. Precompute isPalin[i][j] = is s[i..j] a palindrome?
2. dp[i] = min cuts for s[0..i]
   If s[0..i] is palindrome → dp[i] = 0 (no cut needed)
   Else → dp[i] = min(dp[j-1] + 1) for all j where s[j..i] is palindrome

```java
public int minCut(String s) {
    int n = s.length();

    // Precompute palindromes
    boolean[][] isPalin = new boolean[n][n];
    for (int i = n-1; i >= 0; i--)
        for (int j = i; j < n; j++)
            isPalin[i][j] = s.charAt(i) == s.charAt(j) &&
                            (j - i <= 2 || isPalin[i+1][j-1]);

    int[] dp = new int[n];
    Arrays.fill(dp, Integer.MAX_VALUE);

    for (int i = 0; i < n; i++) {
        if (isPalin[0][i]) { dp[i] = 0; continue; }  // Whole prefix is palindrome
        for (int j = 1; j <= i; j++)
            if (isPalin[j][i])
                dp[i] = Math.min(dp[i], dp[j-1] + 1);
    }
    return dp[n-1];
}
```
**Time:** O(N²) | **Space:** O(N²)

---
## 🎯 Summary
- Precompute all palindrome substrings in O(N²)
- dp[i] = min cuts for prefix ending at i
- If entire prefix palindrome: 0 cuts; else try all palindromic suffixes
