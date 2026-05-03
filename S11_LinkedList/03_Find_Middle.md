# 03. Middle of a Linked List

**LeetCode:** https://leetcode.com/problems/middle-of-the-linked-list/
**Difficulty:** 🟢 Easy | **Pattern:** Tortoise and Hare

---

## 🧠 Intuition

**In Simple Words:**
Find the middle node of a linked list.
1→2→3→4→5 → return node 3
1→2→3→4 → return node 3 (second middle for even length)

**Core Idea:**
Use slow/fast pointers. Fast moves 2x speed of slow.
When fast reaches end → slow is at middle!

---

## 🚀 Solution

```java
public class MiddleLinkedList {
    public static ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        // When fast reaches end, slow is at middle
        while (fast != null && fast.next != null) {
            slow = slow.next;        // 1 step
            fast = fast.next.next;   // 2 steps
        }

        return slow;  // Middle node
    }

    public static void main(String[] args) {
        // 1→2→3→4→5 → middle is 3
        // 1→2→3→4   → middle is 3 (second middle)
    }
}
```

**Dry Run:** 1→2→3→4→5
```
slow=1, fast=1
Step 1: slow=2, fast=3
Step 2: slow=3, fast=5
Step 3: fast.next=null → STOP. Return slow=3 ✅
```

**Dry Run:** 1→2→3→4 (even)
```
slow=1, fast=1
Step 1: slow=2, fast=3
Step 2: slow=3, fast=null (fast: 3.next=4, 4.next=null → fast=null)
STOP. Return slow=3 (second middle) ✅
```

---

## 🎯 Final Summary

- Slow/fast pointer: slow moves 1, fast moves 2
- When fast=null or fast.next=null → slow is at middle
- Even length: returns second middle node
- O(N) time, O(1) space — elegant one-pass solution
