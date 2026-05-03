# 01. Two Sum

**LeetCode:** https://leetcode.com/problems/two-sum/
**Difficulty:** 🟢 Easy | **Pattern:** HashMap / Two Pointer

---

## 🧠 Intuition

**In Simple Words:**
Given array and target, find two numbers that sum to target. Return their indices.
[2,7,11,15], target=9 → [0,1] (nums[0]+nums[1] = 2+7 = 9)

**Real-Life Analogy:**
Shopping with $50. You have price tags. Find which two items together cost exactly $50.

**Core Idea:**
For each element X, we need to find target - X.
HashMap: store each element. For each new element, check if (target - element) exists.

---

## 📌 Problem Understanding

- **Input:** int[] nums, int target
- **Output:** int[] of two indices
- **Constraints:** Exactly one solution exists. Same element can't be used twice.

**Example:**
nums = [2,7,11,15], target = 9 → [0,1]
nums = [3,2,4], target = 6 → [1,2]

---

## 🐢 Brute Force: O(N²)

```java
public static int[] twoSumBrute(int[] nums, int target) {
    int n = nums.length;
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[]{i, j};
            }
        }
    }
    return new int[]{};
}
```

**Time:** O(N²) | **Space:** O(1)

---

## 🚀 Optimal: HashMap O(N)

**Idea:** As we scan, store "num → index" in map.
For each num, check if (target - num) already in map.

**How to convert logic → code:**
1. Create HashMap<Integer, Integer>
2. For each index i:
   a. complement = target - nums[i]
   b. If complement in map → return {map.get(complement), i}
   c. Else → put nums[i] → i in map

```java
import java.util.*;

public class TwoSum {
    public static int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();  // value → index

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];  // What we need

            if (map.containsKey(complement)) {
                // Found it! Return both indices
                return new int[]{map.get(complement), i};
            }

            // Store current number and its index
            map.put(nums[i], i);
        }

        return new int[]{};  // No solution (problem guarantees this won't happen)
    }

    public static void main(String[] args) {
        System.out.println(Arrays.toString(twoSum(new int[]{2,7,11,15}, 9)));  // [0,1]
        System.out.println(Arrays.toString(twoSum(new int[]{3,2,4}, 6)));      // [1,2]
    }
}
```

**Time:** O(N) | **Space:** O(N) for HashMap

**Dry Run:** nums=[2,7,11,15], target=9
```
i=0: complement=9-2=7. map={}. 7 not in map. map={2:0}
i=1: complement=9-7=2. map={2:0}. 2 IS in map! Return {map.get(2), 1} = {0,1} ✅
```

**Note:** If array is sorted and you can return VALUES (not indices), use Two Pointer O(1) space:
```java
// Only works when sorted and need values, not indices
int left = 0, right = nums.length - 1;
while (left < right) {
    int sum = nums[left] + nums[right];
    if (sum == target) return new int[]{left, right};
    else if (sum < target) left++;
    else right--;
}
```

---

## 🔁 Pattern Recognition

**Pattern:** HashMap "store and look up complement"
**Same pattern:** Three Sum (sort + two pointer), 4Sum, Two Sum variants

---

## 🧠 How to Think (Interview View)

1. Brute: check all pairs → O(N²)
2. "Can we avoid inner loop?" → Use HashMap to look up complement in O(1)
3. "If sorted?" → Two Pointer O(1) space, but only if indices not required

---

## ⚠️ Common Mistakes

1. ❌ Checking map.containsKey(nums[i]) before map.put (uses same element twice)
2. ❌ Returning values instead of indices
3. ❌ Using array instead of HashMap (O(N) search)

---

## 🧩 Variations

1. **Two Sum II (sorted)** — LeetCode 167: Two Pointer
2. **3Sum** — LeetCode 15: Sort + Two Pointer
3. **4Sum** — LeetCode 18: Sort + Two Pointer + Two Pointer
4. **Two Sum (count pairs)** — return count of valid pairs
5. **Two Sum (BST)** — LeetCode 653

---

## 🧠 Memory Trick

**"Store what you've SEEN. Check if COMPLEMENT was seen before."**
map stores: "I've seen value X at index i"
Check: "Do I need X? (target - current)"

---

## 🎯 Final Summary

- For each element, check if (target - element) exists in HashMap
- HashMap approach: O(N) time, O(N) space
- Two Pointer (if sorted): O(N) time, O(1) space (but needs sorted array)
- Store INDICES in map (not just values) for this variant
- Most common HashMap problem in interviews — master it!
