# 03. Max Consecutive Ones III (Flip at most K zeros)

**LeetCode:** https://leetcode.com/problems/max-consecutive-ones-iii/
**Difficulty:** 🟡 Medium | **Pattern:** Sliding Window

---

## 🧠 Intuition

**In Simple Words:**
Binary array. Can flip at most K zeros to ones. Find max consecutive ones.
[1,1,1,0,0,0,1,1,1,1,0], K=2 → answer=6 (flip positions 9,10 or 3,4)

**Core Idea:**
Sliding window: maintain window with at most K zeros.
Expand right. When zeros > K, shrink from left (skip until zeros ≤ K).

---

## 🚀 Solution

```java
public class MaxConsecutiveOnesIII {
    public static int longestOnes(int[] nums, int k) {
        int left = 0, zeros = 0, maxLen = 0;

        for (int right = 0; right < nums.length; right++) {
            if (nums[right] == 0) zeros++;  // Expand: add zero

            // Too many zeros → shrink window
            while (zeros > k) {
                if (nums[left] == 0) zeros--;
                left++;
            }

            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen;
    }

    public static void main(String[] args) {
        System.out.println(longestOnes(new int[]{1,1,1,0,0,0,1,1,1,1,0}, 2)); // 6
        System.out.println(longestOnes(new int[]{0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1,0}, 3)); // 10
    }
}
```

**Time:** O(N) | **Space:** O(1)

---

## 🎯 Final Summary

- Sliding window: expand right, when zeros > K, shrink left
- Track count of zeros in window
- maxLen = max window size with ≤ K zeros
- O(N) time, O(1) space — clean one-pass solution
