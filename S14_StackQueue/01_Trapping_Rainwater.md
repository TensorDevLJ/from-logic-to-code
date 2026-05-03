# 01. Trapping Rain Water

**LeetCode:** https://leetcode.com/problems/trapping-rain-water/
**Difficulty:** 🔴 Hard | **Pattern:** Two Pointer / Monotonic Stack / Prefix Max

---

## 🧠 Intuition

**In Simple Words:**
Given bar chart heights, compute how much water accumulates between bars after rain.
height=[0,1,0,2,1,0,1,3,2,1,2,1] → answer=6

**Real-Life Analogy:**
Think of the heights as walls. Rain fills the valleys between taller walls. At each position, water level = min(tallestWallLeft, tallestWallRight) - height[i].

**Core Insight:**
Water at position i = min(maxLeft[i], maxRight[i]) - height[i]
(Bounded by shorter of two walls from left and right)

---

## 🐢 Brute Force O(N²)

```java
public static int trapBrute(int[] height) {
    int n = height.length, total = 0;
    for (int i = 0; i < n; i++) {
        int maxLeft = 0, maxRight = 0;
        for (int j = 0; j <= i; j++) maxLeft = Math.max(maxLeft, height[j]);
        for (int j = i; j < n; j++) maxRight = Math.max(maxRight, height[j]);
        total += Math.min(maxLeft, maxRight) - height[i];
    }
    return total;
}
```

**Time:** O(N²) | **Space:** O(1)

---

## ⚡ Better: Prefix/Suffix Arrays O(N), O(N) space

```java
public static int trapPrefixSuffix(int[] height) {
    int n = height.length;
    int[] maxLeft = new int[n];
    int[] maxRight = new int[n];

    maxLeft[0] = height[0];
    for (int i = 1; i < n; i++)
        maxLeft[i] = Math.max(maxLeft[i-1], height[i]);

    maxRight[n-1] = height[n-1];
    for (int i = n-2; i >= 0; i--)
        maxRight[i] = Math.max(maxRight[i+1], height[i]);

    int total = 0;
    for (int i = 0; i < n; i++)
        total += Math.min(maxLeft[i], maxRight[i]) - height[i];

    return total;
}
```

---

## 🚀 Optimal: Two Pointer O(N), O(1) space

**Insight:** At each step, the SMALLER side determines water level.
If maxLeft < maxRight → process left. Else process right.

```java
public class TrappingRainWater {
    public static int trap(int[] height) {
        int left = 0, right = height.length - 1;
        int maxLeft = 0, maxRight = 0;
        int water = 0;

        while (left < right) {
            if (height[left] < height[right]) {
                // Left side is limiting factor
                if (height[left] >= maxLeft) {
                    maxLeft = height[left];  // Update max
                } else {
                    water += maxLeft - height[left];  // Collect water
                }
                left++;
            } else {
                // Right side is limiting factor
                if (height[right] >= maxRight) {
                    maxRight = height[right];
                } else {
                    water += maxRight - height[right];
                }
                right--;
            }
        }

        return water;
    }

    public static void main(String[] args) {
        System.out.println(trap(new int[]{0,1,0,2,1,0,1,3,2,1,2,1})); // 6
        System.out.println(trap(new int[]{4,2,0,3,2,5}));              // 9
    }
}
```

**Time:** O(N) | **Space:** O(1)

**Dry Run:** [0,1,0,2,1,0,1,3,2,1,2,1]
```
left=0, right=11, maxL=0, maxR=0, water=0

h[0]=0 < h[11]=1:
  h[0]=0 >= maxL=0 → maxL=0. left=1. water=0

h[1]=1 < h[11]=1? NO (equal, take else path):
  h[11]=1 >= maxR=0 → maxR=1. right=10. water=0

h[1]=1 < h[10]=2:
  h[1]=1 >= maxL=0 → maxL=1. left=2. water=0

h[2]=0 < h[10]=2:
  h[2]=0 < maxL=1 → water += 1-0 = 1. left=3. water=1

h[3]=2 < h[10]=2? NO:
  h[10]=2 >= maxR=1 → maxR=2. right=9. water=1

...continuing this way...
Final water = 6 ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Two Pointer (meets in middle)
**Also solvable with:** Monotonic Stack (track water levels using stack)

---

## 🧠 How to Think (Interview View)

1. "Water at i = min(leftMax, rightMax) - height[i]"
2. Precompute leftMax/rightMax arrays: O(N) space
3. Optimize: Two pointer — process from whichever side is SMALLER

**Why two pointer works?**
If height[left] < height[right], we know maxRight ≥ height[right] > height[left].
So water at left = maxLeft - height[left] (safe to compute without knowing exact maxRight!)

---

## ⚠️ Common Mistakes

1. ❌ `water += min(maxLeft, maxRight) - height[i]` can go negative → always ≥ 0 but min-height could be 0
2. ❌ Wrong pointer update direction
3. ❌ Not initializing maxLeft/maxRight correctly

---

## 🧩 Variations

1. **Container With Most Water** — LeetCode 11: two pointer, maximize area
2. **Trapping Rain Water II** — LeetCode 407: 2D version with priority queue

---

## 🎯 Final Summary

- Water at i = min(maxLeft, maxRight) - height[i]
- Prefix/suffix arrays: O(N) space, clear and simple
- Two pointer: O(1) space, process smaller side first
- Key insight: smaller side is the bottleneck for water level
- Classic interview problem — memorize the two-pointer approach!
