# 01. Reverse a Linked List

**LeetCode:** https://leetcode.com/problems/reverse-linked-list/
**Difficulty:** 🟡 Medium | **Pattern:** Linked List Manipulation

---

## 🧠 Intuition

**In Simple Words:**
1→2→3→4→5→null becomes 5→4→3→2→1→null

**Real-Life Analogy:**
A train with coaches. Detach each coach and attach it to the front of a new train.

**Core Idea:**
Walk through list. For each node, reverse its pointer to point to the previous node.
Need 3 pointers: prev (null), curr (head), next (to not lose the rest of list).

---

## 📌 Node Definition (Used throughout)

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}
```

---

## 🚀 Solution — Iterative (Best for interviews)

```java
public class ReverseLinkedList {
    public static ListNode reverseList(ListNode head) {
        ListNode prev = null;   // Previous node (new tail points to null)
        ListNode curr = head;   // Current node being processed

        while (curr != null) {
            ListNode nextTemp = curr.next; // Save next before breaking link
            curr.next = prev;             // Reverse the pointer!
            prev = curr;                  // Move prev forward
            curr = nextTemp;              // Move curr forward
        }

        return prev;  // prev is now the new head
    }

    // Recursive Approach
    public static ListNode reverseRecursive(ListNode head) {
        // Base case: empty or single node
        if (head == null || head.next == null) return head;

        // Reverse the rest of the list
        ListNode newHead = reverseRecursive(head.next);

        // Make head.next point back to head
        head.next.next = head;
        head.next = null;  // head becomes new tail

        return newHead;
    }

    public static void main(String[] args) {
        // Build: 1→2→3→4→5
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(3);
        head.next.next.next = new ListNode(4);
        head.next.next.next.next = new ListNode(5);

        ListNode rev = reverseList(head);
        // Print: 5→4→3→2→1
        while (rev != null) {
            System.out.print(rev.val + " ");
            rev = rev.next;
        }
    }
}
```

**Time:** O(N) | **Space:** O(1) iterative, O(N) recursive stack

**Dry Run (Iterative):** 1→2→3→null
```
Init: prev=null, curr=1

Step 1: next=2, curr(1).next=null, prev=1, curr=2
  List state: null←1  2→3→null

Step 2: next=3, curr(2).next=1, prev=2, curr=3
  List state: null←1←2  3→null

Step 3: next=null, curr(3).next=2, prev=3, curr=null
  List state: null←1←2←3

curr==null → stop. Return prev=3
3→2→1→null ✅
```

**Dry Run (Recursive):** 1→2→3
```
rev(1→2→3):
  rev(2→3):
    rev(3): returns 3 (base case)
    3.next = 2 (3→2), 2.next = null
    returns 3 (new head)
  3.next=2 (already), 2.next.next = 1 → 1.next=1? NO:
  head.next.next = head → 2.next = 1 (2→1)
  head.next = null → 1.next = null
  returns 3
Final: 3→2→1→null ✅
```

---

## 🔁 Pattern Recognition

- **Pattern:** Pointer manipulation in linked lists
- **Used in:** Reverse in K groups, Palindrome LL, Rotate LL

---

## 🧠 How to Think (Interview View)

Draw the pointer changes first!
Before: prev ← null, curr→ 1→2→3
After each step: reverse curr.next to prev, slide both forward.

**Key: always save next BEFORE changing curr.next!**

---

## ⚠️ Common Mistakes

1. ❌ Not saving `nextTemp = curr.next` before reversing pointer
2. ❌ Returning `curr` instead of `prev` at end
3. ❌ Recursive: forgetting `head.next = null` (creates cycle!)

---

## 🧩 Variations

1. **Reverse in K Groups** — LeetCode 25
2. **Reverse between positions** — LeetCode 92
3. **Palindrome LL** — reverse second half, compare
4. **Rotate LL** — uses reverse or find new tail

---

## 🧠 Memory Trick

**"Three Musketeers: prev, curr, next — always save NEXT before reversing!"**

---

## 🎯 Final Summary

- Three pointer approach: prev=null, curr=head
- Each step: save next, reverse pointer, advance both
- Return prev (new head) at end
- O(N) time, O(1) space iteratively
- Recursive: elegant but uses O(N) stack space
