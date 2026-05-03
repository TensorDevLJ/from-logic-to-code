# 06. Next Permutation

**LeetCode:** https://leetcode.com/problems/next-permutation/
**Difficulty:** 🟡 Medium | **Pattern:** Array / In-place

---

## 🧠 Intuition

**In Simple Words:**
Find the next lexicographically greater permutation of the array.
[1,2,3] → [1,3,2] → [2,1,3] → [2,3,1] → [3,1,2] → [3,2,1] → [1,2,3] (wraps)

**Real-Life Analogy:**
Imagine all arrangements of digits sorted in a dictionary. Next permutation = next entry in that dictionary.

**Key Observation:**
- [1,2,3] → next [1,3,2] (swap 2 and 3)
- [3,2,1] → all decreasing → no next → wrap to [1,2,3]
- The longer the decreasing suffix, the farther back we need to go

**Algorithm (3 steps):**
1. **Find the break point:** Find largest index i where arr[i] < arr[i+1] (first descent from right)
2. **Find the swap partner:** Find largest index j > i where arr[j] > arr[i] (just larger element)
3. **Swap and reverse:** Swap arr[i] and arr[j], then reverse arr[i+1..n-1]

---

## 🚀 Solution

```java
public class NextPermutation {
    public static void nextPermutation(int[] nums) {
        int n = nums.length;

        // Step 1: Find the rightmost "dip" — where nums[i] < nums[i+1]
        int i = n - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;  // Move left until we find dip
        }

        if (i >= 0) {  // Found a dip (not fully descending)
            // Step 2: Find the element just larger than nums[i] from right
            int j = n - 1;
            while (nums[j] <= nums[i]) {
                j--;  // Find first element > nums[i] from right
            }

            // Step 3a: Swap nums[i] and nums[j]
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }

        // Step 3b: Reverse suffix after i (make it smallest permutation)
        int left = i + 1, right = n - 1;
        while (left < right) {
            int temp = nums[left];
            nums[left++] = nums[right];
            nums[right--] = temp;
        }
    }

    public static void main(String[] args) {
        int[] arr1 = {1, 2, 3};
        nextPermutation(arr1);
        System.out.println(Arrays.toString(arr1)); // [1, 3, 2]

        int[] arr2 = {3, 2, 1};
        nextPermutation(arr2);
        System.out.println(Arrays.toString(arr2)); // [1, 2, 3]

        int[] arr3 = {1, 1, 5};
        nextPermutation(arr3);
        System.out.println(Arrays.toString(arr3)); // [1, 5, 1]
    }
}
```

**Time:** O(N) | **Space:** O(1)

**Dry Run:** [1, 3, 5, 4, 2]
```
Step 1: Find dip from right
  i=3: nums[3]=4 >= nums[4]=2 → skip
  i=2: nums[2]=5 >= nums[3]=4 → skip
  i=1: nums[1]=3 < nums[2]=5 → FOUND! i=1

Step 2: Find element just greater than nums[1]=3 from right
  j=4: nums[4]=2 <= 3 → skip
  j=3: nums[3]=4 > 3 → FOUND! j=3

Step 3a: Swap nums[1] and nums[3]
  [1, 3, 5, 4, 2] → [1, 4, 5, 3, 2]

Step 3b: Reverse from i+1=2 to end
  Reverse [5, 3, 2] → [2, 3, 5]
  Result: [1, 4, 2, 3, 5] ✅

Verify: After [1,3,5,4,2], next permutation IS [1,4,2,3,5] ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Array manipulation, suffix understanding
The suffix after the dip is ALWAYS in descending order — reversing makes it ascending (smallest possible)

---

## 🧠 How to Think (Interview View)

1. "Next permutation" → know the 3-step algorithm
2. Why reverse? The suffix was descending → reversing = ascending = smallest arrangement
3. Why find just-larger element? We want the NEXT permutation, not skip several

---

## ⚠️ Common Mistakes

1. ❌ Forgetting to handle fully descending case (i goes to -1)
2. ❌ Finding first j > i instead of rightmost j
3. ❌ Not reversing suffix (or reversing wrong range)

---

## 🎯 Final Summary

- Find rightmost dip i (nums[i] < nums[i+1] from right)
- Find rightmost element j > i where nums[j] > nums[i]
- Swap nums[i] and nums[j], then reverse suffix from i+1 to end
- Fully descending? → reverse entire array (gives smallest)
- O(N) time, O(1) space
