# 468: Count Palindromic Subsequences (LeetCode 730)

**LeetCode:** https://leetcode.com/problems/count-different-palindromic-subsequences/
**Difficulty:** 🟡 Medium (Sheet), 🔴 Hard (LeetCode) | **Q# in sheet: 468**

---
## 🧠 Intuition
Count distinct palindromic subsequences in string s.

**DP Approach:** dp[i][j] = count of distinct palindromic subsequences in s[i..j].

**Recurrence:**
- If s[i] != s[j]: dp[i][j] = dp[i+1][j] + dp[i][j-1] - dp[i+1][j-1] (inclusion-exclusion)
- If s[i] == s[j]:
  - Find left = first occurrence of s[i] after i
  - Find right = last occurrence of s[j] before j
  - If left > right: dp[i][j] = dp[i+1][j-1] * 2 + 2  (no s[i] inside)
  - If left == right: dp[i][j] = dp[i+1][j-1] * 2 + 1  (one s[i] inside)
  - If left < right: dp[i][j] = dp[i+1][j-1] * 2 - dp[left+1][right-1]  (duplicates)

```java
public int countPalindromicSubsequences(String s) {
    int n = s.length();
    long MOD = 1_000_000_007L;
    long[][] dp = new long[n][n];

    for (int i = 0; i < n; i++) dp[i][i] = 1;

    for (int len = 2; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            if (s.charAt(i) == s.charAt(j)) {
                int left = i + 1, right = j - 1;
                while (left <= right && s.charAt(left) != s.charAt(i)) left++;
                while (right >= left && s.charAt(right) != s.charAt(j)) right--;

                if (left > right) {
                    dp[i][j] = dp[i+1][j-1] * 2 + 2;
                } else if (left == right) {
                    dp[i][j] = dp[i+1][j-1] * 2 + 1;
                } else {
                    dp[i][j] = dp[i+1][j-1] * 2 - dp[left+1][right-1];
                }
            } else {
                dp[i][j] = dp[i+1][j] + dp[i][j-1] - dp[i+1][j-1];
            }
            dp[i][j] = (dp[i][j] % MOD + MOD) % MOD;  // Handle negatives
        }
    }
    return (int) dp[0][n-1];
}
```
**Time:** O(N²) | **Space:** O(N²)

---
## 🎯 Summary
- 2D DP: dp[i][j] = distinct palindromic subsequences in s[i..j]
- When ends match: 3 cases based on next/prev occurrence of same char
- When ends don't match: inclusion-exclusion
- Apply MOD at each step, add MOD before % to handle negatives
