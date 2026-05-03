# 02. House Robber (Max Sum of Non-Adjacent Elements)

**LeetCode:** https://leetcode.com/problems/house-robber/
**Difficulty:** 🟡 Medium | **Pattern:** 1D DP

---

## 🧠 Intuition

**In Simple Words:**
Array of house values. Can't rob two adjacent houses. Maximize total robbed.
[2,7,9,3,1] → rob houses 0,2,4: 2+9+1=12 OR 0,2: 2+9=11 → answer: 12

**Core Insight:**
For house i: either rob it (get nums[i] + best from i-2) OR skip it (take best from i-1).
dp[i] = max(nums[i] + dp[i-2], dp[i-1])

---

## 🚀 Solution

```java
public class HouseRobber {
    // Space Optimized O(1)
    public static int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];

        int prev2 = 0;          // dp[i-2]
        int prev1 = nums[0];    // dp[i-1] = best up to house 0

        for (int i = 1; i < n; i++) {
            // Option 1: Rob current (nums[i]) + skip to i-2
            // Option 2: Skip current, take prev1
            int curr = Math.max(nums[i] + prev2, prev1);
            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }

    public static void main(String[] args) {
        System.out.println(rob(new int[]{2,7,9,3,1})); // 12 (rob 0,2,4)
        System.out.println(rob(new int[]{1,2,3,1}));   // 4  (rob 0,2: 1+3=4)
    }
}
```

**Dry Run:** [2,7,9,3,1]
```
prev2=0, prev1=2

i=1 (val=7): curr=max(7+0, 2)=7. prev2=2, prev1=7
i=2 (val=9): curr=max(9+2, 7)=11. prev2=7, prev1=11
i=3 (val=3): curr=max(3+7, 11)=11. prev2=11, prev1=11
i=4 (val=1): curr=max(1+11, 11)=12. prev2=11, prev1=12

Return 12 ✅
```

---

## 🎯 Final Summary

- dp[i] = max(rob: nums[i] + dp[i-2], skip: dp[i-1])
- Initialize: dp[0] = nums[0], dp[1] = max(nums[0], nums[1])
- O(N) time, O(1) space with two variables
