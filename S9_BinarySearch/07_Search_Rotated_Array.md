# 07. Search in Rotated Sorted Array

**LeetCode:** https://leetcode.com/problems/search-in-rotated-sorted-array/
**Difficulty:** 🟡 Medium | **Pattern:** Modified Binary Search

---

## 🧠 Intuition

**In Simple Words:**
Array was sorted then rotated at some pivot.
[4,5,6,7,0,1,2] — rotated sorted array. Find if target exists.

**Key Insight:**
Even in a rotated array, when you look at mid:
- Either the LEFT half is sorted, OR the RIGHT half is sorted (always one of them!)
Use this to determine which half the target lies in.

**Decision Logic:**
1. If arr[left] <= arr[mid]: LEFT HALF IS SORTED
   - If target in [arr[left], arr[mid]): search left
   - Else: search right
2. Else: RIGHT HALF IS SORTED
   - If target in (arr[mid], arr[right]]: search right
   - Else: search left

---

## 🚀 Solution

```java
public class SearchRotatedArray {
    public static int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (nums[mid] == target) return mid;

            // LEFT HALF IS SORTED
            if (nums[low] <= nums[mid]) {
                if (target >= nums[low] && target < nums[mid]) {
                    high = mid - 1;  // Target in left sorted half
                } else {
                    low = mid + 1;   // Target in right half
                }
            }
            // RIGHT HALF IS SORTED
            else {
                if (target > nums[mid] && target <= nums[high]) {
                    low = mid + 1;   // Target in right sorted half
                } else {
                    high = mid - 1;  // Target in left half
                }
            }
        }

        return -1;  // Not found
    }

    public static void main(String[] args) {
        System.out.println(search(new int[]{4,5,6,7,0,1,2}, 0));  // 4
        System.out.println(search(new int[]{4,5,6,7,0,1,2}, 3));  // -1
        System.out.println(search(new int[]{1}, 0));               // -1
    }
}
```

**Time:** O(log N) | **Space:** O(1)

**Dry Run:** [4,5,6,7,0,1,2], target=0
```
low=0, high=6
Iter 1: mid=3, arr[3]=7 ≠ 0
  arr[0]=4 <= arr[3]=7 → LEFT sorted [4,5,6,7]
  target=0: 0 >= 4? NO → go right: low=4

low=4, high=6
Iter 2: mid=5, arr[5]=1 ≠ 0
  arr[4]=0 <= arr[5]=1 → LEFT sorted [0,1]
  target=0: 0 >= 0 AND 0 < 1? YES → go left: high=4

low=4, high=4
Iter 3: mid=4, arr[4]=0 == 0 → return 4 ✅
```

---

## 🔁 Pattern Recognition

- **Pattern:** Modified Binary Search on non-standard sorted arrays
- The KEY: always IDENTIFY which half is guaranteed sorted, THEN decide

---

## ⚠️ Common Mistakes

1. ❌ `nums[low] < nums[mid]` — should be `<=` (handles duplicates issue in variant II)
2. ❌ Wrong range check: `target >= nums[low] && target < nums[mid]` (not <=)
3. ❌ Trying to find the pivot first — not necessary!

---

## 🎯 Final Summary

- Rotated sorted array always has ONE sorted half after mid split
- Check if left sorted: nums[low] <= nums[mid]
- If left sorted: check if target in that range → search left, else right
- If right sorted: check if target in that range → search right, else left
- O(log N) — beautiful binary search on "almost sorted" data
