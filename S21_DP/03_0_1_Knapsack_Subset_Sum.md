# 03. 0/1 Knapsack & Subset Sum

**LeetCode (Subset Sum = Partition Equal):** https://leetcode.com/problems/partition-equal-subset-sum/
**GFG Knapsack:** https://practice.geeksforgeeks.org/problems/0-1-knapsack-problem/0
**Difficulty:** 🔴 Hard | **Pattern:** 2D DP / Subset DP

---

## 🧠 Intuition

**0/1 Knapsack:**
Given weights[] and values[], knapsack capacity W. Maximize value by picking items (each at most once).

**Subset Sum:**
Given array, can we find a subset that sums to target? (Simpler version of Knapsack)

**Core Idea — "Include or Exclude":**
For each item i at capacity j:
- EXCLUDE: dp[i][j] = dp[i-1][j] (don't take item i)
- INCLUDE: dp[i][j] = dp[i-1][j - weight[i]] + value[i] (take item i, if it fits)
- dp[i][j] = max(exclude, include)

---

## 🚀 Knapsack Solution

```java
public class Knapsack {

    // 2D DP
    public static int knapsack(int[] weights, int[] values, int W) {
        int n = weights.length;
        int[][] dp = new int[n + 1][W + 1];

        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= W; j++) {
                // Exclude item i
                dp[i][j] = dp[i-1][j];

                // Include item i (if it fits)
                if (weights[i-1] <= j) {
                    dp[i][j] = Math.max(dp[i][j],
                        dp[i-1][j - weights[i-1]] + values[i-1]);
                }
            }
        }

        return dp[n][W];
    }

    // SPACE OPTIMIZED: 1D DP (traverse j right-to-left!)
    public static int knapsack1D(int[] weights, int[] values, int W) {
        int n = weights.length;
        int[] dp = new int[W + 1];

        for (int i = 0; i < n; i++) {
            // Traverse RIGHT-TO-LEFT to avoid using same item twice!
            for (int j = W; j >= weights[i]; j--) {
                dp[j] = Math.max(dp[j], dp[j - weights[i]] + values[i]);
            }
        }

        return dp[W];
    }

    public static void main(String[] args) {
        int[] weights = {1, 3, 4, 5};
        int[] values  = {1, 4, 5, 7};
        System.out.println(knapsack(weights, values, 7));   // 9
        System.out.println(knapsack1D(weights, values, 7)); // 9
    }
}
```

**Subset Sum Solution:**

```java
public class SubsetSum {
    // Can we find subset summing to target?
    public static boolean subsetSum(int[] nums, int target) {
        int n = nums.length;
        boolean[] dp = new boolean[target + 1];
        dp[0] = true;  // Empty subset sums to 0

        for (int num : nums) {
            // Right-to-left to avoid reusing elements
            for (int j = target; j >= num; j--) {
                dp[j] = dp[j] || dp[j - num];
            }
        }

        return dp[target];
    }

    // Partition Equal Subset Sum (LeetCode 416)
    public static boolean canPartition(int[] nums) {
        int total = 0;
        for (int n : nums) total += n;

        if (total % 2 != 0) return false;  // Odd sum → impossible

        return subsetSum(nums, total / 2);
    }

    public static void main(String[] args) {
        System.out.println(canPartition(new int[]{1,5,11,5})); // true (1+5+5=11)
        System.out.println(canPartition(new int[]{1,2,3,5}));  // false
    }
}
```

**Dry Run — Knapsack 1D:** weights=[1,3], values=[1,4], W=4
```
Initial dp = [0,0,0,0,0]

Item 0 (w=1, v=1): j from 4 to 1
  j=4: dp[4] = max(0, dp[3]+1) = 1
  j=3: dp[3] = max(0, dp[2]+1) = 1
  j=2: dp[2] = max(0, dp[1]+1) = 1
  j=1: dp[1] = max(0, dp[0]+1) = 1
  dp = [0,1,1,1,1]

Item 1 (w=3, v=4): j from 4 to 3
  j=4: dp[4] = max(1, dp[1]+4) = max(1, 5) = 5
  j=3: dp[3] = max(1, dp[0]+4) = max(1, 4) = 4
  dp = [0,1,1,4,5]

dp[4] = 5 → Take item 1 (w=3,v=4) + item 0 (w=1,v=1) = weight 4, value 5 ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Include/Exclude DP on items
- 0/1 Knapsack: each item used at most once → right-to-left 1D
- Unbounded Knapsack: each item unlimited → left-to-right 1D
- Subset Sum: special case (values = weights = nums)

---

## ⚠️ Key Insight: Right-to-Left vs Left-to-Right

```
0/1 Knapsack (each item ONCE):
  → Traverse j RIGHT-TO-LEFT
  → Ensures we don't pick same item twice

Unbounded Knapsack (each item MANY times):
  → Traverse j LEFT-TO-RIGHT
  → Allows reusing same item
```

---

## 🎯 Final Summary

- 0/1 Knapsack: dp[j] = max(dp[j], dp[j-w]+v) traversed right-to-left
- Subset Sum: dp[j] = dp[j] || dp[j-num] traversed right-to-left
- Partition Equal Subset: find subset summing to total/2
- 2D DP: rows=items, cols=capacity. Include/exclude decision at each cell
- Space optimize: 1D array, direction depends on problem type!
