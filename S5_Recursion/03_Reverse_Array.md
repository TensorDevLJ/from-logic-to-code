# 03. Reverse an Array

**LeetCode:** https://leetcode.com/problems/reverse-string/ (similar)
**GFG:** https://practice.geeksforgeeks.org/problems/reverse-an-array/0
**Difficulty:** 🟢 Easy | **Pattern:** Recursion / Two Pointer

---

## 🧠 Intuition

**In Simple Words:**
Given [1,2,3,4,5], return [5,4,3,2,1].

**Recursive Idea:**
"To reverse an array: swap first and last, then recursively reverse the middle!"

---

## 📌 Problem Understanding

- Input: int[] arr
- Output: same array reversed in-place

---

## 🐢 Iterative (Two Pointer)

```java
public static void reverseIterative(int[] arr) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
        // Swap arr[left] and arr[right]
        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
        left++;
        right--;
    }
}
```

**Time:** O(N) | **Space:** O(1)

---

## 🚀 Recursive Approach

```java
public class ReverseArray {
    public static void reverse(int[] arr, int left, int right) {
        // Base case: pointers crossed or met
        if (left >= right) return;

        // Swap
        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;

        // Recurse on inner subarray
        reverse(arr, left + 1, right - 1);
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        reverse(arr, 0, arr.length - 1);
        System.out.println(Arrays.toString(arr)); // [5, 4, 3, 2, 1]
    }
}
```

**Time:** O(N) | **Space:** O(N/2) = O(N) recursive stack

**Dry Run:** [1,2,3,4,5], left=0, right=4
```
swap(arr[0],arr[4]) → [5,2,3,4,1], recurse(1,3)
swap(arr[1],arr[3]) → [5,4,3,2,1], recurse(2,2)
left(2) >= right(2) → BASE CASE, return
Final: [5,4,3,2,1] ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Two-pointer + Recursion
**Used in:** Palindrome check, reverse LL, reverse string

---

## ⚠️ Common Mistakes

1. ❌ Base case: `left > right` misses even-length arrays
2. ❌ Modifying arr without in-place requirement
3. ❌ Off by one: should be `left >= right` not `left == right`

---

## 🎯 Final Summary

- Two pointer: swap from both ends moving inward
- Recursive: swap ends, recurse on inner subarray
- Base case: left >= right (pointers crossed or same)
- Both O(N) time, recursive uses O(N) stack space
- Pattern foundation for palindrome checks!
