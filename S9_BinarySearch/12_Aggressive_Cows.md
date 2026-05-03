# 12. Aggressive Cows

**GFG/SPOJ:** https://practice.geeksforgeeks.org/problems/aggressive-cows/0
**Difficulty:** 🔴 Hard | **Pattern:** Binary Search on Answer

---

## 🧠 Intuition

**In Simple Words:**
Given N stalls at positions, place C cows such that the MINIMUM distance between any two cows is MAXIMIZED.
Stalls=[1,2,4,8,9], C=3 → Place cows at 1,4,9 → min distance=3. Answer=3.

**Core Idea:**
We want to MAXIMIZE the minimum gap. This screams "Binary Search on Answer!"
Search space: min distance can range from 1 to (max-min)/(c-1).
For each candidate distance d: check if we can place all C cows with minimum gap ≥ d.

**Monotonic Property:**
- Large min distance → harder to place cows (fewer valid placements)
- Small min distance → easier to place
- Binary search for the MAXIMUM valid min distance!

---

## 🚀 Solution

```java
public class AggressiveCows {
    // Check if we can place 'cows' with at least 'minDist' gap
    private static boolean canPlace(int[] stalls, int cows, int minDist) {
        int count = 1;           // Place first cow at stalls[0]
        int lastPos = stalls[0]; // Position of last placed cow

        for (int i = 1; i < stalls.length; i++) {
            if (stalls[i] - lastPos >= minDist) {
                count++;            // Place cow here
                lastPos = stalls[i];
                if (count == cows) return true;  // All placed!
            }
        }
        return false;  // Couldn't place all cows
    }

    public static int solve(int[] stalls, int k) {
        Arrays.sort(stalls);  // Must sort first!

        int n = stalls.length;
        int low = 1;  // Minimum possible gap
        int high = stalls[n - 1] - stalls[0];  // Maximum possible gap

        int result = 0;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (canPlace(stalls, k, mid)) {
                result = mid;    // Mid works! Try larger gap
                low = mid + 1;  // Maximize → try higher
            } else {
                high = mid - 1;  // Too large, reduce gap
            }
        }

        return result;
    }

    public static void main(String[] args) {
        int[] stalls = {1, 2, 4, 8, 9};
        System.out.println(solve(stalls, 3));  // 3 (place at 1,4,9)
    }
}
```

**Time:** O(N log N) sort + O(N log(max-min)) search | **Space:** O(1)

**Dry Run:** stalls=[1,2,4,8,9], k=3
```
sorted: [1,2,4,8,9], low=1, high=8

mid=4: canPlace(k=3, gap=4)?
  Place at 1. Next: 2-1=1<4, 4-1=3<4, 8-1=7≥4 → place at 8. count=2
  Next: 9-8=1<4. Only 2 placed. FALSE → high=3

mid=2: canPlace(k=3, gap=2)?
  Place at 1. 2-1=1<2, 4-1=3≥2 → place at 4. count=2
  8-4=4≥2 → place at 8. count=3=k → TRUE! result=2, low=3

mid=3: canPlace(k=3, gap=3)?
  Place at 1. 2-1=1<3, 4-1=3≥3 → place at 4. count=2
  8-4=4≥3 → place at 8. count=3=k → TRUE! result=3, low=4

low(4) > high(3) → STOP. Return 3 ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Maximize minimum → Binary Search on Answer
**Opposite:** Minimize maximum → also Binary Search on Answer
**These are CLASSIC interview problems using this pattern!**

---

## 🎯 Final Summary

- Sort stalls first! Binary search on gap value [1, max-min]
- canPlace: greedily place cows maintaining minimum gap
- Maximize min gap → when valid, try larger (low = mid+1)
- O(N log N) total time
- Same pattern: Book Allocation, EKO, Split Array Largest Sum
