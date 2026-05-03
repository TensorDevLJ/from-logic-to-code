# 01. Introduction to DP + Climbing Stairs

**LeetCode:** https://leetcode.com/problems/climbing-stairs/
**Difficulty:** 🟡 Medium | **Pattern:** 1D DP (Fibonacci-style)

---

## 🧠 What is Dynamic Programming?

**Core Idea:**
Break a problem into OVERLAPPING subproblems. Solve each subproblem ONCE and store the result. Use stored result when same subproblem appears again.

**Two properties needed for DP:**
1. **Optimal Substructure:** Optimal solution built from optimal solutions of subproblems
2. **Overlapping Subproblems:** Same subproblems solved multiple times

**Two Approaches:**
- **Top-Down (Memoization):** Recursive + cache (HashMap or array)
- **Bottom-Up (Tabulation):** Iterative, fill table from base cases

---

## 🏃 Climbing Stairs

**Problem:** To climb N stairs, you can take 1 or 2 steps at a time. How many distinct ways?

**Intuition:**
To reach stair N: you came from stair N-1 (took 1 step) OR stair N-2 (took 2 steps).
So: ways(N) = ways(N-1) + ways(N-2) → Fibonacci!

---

## 🚀 Solutions

```java
public class ClimbingStairs {

    // TOP-DOWN: Memoization
    public static int climbMemo(int n, int[] memo) {
        if (n <= 1) return 1;      // Base cases: 1 way to reach 0 or 1
        if (memo[n] != 0) return memo[n];  // Already computed
        memo[n] = climbMemo(n-1, memo) + climbMemo(n-2, memo);
        return memo[n];
    }

    // BOTTOM-UP: Tabulation
    public static int climbDP(int n) {
        if (n <= 1) return 1;

        int[] dp = new int[n + 1];
        dp[0] = 1;  // 1 way to stay at ground (do nothing)
        dp[1] = 1;  // 1 way to reach stair 1 (take 1 step)

        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i-1] + dp[i-2];  // From N-1 or N-2
        }

        return dp[n];
    }

    // SPACE OPTIMIZED: O(1) space
    public static int climbOptimal(int n) {
        if (n <= 1) return 1;

        int prev2 = 1, prev1 = 1;  // dp[0], dp[1]

        for (int i = 2; i <= n; i++) {
            int curr = prev1 + prev2;
            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }

    public static void main(String[] args) {
        System.out.println(climbOptimal(5));  // 8
        System.out.println(climbOptimal(10)); // 89
    }
}
```

**Time:** O(N) | **Space:** O(N) for DP table, O(1) for optimized

**dp table for N=5:**
```
i:  0  1  2  3  4  5
dp: 1  1  2  3  5  8

dp[2] = dp[1]+dp[0] = 1+1 = 2
dp[3] = dp[2]+dp[1] = 2+1 = 3
dp[4] = dp[3]+dp[2] = 3+2 = 5
dp[5] = dp[4]+dp[3] = 5+3 = 8 ✅
```

---

## 🎯 Final Summary

- DP = Memoize overlapping subproblems
- Climbing stairs = Fibonacci: dp[i] = dp[i-1] + dp[i-2]
- Base: dp[0]=1, dp[1]=1
- Space optimize: only keep last 2 values
- This exact pattern: Frog Jump, House Robber, Min Cost Climbing
