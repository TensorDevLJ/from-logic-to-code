# 10. Koko Eating Bananas (Binary Search on Answer)

**LeetCode:** https://leetcode.com/problems/koko-eating-bananas/
**Difficulty:** 🟡 Medium | **Pattern:** Binary Search on Answer Space

---

## 🧠 Intuition

**In Simple Words:**
Koko has piles of bananas. She eats at most k bananas per hour. Find minimum k such that she finishes all piles within h hours.

piles=[3,6,7,11], h=8 → Answer: k=4
- At k=4: pile[0]: ceil(3/4)=1hr, pile[1]: ceil(6/4)=2hr, pile[2]: ceil(7/4)=2hr, pile[3]: ceil(11/4)=3hr. Total=8hrs ≤ 8 ✅

**Core Insight — Binary Search on Answer:**
Instead of searching in the array, we search in the ANSWER SPACE (possible values of k).
- k ranges from 1 to max(piles)
- For a given k, check if total hours ≤ h (can she finish in time?)
- Find the MINIMUM k where this is possible

**Monotonic Property:**
- Higher k → fewer hours needed (can always finish)
- Lower k → more hours needed
- → Binary search for minimum valid k!

---

## 🚀 Solution

```java
public class KokoEatingBananas {
    // Check if eating at speed k finishes all piles in h hours
    private static boolean canFinish(int[] piles, int k, int h) {
        long totalHours = 0;
        for (int pile : piles) {
            totalHours += (pile + k - 1) / k;  // ceil(pile/k) = (pile + k - 1) / k
            if (totalHours > h) return false;   // Early exit optimization
        }
        return totalHours <= h;
    }

    public static int minEatingSpeed(int[] piles, int h) {
        int low = 1;                            // Minimum possible speed
        int high = 0;
        for (int p : piles) high = Math.max(high, p);  // Max pile size

        int result = high;  // Worst case: eat fastest pile per hour

        while (low <= high) {
            int mid = low + (high - low) / 2;  // Try this speed

            if (canFinish(piles, mid, h)) {
                result = mid;       // Mid works! Try even smaller
                high = mid - 1;
            } else {
                low = mid + 1;      // Too slow, need higher speed
            }
        }

        return result;
    }

    public static void main(String[] args) {
        System.out.println(minEatingSpeed(new int[]{3,6,7,11}, 8));  // 4
        System.out.println(minEatingSpeed(new int[]{30,11,23,4,20}, 5)); // 30
    }
}
```

**Time:** O(N × log(max_pile)) | **Space:** O(1)

**Dry Run:** piles=[3,6,7,11], h=8, low=1, high=11
```
mid=6: canFinish(k=6)? 1+1+2+2=6hrs ≤ 8 ✅. result=6, high=5
mid=3: canFinish(k=3)? 1+2+3+4=10hrs > 8 ❌. low=4
mid=4: canFinish(k=4)? 1+2+2+3=8hrs ≤ 8 ✅. result=4, high=3
low(4) > high(3) → STOP. Return 4 ✅
```

---

## 🔑 Binary Search on Answer — The Pattern

**When to use:**
- "Find minimum X such that [condition] is satisfied"
- "Find maximum X such that [condition] is satisfied"
- The condition must be MONOTONIC (once true, stays true as X increases/decreases)

**Template:**
```java
int low = minPossible, high = maxPossible;
int result = high;  // or low for max problems

while (low <= high) {
    int mid = low + (high - low) / 2;
    if (isValid(mid)) {       // Can we achieve it with 'mid'?
        result = mid;
        high = mid - 1;       // Try better (smaller for minimization)
    } else {
        low = mid + 1;        // Not achievable, try larger
    }
}
return result;
```

---

## 🔁 Pattern Recognition

**All "Binary Search on Answer" problems follow same template:**
- Koko Eating Bananas → find min eating speed
- Book Allocation → find min max pages  
- Aggressive Cows → find max min distance
- Capacity to Ship → find min ship capacity

**The key:** Identify the search space (low to high) and define `isValid(mid)` function.

---

## ⚠️ Common Mistakes

1. ❌ `pile / k` for ceiling division → should be `(pile + k - 1) / k`
2. ❌ Setting high = piles.length (should be max element!)
3. ❌ Not handling overflow in totalHours (use long!)

---

## 🎯 Final Summary

- Binary Search on Answer: search in answer space [1, max(piles)]
- For each candidate k: check if total hours ≤ h
- Minimize k → when valid, shrink high; when invalid, grow low
- ceil division trick: `(pile + k - 1) / k` = Math.ceil(pile/k)
- Pattern: ALL "minimize maximum / maximize minimum" problems use this!
