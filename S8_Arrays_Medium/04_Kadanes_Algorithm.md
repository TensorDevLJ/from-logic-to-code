# 04. Kadane's Algorithm — Maximum Subarray Sum

**LeetCode:** https://leetcode.com/problems/maximum-subarray/
**Difficulty:** 🟡 Medium | **Pattern:** Dynamic Programming / Greedy

---

## 🧠 Intuition

**In Simple Words:**
Find contiguous subarray with the largest sum.
[-2,1,-3,4,-1,2,1,-5,4] → subarray [4,-1,2,1] = sum 6 (maximum)

**Real-Life Analogy:**
Stock market: you track daily profit/loss. Find the period (consecutive days) where your total profit was maximum. If carrying a "negative balance" into tomorrow hurts you, start fresh!

**Core Insight (Greedy):**
As we extend a subarray, if current sum becomes negative → that subarray is "poisonous."
Any future element would be better off starting fresh (without the negative baggage).

**Formal:**
At each position i: currentSum = max(nums[i], currentSum + nums[i])
→ "Start fresh OR extend previous subarray — whichever is better."

---

## 📌 Problem Understanding

- **Input:** int[] nums (can have negatives)
- **Output:** Maximum subarray sum
- **Constraint:** Subarray must have at least 1 element

| Input | Output | Subarray |
|-------|--------|---------|
| [-2,1,-3,4,-1,2,1,-5,4] | 6 | [4,-1,2,1] |
| [1] | 1 | [1] |
| [-1,-2,-3] | -1 | [-1] |
| [5,4,-1,7,8] | 23 | [5,4,-1,7,8] |

---

## 🐢 Brute Force: O(N²) or O(N³)

```java
// O(N^2): check all subarrays
public static int maxSubarrayBrute(int[] nums) {
    int maxSum = Integer.MIN_VALUE;
    for (int i = 0; i < nums.length; i++) {
        int sum = 0;
        for (int j = i; j < nums.length; j++) {
            sum += nums[j];
            maxSum = Math.max(maxSum, sum);
        }
    }
    return maxSum;
}
```

**Time:** O(N²) | **Space:** O(1)

---

## 🚀 Optimal: Kadane's Algorithm O(N)

```java
public class KadanesAlgorithm {
    public static int maxSubArray(int[] nums) {
        int currentSum = nums[0];  // Sum of current subarray
        int maxSum = nums[0];      // Global maximum

        for (int i = 1; i < nums.length; i++) {
            // Decision: extend current subarray OR start fresh?
            currentSum = Math.max(nums[i], currentSum + nums[i]);
            
            // Update global maximum
            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
    }

    // BONUS: Also return the subarray itself
    public static int[] maxSubArrayWithIndices(int[] nums) {
        int currentSum = nums[0];
        int maxSum = nums[0];
        int start = 0, end = 0, tempStart = 0;

        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > currentSum + nums[i]) {
                currentSum = nums[i];
                tempStart = i;  // Starting fresh
            } else {
                currentSum = currentSum + nums[i];
            }

            if (currentSum > maxSum) {
                maxSum = currentSum;
                start = tempStart;
                end = i;
            }
        }

        System.out.println("Subarray: " + Arrays.toString(Arrays.copyOfRange(nums, start, end+1)));
        return new int[]{maxSum, start, end};
    }

    public static void main(String[] args) {
        System.out.println(maxSubArray(new int[]{-2,1,-3,4,-1,2,1,-5,4})); // 6
        System.out.println(maxSubArray(new int[]{-1,-2,-3}));               // -1
        maxSubArrayWithIndices(new int[]{-2,1,-3,4,-1,2,1,-5,4});           // [4,-1,2,1]
    }
}
```

**Time:** O(N) | **Space:** O(1)

**Dry Run:** [-2,1,-3,4,-1,2,1,-5,4]
```
Init: curr=-2, max=-2

i=1: nums[1]=1. max(1, -2+1=-1) = 1. curr=1, max=max(-2,1)=1
i=2: nums[2]=-3. max(-3, 1-3=-2) = -2. curr=-2, max=max(1,-2)=1
i=3: nums[3]=4. max(4, -2+4=2) = 4. curr=4, max=max(1,4)=4
i=4: nums[4]=-1. max(-1, 4-1=3) = 3. curr=3, max=max(4,3)=4
i=5: nums[5]=2. max(2, 3+2=5) = 5. curr=5, max=max(4,5)=5
i=6: nums[6]=1. max(1, 5+1=6) = 6. curr=6, max=max(5,6)=6
i=7: nums[7]=-5. max(-5, 6-5=1) = 1. curr=1, max=max(6,1)=6
i=8: nums[8]=4. max(4, 1+4=5) = 5. curr=5, max=max(6,5)=6

Final max = 6 ✅ (subarray [4,-1,2,1])
```

---

## 🔁 Pattern Recognition

**Pattern:** Dynamic Programming (1D) / Greedy
- At each step: local optimal choice
- dp[i] = max(nums[i], dp[i-1] + nums[i])

**Same pattern used in:**
- Maximum product subarray (keep track of min and max)
- Circular subarray max sum

---

## 🧠 How to Think (Interview View)

1. Brute: check all subarrays O(N²)
2. "Can I decide at each element whether to extend or restart?"
3. "Negative currentSum is a burden — restart!" → O(N) Kadane's

**Key question interviewers ask:** "What if all elements are negative?" → Return the maximum single element (handled by initializing with nums[0])

---

## ⚠️ Common Mistakes

1. ❌ Initializing currentSum/maxSum to 0 (fails for all-negative arrays!)
2. ❌ Wrong condition: `currentSum < 0` reset — should be `nums[i] > currentSum + nums[i]`
3. ❌ Not tracking indices for returning the actual subarray

---

## 🧩 Variations

1. **Maximum Product Subarray** — LeetCode 152: track min and max (negative×negative=positive!)
2. **Circular Subarray Maximum Sum** — LeetCode 918: maxSum = max(kadane, total - minKadane)
3. **Print Maximum Subarray** — track start and end indices
4. **At Least K Sum** — modified Kadane with constraint

---

## 🧠 Memory Trick

**"If carrying negative weight, drop it and START FRESH!"**
curr = max(curr + nums[i], nums[i])
"Is it better to start anew or carry forward?"

---

## 🎯 Final Summary

- Kadane: currentSum = max(nums[i], currentSum + nums[i])
- Greedy insight: negative running sum → start fresh
- O(N) time, O(1) space — optimal
- Initialize with nums[0] NOT 0 (handles all-negative arrays)
- Extended: track indices to return actual subarray
