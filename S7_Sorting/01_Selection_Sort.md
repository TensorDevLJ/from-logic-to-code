# 01. Selection Sort

**GFG:** https://practice.geeksforgeeks.org/problems/selection-sort/1
**Difficulty:** 🟢 Easy | **Pattern:** Comparison-Based Sorting

---

## 🧠 Intuition

**In Simple Words:**
In each pass, FIND the minimum element from the unsorted part and place it at the beginning.
Like picking the smallest card from a hand and placing it on the table.

**Real-Life Analogy:**
You're sorting books by page count. You look through all unsorted books, pick the thinnest, place it first. Repeat for remaining.

**Core Idea:**
For each position i (from 0 to n-2):
- Find minimum in arr[i...n-1]
- Swap it with arr[i]
- Position i is now "sorted" — move on!

---

## 📌 Problem Understanding

- **Input:** Unsorted integer array
- **Output:** Sorted array (ascending)
- **In-place:** Yes | **Stable:** No

---

## 🚀 Solution

```java
public class SelectionSort {
    public static void selectionSort(int[] arr) {
        int n = arr.length;

        for (int i = 0; i < n - 1; i++) {
            // Find minimum element in arr[i+1...n-1]
            int minIdx = i;  // Assume current position is minimum

            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIdx]) {
                    minIdx = j;  // Found new minimum
                }
            }

            // Swap minimum to position i
            if (minIdx != i) {
                int temp = arr[minIdx];
                arr[minIdx] = arr[i];
                arr[i] = temp;
            }
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 25, 12, 22, 11};
        selectionSort(arr);
        System.out.println(Arrays.toString(arr)); // [11, 12, 22, 25, 64]
    }
}
```

**Time:** O(N²) always (best, avg, worst — same!)  
**Space:** O(1) — in-place

**Dry Run:** [64, 25, 12, 22, 11]
```
Pass 0 (i=0): min=11 at idx=4, swap → [11, 25, 12, 22, 64]
Pass 1 (i=1): min=12 at idx=2, swap → [11, 12, 25, 22, 64]
Pass 2 (i=2): min=22 at idx=3, swap → [11, 12, 22, 25, 64]
Pass 3 (i=3): min=25 at idx=3, no swap needed
Final: [11, 12, 22, 25, 64] ✅
```

---

## 🔁 Properties

| Property | Value |
|----------|-------|
| Time (Best) | O(N²) |
| Time (Avg) | O(N²) |
| Time (Worst) | O(N²) |
| Space | O(1) |
| Stable | ❌ No |
| In-place | ✅ Yes |

**Why not stable?** Swapping can change relative order of equal elements.

---

## 🧠 When to Use in Interview

- When you need **minimum swaps** — Selection sort does at most N swaps!
- Small datasets
- When auxiliary space is critical (O(1))

---

## ⚠️ Common Mistakes

1. ❌ Inner loop starting at i instead of i+1
2. ❌ Outer loop going to n instead of n-1
3. ❌ Calling it stable — it's NOT

---

## 🎯 Final Summary

- Find minimum in unsorted portion → swap to current position
- O(N²) in all cases — simple but slow
- Maximum N-1 swaps (better than bubble sort's swaps)
- Not stable, in-place
- Use when: N is small, swaps are expensive (but comparisons are free)
