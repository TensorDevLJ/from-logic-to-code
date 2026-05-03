# 02. Bubble Sort

**GFG:** https://practice.geeksforgeeks.org/problems/bubble-sort/1
**Difficulty:** 🟢 Easy | **Pattern:** Comparison-Based Sorting

---

## 🧠 Intuition

**In Simple Words:**
In each pass, compare adjacent elements. If left > right, swap them.
Largest elements "bubble up" to their correct position at the end.

**Real-Life Analogy:**
Bubbles in soda rise to the top — largest elements rise to the end each pass.

**Core Idea:**
- In each pass, the largest unsorted element reaches its final position
- After k passes, last k elements are sorted
- Optimization: if no swaps in a pass → array is sorted → stop early!

---

## 🚀 Solution (Optimized with Early Exit)

```java
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;

        for (int i = 0; i < n - 1; i++) {
            boolean swapped = false;  // Optimization flag

            // In each pass, largest unsorted element goes to arr[n-1-i]
            for (int j = 0; j < n - 1 - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Swap adjacent elements
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }

            // If no swaps → already sorted!
            if (!swapped) break;
        }
    }

    public static void main(String[] args) {
        int[] arr = {5, 4, 3, 2, 1};
        bubbleSort(arr);
        System.out.println(Arrays.toString(arr)); // [1, 2, 3, 4, 5]

        int[] sorted = {1, 2, 3, 4, 5};
        bubbleSort(sorted); // Only 1 pass! (early exit)
    }
}
```

**Time:**
- Best (sorted): O(N) — with optimization
- Average/Worst: O(N²)

**Space:** O(1)

**Dry Run:** [5, 3, 1, 4, 2]
```
Pass 0 (i=0): j goes 0..3
  j=0: 5>3 → swap → [3,5,1,4,2]
  j=1: 5>1 → swap → [3,1,5,4,2]
  j=2: 5>4 → swap → [3,1,4,5,2]
  j=3: 5>2 → swap → [3,1,4,2,5]  ← 5 is sorted!

Pass 1 (i=1): j goes 0..2
  j=0: 3>1 → swap → [1,3,4,2,5]
  j=1: 3<4 → no swap
  j=2: 4>2 → swap → [1,3,2,4,5]  ← 4 is sorted!

Pass 2 (i=2): j goes 0..1
  j=0: 1<3 → no swap
  j=1: 3>2 → swap → [1,2,3,4,5]  ← 3 is sorted!

Pass 3 (i=3): no swaps → swapped=false → BREAK!
Final: [1,2,3,4,5] ✅
```

---

## 🔁 Properties

| Property | Value |
|----------|-------|
| Time (Best) | O(N) with optimization |
| Time (Avg/Worst) | O(N²) |
| Space | O(1) |
| Stable | ✅ Yes |
| In-place | ✅ Yes |

**Why stable?** We only swap if arr[j] > arr[j+1]. Equal elements never swap.

---

## ⚠️ Common Mistakes

1. ❌ Inner loop to n-1 instead of n-1-i (re-sorting sorted elements)
2. ❌ Forgetting the `swapped` optimization
3. ❌ Using `>=` instead of `>` (breaks stability)

---

## 🎯 Final Summary

- Compare adjacent, swap if out of order → largest bubbles to end
- O(N²) worst/avg, O(N) best (with swapped flag optimization)
- Stable sort (equal elements maintain order)
- Key optimization: `swapped` flag for early termination
- Rarely used in practice but fundamental to teach sorting concepts
