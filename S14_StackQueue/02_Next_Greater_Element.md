# 02. Next Greater Element (Monotonic Stack)

**LeetCode:** https://leetcode.com/problems/next-greater-element-i/
**Difficulty:** 🟡 Medium | **Pattern:** Monotonic Stack

---

## 🧠 Intuition

**In Simple Words:**
For each element, find the NEXT element to its RIGHT that is GREATER.
[4,1,2] → [-1, 2, -1] (no greater for 4, 2 is next greater for 1, nothing for 2)

**Real-Life Analogy:**
Standing in a queue. Each person wants to know: "Who is the next taller person standing to my right?"

**Core Idea — Monotonic Stack:**
Maintain a stack in DECREASING order.
When we see a larger element → it's the "next greater" for everything smaller in the stack!
Pop all smaller elements, assign current as their answer.

---

## 🚀 Solution — Monotonic Stack O(N)

```java
public class NextGreaterElement {
    public static int[] nextGreaterElement(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        Arrays.fill(result, -1);  // Default: no greater element

        Deque<Integer> stack = new ArrayDeque<>();  // Stores INDICES

        for (int i = 0; i < n; i++) {
            // Current element is greater than stack top → pop and assign
            while (!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
                result[stack.pop()] = nums[i];
            }
            stack.push(i);  // Push current index
        }

        return result;
    }

    // Variant: Next Greater Element in Circular Array (LeetCode 503)
    public static int[] nextGreaterCircular(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        Arrays.fill(result, -1);
        Deque<Integer> stack = new ArrayDeque<>();

        // Process array TWICE (simulate circular)
        for (int i = 0; i < 2 * n; i++) {
            int idx = i % n;
            while (!stack.isEmpty() && nums[stack.peek()] < nums[idx]) {
                result[stack.pop()] = nums[idx];
            }
            if (i < n) stack.push(i);  // Only push in first pass
        }

        return result;
    }

    public static void main(String[] args) {
        System.out.println(Arrays.toString(nextGreaterElement(new int[]{4,1,2}))); // [-1,2,-1]
        System.out.println(Arrays.toString(nextGreaterElement(new int[]{2,1,2,4,3}))); // [4,2,4,-1,-1]
    }
}
```

**Time:** O(N) — each element pushed and popped at most once!  
**Space:** O(N) for stack

**Dry Run:** [2,1,2,4,3]
```
i=0: nums[0]=2. stack empty. push 0. stack=[0]
i=1: nums[1]=1. 1 < 2 → push 1. stack=[0,1]
i=2: nums[2]=2. 2 > nums[1]=1 → pop 1, result[1]=2. 2 == nums[0]=2, stop. push 2. stack=[0,2]
i=3: nums[3]=4. 4>nums[2]=2 → pop 2, result[2]=4. 4>nums[0]=2 → pop 0, result[0]=4. push 3. stack=[3]
i=4: nums[4]=3. 3 < 4, push 4. stack=[3,4]

Remaining in stack: indices 3,4 → result stays -1

Result: [4,2,4,-1,-1] ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Monotonic Decreasing Stack
**Used in:** Next Greater Element, Previous Smaller Element, Largest Rectangle in Histogram, Stock Span, Sum of Subarray Minimums

**Monotonic Stack Types:**
- **Decreasing stack** → used for "Next Greater Element"
- **Increasing stack** → used for "Next Smaller Element"

---

## 🧠 How to Think (Interview View)

1. "Next greater to the right" → Monotonic DECREASING stack
2. "Next smaller to the right" → Monotonic INCREASING stack
3. Stack stores INDICES (not values) — to assign results

**Template:**
```
for each element i:
    while stack not empty AND nums[stack.top] < nums[i]:  // violates decreasing
        result[stack.pop()] = nums[i]  // current is "next greater"
    push i
// remaining in stack → no next greater → result = -1
```

---

## ⚠️ Common Mistakes

1. ❌ Storing values in stack (need indices to assign results)
2. ❌ Using wrong comparison (`<=` vs `<`)
3. ❌ Forgetting to initialize result with -1

---

## 🧩 Variations

1. **Previous Greater Element** → traverse right to left
2. **Next Smaller Element** → use `>` instead of `<`
3. **Circular Array** → process 2N elements with `i % n`
4. **Stock Span** → count days stock price was ≤ current (monotonic stack)

---

## 🧠 Memory Trick

**"Stack stays DECREASING. Big element POPS smaller ones and becomes their ANSWER."**

---

## 🎯 Final Summary

- Monotonic decreasing stack: when larger element seen, pop smaller ones
- Each popped element's "next greater" = current element
- O(N) time — each element pushed/popped exactly once
- Store indices not values
- Circular: traverse 2N times, only push in first pass
