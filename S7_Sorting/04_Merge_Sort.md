# 04. Merge Sort

**LeetCode:** https://leetcode.com/problems/sort-an-array/
**GFG:** https://practice.geeksforgeeks.org/problems/merge-sort/1
**Difficulty:** 🟡 Medium | **Pattern:** Divide and Conquer

---

## 🧠 Intuition

**In Simple Words:**
Split array in half recursively until single elements. Then merge pairs in sorted order. "Divide and Conquer."

**Real-Life Analogy:**
Sorting a 1000-page document. Split into two halves of 500, sort each half, then merge them by picking smaller page from front of each half.

**Core Idea:**
- **Divide:** Split array into two halves at midpoint
- **Conquer:** Recursively sort each half
- **Merge:** Combine two sorted halves into one sorted array

---

## 🚀 Solution

```java
public class MergeSort {
    public static void mergeSort(int[] arr, int left, int right) {
        if (left >= right) return;  // Base case: single element

        int mid = left + (right - left) / 2;  // Avoid overflow (not (left+right)/2)

        mergeSort(arr, left, mid);      // Sort left half
        mergeSort(arr, mid + 1, right); // Sort right half
        merge(arr, left, mid, right);   // Merge sorted halves
    }

    private static void merge(int[] arr, int left, int mid, int right) {
        // Create temp arrays for both halves
        int n1 = mid - left + 1;
        int n2 = right - mid;

        int[] L = new int[n1];
        int[] R = new int[n2];

        // Copy data
        for (int i = 0; i < n1; i++) L[i] = arr[left + i];
        for (int j = 0; j < n2; j++) R[j] = arr[mid + 1 + j];

        // Merge L and R back into arr
        int i = 0, j = 0, k = left;

        while (i < n1 && j < n2) {
            if (L[i] <= R[j]) {      // Note: <= for stability
                arr[k++] = L[i++];
            } else {
                arr[k++] = R[j++];
            }
        }

        // Copy remaining elements
        while (i < n1) arr[k++] = L[i++];
        while (j < n2) arr[k++] = R[j++];
    }

    public static void main(String[] args) {
        int[] arr = {38, 27, 43, 3, 9, 82, 10};
        mergeSort(arr, 0, arr.length - 1);
        System.out.println(Arrays.toString(arr));
        // [3, 9, 10, 27, 38, 43, 82]
    }
}
```

**Time:** O(N log N) — always!  
**Space:** O(N) — for temporary arrays

**Dry Run:** [38, 27, 43, 3]
```
mergeSort(0,3): mid=1
  mergeSort(0,1): mid=0
    mergeSort(0,0): base case [38]
    mergeSort(1,1): base case [27]
    merge(0,0,1): [38],[27] → compare 38 vs 27 → [27,38]
  mergeSort(2,3): mid=2
    mergeSort(2,2): base case [43]
    mergeSort(3,3): base case [3]
    merge(2,2,3): [43],[3] → compare → [3,43]
  merge(0,1,3): [27,38] and [3,43]
    compare 27 vs 3 → take 3
    compare 27 vs 43 → take 27
    compare 38 vs 43 → take 38
    remaining: take 43
    Result: [3,27,38,43] ✅
```

---

## 🔁 Properties

| Property | Value |
|----------|-------|
| Time (All cases) | O(N log N) |
| Space | O(N) |
| Stable | ✅ Yes |
| In-place | ❌ No |

**Why O(N log N)?**
- log N levels of recursion (splitting in half each time)
- N work at each level (merging all elements)
- Total: N × log N

---

## 🧠 How to Think (Interview View)

1. "Sort" → think merge sort for guaranteed O(N log N)
2. "Count inversions" → merge sort variation! (count when right < left)
3. "Sort linked list" → merge sort is ideal (no random access needed)

**Merge Sort is used for:**
- External sorting (data too large for memory)
- Counting inversions (classic problem)
- Stable sort requirement

---

## ⚠️ Common Mistakes

1. ❌ Using `(left + right) / 2` — can overflow! Use `left + (right-left)/2`
2. ❌ `if (L[i] < R[j])` instead of `<=` — breaks stability
3. ❌ Forgetting to copy remaining elements after one pointer exhausted

---

## 🧩 Variations

1. **Count Inversions** — when we merge, count how many R elements go before L elements
2. **Sort Linked List** — merge sort is perfect (find middle with slow/fast pointer)
3. **Merge K Sorted Arrays** — extension of merge step

---

## 🧠 Memory Trick

**"DIVIDE by mid, SORT halves, MERGE back"**
3 steps: find mid → recursive sort → merge function

---

## 🎯 Final Summary

- Divide and Conquer: split → sort halves → merge
- O(N log N) guaranteed — no worst case like Quick Sort
- Stable sort (use <= in comparison during merge)
- Extra O(N) space — disadvantage vs quick sort
- Perfect for: linked lists, external sorting, counting inversions
