# 02. Lower Bound & Upper Bound

**LeetCode (Lower Bound):** Find First and Last Position — https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/
**Difficulty:** 🟢 Easy | **Pattern:** Binary Search on Answer

---

## 🧠 Intuition

**Lower Bound:**
Find the FIRST position where arr[i] >= target.
"Smallest index where target could be inserted to keep sorted order."

**Upper Bound:**
Find the FIRST position where arr[i] > target.
"Where to insert target to keep sorted, placing it AFTER all existing targets."

**Real-Life Example:**
arr = [1,2,4,4,4,6], target = 4
Lower Bound: index 2 (first 4)
Upper Bound: index 5 (first element > 4)

---

## 🚀 Solutions

```java
public class Bounds {
    // Lower Bound: first index where arr[i] >= target
    public static int lowerBound(int[] arr, int target) {
        int low = 0, high = arr.length - 1;
        int result = arr.length;  // Default: target would go at end

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] >= target) {
                result = mid;    // Possible answer, but try LEFT for earlier
                high = mid - 1;
            } else {
                low = mid + 1;   // Too small, go right
            }
        }
        return result;
    }

    // Upper Bound: first index where arr[i] > target
    public static int upperBound(int[] arr, int target) {
        int low = 0, high = arr.length - 1;
        int result = arr.length;  // Default

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] > target) {
                result = mid;    // Possible answer, try LEFT
                high = mid - 1;
            } else {
                low = mid + 1;   // arr[mid] <= target, go right
            }
        }
        return result;
    }

    // Using Java built-ins (for reference)
    // Arrays.binarySearch returns index or -(insertion point)-1
    // But lowerBound = upperBound in standard Java

    public static void main(String[] args) {
        int[] arr = {1, 2, 4, 4, 4, 6};

        System.out.println(lowerBound(arr, 4));  // 2 (first 4)
        System.out.println(upperBound(arr, 4));  // 5 (first > 4)

        System.out.println(lowerBound(arr, 3));  // 2 (where 3 would go)
        System.out.println(upperBound(arr, 3));  // 2 (same for missing element)
    }
}
```

**Time:** O(log N) | **Space:** O(1)

**Dry Run — lowerBound([1,2,4,4,4,6], target=4):**
```
low=0, high=5, result=6
mid=2: arr[2]=4 >= 4 → result=2, high=1
mid=0: arr[0]=1 < 4 → low=1
mid=1: arr[1]=2 < 4 → low=2
low(2) > high(1) → STOP. Return result=2 ✅
```

**Dry Run — upperBound([1,2,4,4,4,6], target=4):**
```
low=0, high=5, result=6
mid=2: arr[2]=4, NOT > 4 → low=3
mid=4: arr[4]=4, NOT > 4 → low=5
mid=5: arr[5]=6 > 4 → result=5, high=4
low(5) > high(4) → STOP. Return result=5 ✅
```

---

## 🔁 Applications

| Use Case | Formula |
|----------|---------|
| Count of target in array | upperBound(target) - lowerBound(target) |
| First occurrence | lowerBound(target), verify arr[idx]==target |
| Last occurrence | upperBound(target) - 1, verify arr[idx]==target |
| Search Insert Position | lowerBound(target) |
| Floor (largest ≤ target) | lowerBound(target) - 1 |
| Ceil (smallest ≥ target) | lowerBound(target) |

---

## ⚠️ Common Mistakes

1. ❌ Not initializing result = arr.length (handles "not found" case)
2. ❌ Wrong condition: `>=` vs `>` determines lower vs upper bound
3. ❌ Forgetting to verify arr[result] == target before using

---

## 🎯 Final Summary

- LowerBound: first index where arr[i] >= target (use >=, shrink right)
- UpperBound: first index where arr[i] > target (use >, shrink right)
- Both O(log N), both return arr.length if target not found
- Foundation for: count occurrences, floor/ceil, search insert position
