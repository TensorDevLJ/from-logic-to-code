# 01. Binary Search — Core Concept

**LeetCode:** https://leetcode.com/problems/binary-search/
**Difficulty:** 🟢 Easy | **Pattern:** Binary Search

---

## 🧠 Intuition

**In Simple Words:**
Search for target in SORTED array. Instead of checking every element, eliminate HALF the search space each step.

**Real-Life Analogy:**
Guessing a number 1-100. Someone says "higher" or "lower." You'd guess 50, then 25 or 75, etc. Binary search does exactly this — halves the search space each time.

**Core Idea:**
Compare target with MIDDLE element:
- target == mid → found!
- target < mid → search LEFT half
- target > mid → search RIGHT half

---

## 📌 Problem Understanding

- **Input:** Sorted array, target
- **Output:** Index of target, or -1 if not found
- **Constraint:** Array MUST be sorted

---

## 🚀 Solutions

```java
public class BinarySearch {
    // Iterative (Preferred — no stack overhead)
    public static int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;  // Avoid overflow! Not (low+high)/2

            if (nums[mid] == target) {
                return mid;             // Found!
            } else if (nums[mid] < target) {
                low = mid + 1;          // Target in right half
            } else {
                high = mid - 1;         // Target in left half
            }
        }

        return -1;  // Not found
    }

    // Recursive version
    public static int searchRecursive(int[] nums, int target, int low, int high) {
        if (low > high) return -1;  // Base case: not found

        int mid = low + (high - low) / 2;

        if (nums[mid] == target) return mid;
        if (nums[mid] < target) return searchRecursive(nums, target, mid + 1, high);
        return searchRecursive(nums, target, low, mid - 1);
    }

    public static void main(String[] args) {
        int[] arr = {-1, 0, 3, 5, 9, 12};
        System.out.println(search(arr, 9));   // 4
        System.out.println(search(arr, 2));   // -1
    }
}
```

**Time:** O(log N) | **Space:** O(1) iterative, O(log N) recursive

**Dry Run:** arr=[-1,0,3,5,9,12], target=9
```
low=0, high=5
Iter 1: mid=2, arr[2]=3 < 9 → low=3
Iter 2: mid=4, arr[4]=9 == 9 → return 4 ✅
```

**Dry Run:** target=2
```
low=0, high=5
Iter 1: mid=2, arr[2]=3 > 2 → high=1
Iter 2: mid=0, arr[0]=-1 < 2 → low=1
Iter 3: mid=1, arr[1]=0 < 2 → low=2
low(2) > high(1) → return -1 ✅
```

---

## 🔑 Critical Details

**Why `low + (high - low) / 2` NOT `(low + high) / 2`?**
If low = 2*10^9 and high = 2*10^9, low+high overflows int!
`low + (high - low) / 2` is mathematically identical but safe.

**Why `while (low <= high)` NOT `< high`?**
When low == high, there's still one element to check. Use `<=`.

---

## 🔁 Pattern Recognition

**Pattern:** Binary Search — foundational technique
Binary search works on ANY **monotonic** condition, not just sorted arrays!
This extends to: Search Space problems, Find Minimum, Peak Element...

---

## ⚠️ Common Mistakes

1. ❌ `mid = (low + high) / 2` — integer overflow!
2. ❌ `while (low < high)` — misses last element
3. ❌ `low = mid` or `high = mid` instead of `mid ± 1` → infinite loop
4. ❌ Applying to unsorted array

---

## 🎯 Final Summary

- Works ONLY on sorted data (or monotonic conditions)
- Each iteration eliminates HALF the search space → O(log N)
- Key formula: `mid = low + (high - low) / 2` (overflow-safe)
- Conditions: `==` return, `<` go right, `>` go left
- Foundation of ALL binary search problems in this sheet!
