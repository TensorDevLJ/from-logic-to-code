# 05. Quick Sort

**LeetCode:** https://leetcode.com/problems/sort-an-array/
**GFG:** https://practice.geeksforgeeks.org/problems/quick-sort/1
**Difficulty:** 🟢 Easy | **Pattern:** Divide and Conquer / Partitioning

---

## 🧠 Intuition

**In Simple Words:**
Pick a "pivot" element. Partition array so all elements smaller than pivot go left, larger go right. Pivot is now in its FINAL position! Recurse on both sides.

**Real-Life Analogy:**
Organizing people by height. Pick one person as reference (pivot). People shorter than them go left, taller go right. Reference person sits in their final seat. Organize each group similarly.

**Core Idea:**
- Choose pivot (usually last element)
- Partition: elements < pivot to left, > pivot to right
- Pivot is in final sorted position!
- Recursively sort left and right partitions

---

## 🚀 Solution (Lomuto Partition Scheme)

```java
public class QuickSort {
    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {  // Base case: 1 or 0 elements
            int pivotIdx = partition(arr, low, high);

            quickSort(arr, low, pivotIdx - 1);  // Sort left of pivot
            quickSort(arr, pivotIdx + 1, high);  // Sort right of pivot
        }
    }

    private static int partition(int[] arr, int low, int high) {
        int pivot = arr[high];  // Choose last element as pivot
        int i = low - 1;        // i tracks position of last smaller element

        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                // Swap arr[i] and arr[j]
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }

        // Place pivot in correct position
        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;

        return i + 1;  // Return pivot index
    }

    public static void main(String[] args) {
        int[] arr = {10, 7, 8, 9, 1, 5};
        quickSort(arr, 0, arr.length - 1);
        System.out.println(Arrays.toString(arr));
        // [1, 5, 7, 8, 9, 10]
    }
}
```

**Time:**
- Best/Average: O(N log N)
- Worst (sorted array, always pick last as pivot): O(N²)

**Space:** O(log N) recursive stack (avg), O(N) worst case

**Dry Run:** [10, 7, 8, 9, 1, 5], pivot=5
```
i=-1, pivot=5
j=0: arr[0]=10 > 5 → skip
j=1: arr[1]=7  > 5 → skip
j=2: arr[2]=8  > 5 → skip
j=3: arr[3]=9  > 5 → skip
j=4: arr[4]=1 <= 5 → i=0, swap arr[0] and arr[4] → [1,7,8,9,10,5]
Place pivot: swap arr[i+1]=arr[1] with arr[high]=arr[5] → [1,5,8,9,10,7]
Wait, let me redo: after j loop, i=0, place pivot at arr[1]: swap arr[1] and arr[5]
→ [1,5,8,9,10,7]... 

Correct trace: pivot=5, arr=[10,7,8,9,1,5]
j goes 0..4:
  j=0: 10>5, skip. i=-1
  j=1: 7>5, skip. i=-1
  j=2: 8>5, skip. i=-1
  j=3: 9>5, skip. i=-1
  j=4: 1<=5, i=0, swap(arr[0],arr[4]) → [1,7,8,9,10,5]
End of j loop: i=0
swap(arr[i+1],arr[high]) = swap(arr[1],arr[5]) → [1,5,8,9,10,7]
Hmm, 7 in wrong place...

NOTE: pivot=5 is now at index 1.
Left side: [1] (only element < 5)
Right side: [8,9,10,7] (all > 5)
After sorting right: [7,8,9,10]
Final: [1,5,7,8,9,10] ✅
```

---

## 🔁 Properties

| Property | Value |
|----------|-------|
| Best/Avg Time | O(N log N) |
| Worst Time | O(N²) |
| Space | O(log N) avg |
| Stable | ❌ No |
| In-place | ✅ Yes |

**Why O(N log N) average?**
Each partition roughly halves the array → log N levels.
Each level does O(N) work → N log N total.

**Avoiding worst case:**
- Random pivot selection
- Median-of-three pivot
- Java's Arrays.sort uses Dual-Pivot Quicksort

---

## 🧠 How to Think (Interview View)

1. "Quick sort" → think pivot, partition, recurse
2. Partition step is the KEY — understand i and j pointers
3. Pivot goes to its FINAL position after partition — beautiful!

---

## ⚠️ Common Mistakes

1. ❌ Off-by-one in partition (j < high, not j <= high)
2. ❌ Forgetting to place pivot at correct position after j loop
3. ❌ Not handling base case properly

---

## 🧩 Variations

1. **Kth Largest/Smallest** — QuickSelect: only recurse on relevant side! O(N) avg
2. **Dutch National Flag** — 3-way partition (for arrays with duplicates)

---

## 🎯 Final Summary

- Choose pivot → partition (smaller left, larger right) → pivot in final place → recurse
- O(N log N) average, O(N²) worst (avoid with random pivot)
- In-place O(log N) space — better than merge sort's O(N)
- Not stable, but fastest in practice for random data
- QuickSelect: only recurse on one side → find Kth element O(N) avg
