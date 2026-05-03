# 03. Largest Rectangle in Histogram

**LeetCode:** https://leetcode.com/problems/largest-rectangle-in-histogram/
**Difficulty:** 🔴 Hard | **Pattern:** Monotonic Stack

---

## 🧠 Intuition

**In Simple Words:**
Given bar heights, find the largest rectangle that can be formed using consecutive bars.
heights=[2,1,5,6,2,3] → largest rectangle has area 10 (bars of height 5 and 6)

**Key Insight:**
For each bar i, the largest rectangle with height[i] as the MINIMUM extends from:
- Left boundary: first bar SHORTER than height[i] to the left
- Right boundary: first bar SHORTER than height[i] to the right
Width = rightBound - leftBound - 1
Area = height[i] × width

**Monotonic Stack Approach:**
As we add bars, if current bar is SMALLER than stack top → we've found the right boundary for top bar!
Pop and calculate area.

---

## 🚀 Solution

```java
public class LargestRectangleHistogram {
    public static int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int maxArea = 0;
        Deque<Integer> stack = new ArrayDeque<>();  // Monotonic INCREASING

        for (int i = 0; i <= n; i++) {
            int currHeight = (i == n) ? 0 : heights[i];  // 0 at end to flush stack

            while (!stack.isEmpty() && heights[stack.peek()] > currHeight) {
                int height = heights[stack.pop()];
                int width;
                if (stack.isEmpty()) {
                    width = i;  // Extends from 0 to i-1
                } else {
                    width = i - stack.peek() - 1;  // Between stack top and current
                }
                maxArea = Math.max(maxArea, height * width);
            }

            stack.push(i);
        }

        return maxArea;
    }

    public static void main(String[] args) {
        System.out.println(largestRectangleArea(new int[]{2,1,5,6,2,3})); // 10
        System.out.println(largestRectangleArea(new int[]{2,4}));          // 4
    }
}
```

**Time:** O(N) | **Space:** O(N)

**Dry Run:** [2,1,5,6,2,3]
```
i=0 (h=2): stack empty, push 0. stack=[0]
i=1 (h=1): h=1 < heights[0]=2 → pop 0:
  height=2, stack empty → width=1, area=2*1=2, maxArea=2
  push 1. stack=[1]
i=2 (h=5): 5>1, push 2. stack=[1,2]
i=3 (h=6): 6>5, push 3. stack=[1,2,3]
i=4 (h=2): 2 < heights[3]=6 → pop 3:
  height=6, stack top=2 → width=4-2-1=1, area=6*1=6, maxArea=6
  2 < heights[2]=5 → pop 2:
  height=5, stack top=1 → width=4-1-1=2, area=5*2=10, maxArea=10
  2>heights[1]=1, stop. push 4. stack=[1,4]
i=5 (h=3): 3>2, push 5. stack=[1,4,5]
i=6 (h=0, sentinel): pop 5: height=3, top=4, width=6-4-1=1, area=3
  pop 4: height=2, top=1, width=6-1-1=4, area=8
  pop 1: height=1, empty, width=6, area=6

maxArea = 10 ✅
```

---

## 🎯 Final Summary

- For each bar: find first shorter bar on left and right → that's the width
- Monotonic increasing stack: when shorter bar found, pop and calculate area
- Add sentinel 0 at end to flush remaining stack elements
- Width when stack empty = i (extends to left boundary)
- Width when stack not empty = i - stack.peek() - 1
