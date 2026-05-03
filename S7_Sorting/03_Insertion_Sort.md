# 03. Insertion Sort

**GFG:** https://practice.geeksforgeeks.org/problems/insertion-sort/0
**Difficulty:** 🟢 Easy | **Pattern:** Comparison-Based Sorting

---

## 🧠 Intuition

**In Simple Words:**
Build sorted array one element at a time. For each element, insert it into its correct position in the already-sorted left portion.

**Real-Life Analogy:**
Sorting a hand of playing cards: you pick up cards one by one, and each new card is inserted into the right position among the cards already in your hand.

**Core Idea:**
- Elements arr[0..i-1] are always sorted
- Take arr[i], compare with elements to its left, shift right until correct position found, insert

---

## 🚀 Solution

```java
public class InsertionSort {
    public static void insertionSort(int[] arr) {
        int n = arr.length;

        for (int i = 1; i < n; i++) {
            int key = arr[i];  // Current element to insert
            int j = i - 1;

            // Shift elements of arr[0..i-1] that are > key
            // to one position to the right
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];  // Shift right
                j--;
            }

            // Insert key at correct position
            arr[j + 1] = key;
        }
    }

    public static void main(String[] args) {
        int[] arr = {12, 11, 13, 5, 6};
        insertionSort(arr);
        System.out.println(Arrays.toString(arr)); // [5, 6, 11, 12, 13]
    }
}
```

**Time:**
- Best (sorted input): O(N) — inner while loop never executes!
- Average/Worst: O(N²)

**Space:** O(1)

**Dry Run:** [12, 11, 13, 5, 6]
```
i=1: key=11, j=0: arr[0]=12 > 11 → shift: [12,12,13,5,6], j=-1
      place: arr[0]=11 → [11,12,13,5,6]

i=2: key=13, j=1: arr[1]=12 < 13 → STOP
      place: arr[2]=13 → [11,12,13,5,6] (no change)

i=3: key=5, shift 13,12,11 right → [11,12,13,13,6] → [11,12,12,13,6] → [11,11,12,13,6]
      place: arr[0]=5 → [5,11,12,13,6]

i=4: key=6, shift 13,12,11 right, 5<6 → stop
      [5,6,11,12,13] ✅
```

---

## 🔁 Properties

| Property | Value |
|----------|-------|
| Best Case | O(N) — nearly sorted data |
| Avg/Worst | O(N²) |
| Space | O(1) |
| Stable | ✅ Yes |
| Adaptive | ✅ Yes (good for nearly-sorted!) |

---

## 🧠 When to Use

- **Nearly sorted data** → approaches O(N) — much better than other sorts!
- Small arrays (N ≤ 100): simple and efficient
- Online sorting (elements arriving one by one)
- Used internally in TimSort (Java's Arrays.sort) for small segments!

---

## ⚠️ Common Mistakes

1. ❌ Starting i from 0 instead of 1
2. ❌ Forgetting arr[j+1] = key at the end
3. ❌ Using `arr[j] >= key` (breaks stability)

---

## 🎯 Final Summary

- Build sorted prefix: for each element, find its spot and insert
- Inner loop shifts elements right (like making room in a hand of cards)
- O(N) best case for nearly sorted — Insertion sort's superpower!
- Stable, in-place, adaptive — used in Java's hybrid TimSort
- Great choice when N is small or data is nearly sorted
