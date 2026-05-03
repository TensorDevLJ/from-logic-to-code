# 04. Longest Common Subsequence & Edit Distance

**LeetCode LCS:** https://leetcode.com/problems/longest-common-subsequence/
**LeetCode Edit Distance:** https://leetcode.com/problems/edit-distance/
**Difficulty:** 🔴 Hard | **Pattern:** 2D DP on Strings

---

## 🧠 Intuition

**LCS (Longest Common Subsequence):**
Length of longest subsequence present in BOTH strings.
"abcde" and "ace" → LCS = "ace" = length 3

**Edit Distance (Levenshtein):**
Minimum operations (insert, delete, replace) to convert word1 to word2.
"horse" → "ros": replace h→r, delete r, delete e → 3 operations

**Core Idea for LCS:**
- If chars match: dp[i][j] = 1 + dp[i-1][j-1]
- If not match: dp[i][j] = max(dp[i-1][j], dp[i][j-1])

**Core Idea for Edit Distance:**
- If chars match: dp[i][j] = dp[i-1][j-1] (no operation needed)
- If not match: dp[i][j] = 1 + min(insert: dp[i][j-1], delete: dp[i-1][j], replace: dp[i-1][j-1])

---

## 🚀 Solutions

```java
public class DPOnStrings {

    // LONGEST COMMON SUBSEQUENCE
    public static int lcs(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (s1.charAt(i-1) == s2.charAt(j-1)) {
                    dp[i][j] = 1 + dp[i-1][j-1];  // Characters match!
                } else {
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]); // Try excluding one
                }
            }
        }

        return dp[m][n];
    }

    // EDIT DISTANCE
    public static int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] dp = new int[m + 1][n + 1];

        // Base cases: converting to/from empty string
        for (int i = 0; i <= m; i++) dp[i][0] = i;  // Delete i chars
        for (int j = 0; j <= n; j++) dp[0][j] = j;  // Insert j chars

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i-1) == word2.charAt(j-1)) {
                    dp[i][j] = dp[i-1][j-1];  // No operation needed
                } else {
                    dp[i][j] = 1 + Math.min(
                        dp[i-1][j-1],  // Replace
                        Math.min(
                            dp[i][j-1],   // Insert
                            dp[i-1][j]    // Delete
                        )
                    );
                }
            }
        }

        return dp[m][n];
    }

    // LONGEST PALINDROMIC SUBSEQUENCE
    // = LCS(s, reverse(s))
    public static int longestPalindromeSubseq(String s) {
        return lcs(s, new StringBuilder(s).reverse().toString());
    }

    public static void main(String[] args) {
        System.out.println(lcs("abcde", "ace"));        // 3
        System.out.println(lcs("abc", "abc"));           // 3
        System.out.println(minDistance("horse", "ros")); // 3
        System.out.println(minDistance("", "abc"));      // 3
        System.out.println(longestPalindromeSubseq("bbbab")); // 4 (bbbb)
    }
}
```

**LCS Dry Run:** s1="abc", s2="ac"
```
    ""  a   c
""  [0] [0] [0]
a   [0] [1] [1]   a==a → dp[1][1]=1+dp[0][0]=1; c≠a → max(dp[0][2],dp[1][1])=max(0,1)=1
b   [0] [1] [1]   a≠b → max(dp[1][1],dp[2][0])=1; c≠b → max(dp[1][2],dp[2][1])=1
c   [0] [1] [2]   a≠c → max(dp[2][1],dp[3][0])=1; c==c → dp[2][1]+1=1+1=2

LCS = dp[3][2] = 2 ✅ ("ac")
```

---

## 🔁 Pattern Recognition

**2D DP on Strings — Template:**
- i = index in string 1, j = index in string 2
- Matching: diagonal move + some value
- Not matching: take best of excluding from string1 or string2

**Family of problems:** LCS, LIS via patience sorting, Edit Distance, Shortest Common Supersequence, Distinct Subsequences, Wildcard Matching

---

## ⚠️ Common Mistakes

1. ❌ 0-indexed dp table vs 1-indexed strings — use `s.charAt(i-1)` with dp indices 1..n
2. ❌ Edit distance: wrong base case (empty string conversion)
3. ❌ Missing the `dp[i-1][j-1]` for replace operation

---

## 🎯 Final Summary

- LCS: match → diagonal+1; mismatch → max(up, left)
- Edit Distance: match → diagonal; mismatch → 1+min(replace, insert, delete)
- Longest Palindromic Subsequence = LCS(s, reverse(s))
- Fill table from (1,1) to (m,n), answer at dp[m][n]
- Template for all 2D string DP problems
