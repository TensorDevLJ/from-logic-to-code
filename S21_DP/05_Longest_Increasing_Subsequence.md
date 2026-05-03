# 05. Longest Increasing Subsequence (LIS)

**LeetCode:** https://leetcode.com/problems/longest-increasing-subsequence/
**Difficulty:** 🟡 Medium | **Pattern:** DP / Binary Search

---

## 🧠 Intuition

**In Simple Words:**
Find length of longest subsequence that is strictly increasing.
[10, 9, 2, 5, 3, 7, 101, 18] → [2, 3, 7, 101] → length 4

**Note:** Subsequence = elements in order but NOT necessarily contiguous.

---

## 🐢 DP Approach O(N²)

**Idea:** dp[i] = length of LIS ending at index i.
dp[i] = 1 + max(dp[j]) for all j < i where nums[j] < nums[i]

```java
public static int lisDP(int[] nums) {
    int n = nums.length;
    int[] dp = new int[n];
    Arrays.fill(dp, 1);  // Each element is LIS of length 1

    int maxLIS = 1;

    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {  // Can extend with nums[i]
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        maxLIS = Math.max(maxLIS, dp[i]);
    }

    return maxLIS;
}
```

**Dry Run:** [10,9,2,5,3,7,101,18]
```
dp = [1,1,1,1,1,1,1,1]

i=1 (9): j=0 (10>9, skip). dp[1]=1
i=2 (2): j=0,1 all >2, skip. dp[2]=1
i=3 (5): j=2 (2<5): dp[3]=max(1,1+1)=2. dp[3]=2
i=4 (3): j=2 (2<3): dp[4]=max(1,1+1)=2. dp[4]=2
i=5 (7): j=2(2<7):dp=2, j=3(5<7):dp=3, j=4(3<7):dp=3. dp[5]=3
i=6 (101): all <101. dp[6]=max of all+1=4. dp[6]=4
i=7 (18): j=2(dp=2), j=3(dp=3), j=4(dp=3), j=5(dp=4) → dp[7]=4

maxLIS=4 ✅
```

---

## 🚀 Optimal: Binary Search O(N log N)

**Idea:** Maintain a `tails` array where `tails[i]` = smallest tail element of all increasing subsequences of length i+1.

```java
public static int lisBinarySearch(int[] nums) {
    int[] tails = new int[nums.length];
    int size = 0;

    for (int num : nums) {
        // Binary search: find position where num can replace or extend
        int lo = 0, hi = size;
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            if (tails[mid] < num) lo = mid + 1;
            else hi = mid;
        }

        tails[lo] = num;  // Replace or extend
        if (lo == size) size++;  // Extended LIS
    }

    return size;
}
```

**Time:** O(N log N) | **Space:** O(N)

**Dry Run:** [10,9,2,5,3,7,101,18]
```
num=10: tails=[] → [10], size=1
num=9: BS finds pos 0 (9<10) → replace tails[0]=9. tails=[9], size=1
num=2: BS finds pos 0 (2<9) → replace tails[0]=2. tails=[2], size=1
num=5: BS finds pos 1 (5>2) → extend. tails=[2,5], size=2
num=3: BS finds pos 1 (3<5) → replace tails[1]=3. tails=[2,3], size=2
num=7: BS finds pos 2 (7>3) → extend. tails=[2,3,7], size=3
num=101: extend. tails=[2,3,7,101], size=4
num=18: BS finds pos 3 (18<101) → replace. tails=[2,3,7,18], size=4

Size = 4 ✅
```

---

## 🔁 Pattern Recognition

- O(N²) DP: classic DP, easy to understand
- O(N log N): Patience sorting / binary search on tails array

---

## 🎯 Final Summary

- LIS DP: dp[i] = 1 + max(dp[j]) for j<i where nums[j]<nums[i]
- O(N²) DP with nested loops
- O(N log N): maintain tails array, binary search for position
- tails not the actual subsequence — just tracks smallest possible tails
- Similar: Longest Bitonic Subsequence, Largest Divisible Subset
