# 02. Detect Cycle in Linked List (Floyd's Algorithm)

**LeetCode:** https://leetcode.com/problems/linked-list-cycle/
**LeetCode II:** https://leetcode.com/problems/linked-list-cycle-ii/
**Difficulty:** 🟡 Medium | **Pattern:** Tortoise and Hare (Two Pointer)

---

## 🧠 Intuition

**In Simple Words:**
Does the linked list have a cycle (loop)? If yes, where does it start?

**Real-Life Analogy:**
Two runners on a circular track. Faster runner will eventually LAP the slower one — they'll meet!
On a straight track (no cycle), they NEVER meet.

**Floyd's Algorithm:**
Use two pointers — slow (moves 1 step) and fast (moves 2 steps).
- If cycle exists: fast catches up to slow → they MEET
- If no cycle: fast reaches null

**Why they meet?**
Imagine slow has entered cycle. Fast is k steps behind slow inside the cycle.
Each step, fast gains 1 on slow (2-1=1 step gain). After 'k' steps → meet!

---

## 🚀 Solution — Part 1: Detect Cycle

```java
public class DetectCycle {
    // Part 1: Does a cycle exist?
    public static boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;        // Moves 1 step
            fast = fast.next.next;   // Moves 2 steps

            if (slow == fast) return true;  // They met → cycle!
        }

        return false;  // fast reached null → no cycle
    }

    // Part 2: Find START of cycle (LeetCode 142)
    public static ListNode detectCycleStart(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        boolean hasCycle = false;

        // Step 1: Find meeting point
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) {
                hasCycle = true;
                break;
            }
        }

        if (!hasCycle) return null;

        // Step 2: Move one pointer to head, keep other at meeting point
        // Both move 1 step at a time → they meet at cycle START
        slow = head;
        while (slow != fast) {
            slow = slow.next;
            fast = fast.next;
        }

        return slow;  // This is the start of the cycle!
    }
}
```

**Time:** O(N) | **Space:** O(1)

**Why does Part 2 work?**
```
Let:
  L = distance from head to cycle start
  C = cycle length
  k = distance from cycle start to meeting point

When they meet:
  slow traveled: L + k
  fast traveled: L + k + m*C (m full extra loops)
  fast = 2 * slow → L + k + m*C = 2(L + k)
  → L = m*C - k
  → L = (m-1)*C + (C - k)

Moving one pointer to head:
  It travels L steps to reach cycle start.
  Other pointer (at meeting point) also travels L steps
  = (m-1)*C + (C-k) steps from meeting point
  = exactly at cycle start!
→ They meet at cycle start! 🎯
```

**Dry Run:** 1→2→3→4→5→3 (cycle from 5 back to 3)
```
Start: slow=1, fast=1
Step 1: slow=2, fast=3
Step 2: slow=3, fast=5
Step 3: slow=4, fast=4 (fast: 5→3→4, taking 2 steps)

Wait: fast.next.next from 5 → 5.next=3, 3.next=4 → fast=4
slow=4, fast=4 → MEET! hasCycle=true

Move slow to head=1, fast stays at 4:
Step 1: slow=2, fast=5
Step 2: slow=3, fast=3 → MEET! Cycle starts at node 3 ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Floyd's Tortoise and Hare
**Used in:** Find duplicate number (LeetCode 287), Find middle of LL, Happy number

---

## 🧠 How to Think (Interview View)

1. "Cycle detection" → immediately Floyd's algorithm
2. "Find cycle start" → Phase 2: one to head, both move 1 step
3. "Find middle" → slow/fast pointer (slow is middle when fast reaches end)

---

## ⚠️ Common Mistakes

1. ❌ Checking `fast != null` but not `fast.next != null` → NullPointerException
2. ❌ Not resetting slow to head in Phase 2
3. ❌ Moving fast 1 step in Phase 2 (should be 1 step, same as slow)

---

## 🧩 Variations

1. **Length of cycle** — after meeting, move slow 1 step until back to meeting point, count steps
2. **Find duplicate number** — LeetCode 287: model as linked list with cycle!
3. **Happy Number** — LeetCode 202: cycle detection in sequence

---

## 🧠 Memory Trick

**"SLOW tortoise, FAST hare — if they meet, it's a LOOP!"**
Phase 2: "Reset one to START, walk together — meet at LOOP START"

---

## 🎯 Final Summary

- Floyd's: slow moves 1 step, fast moves 2 steps
- Meet → cycle exists. No meet (fast reaches null) → no cycle
- Cycle start: reset slow to head, both move 1 step, they meet at cycle start
- Math proof: L = distance to cycle start = (C - k) from meeting point
- O(N) time, O(1) space — much better than HashSet approach!
