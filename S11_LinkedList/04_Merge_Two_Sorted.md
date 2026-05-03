# 04. Add Two Numbers (Linked List)

**LeetCode:** https://leetcode.com/problems/add-two-numbers/
**Difficulty:** 🟡 Medium | **Pattern:** Linked List Simulation

---

## 🧠 Intuition

**In Simple Words:**
Two numbers stored as linked lists in REVERSE order. Add them and return result as linked list.
(2→4→3) + (5→6→4) = 342 + 465 = 807 = (7→0→8)

**Why Reverse?** Digits stored from least significant to most significant → easy to add (just traverse forward and carry).

**Core Idea:**
Simulate addition column by column (like how you do addition by hand):
- Add corresponding digits + carry
- If sum ≥ 10: digit = sum%10, carry = 1, else carry = 0
- Continue until both lists exhausted AND carry = 0

---

## 🚀 Solution

```java
public class AddTwoNumbers {
    public static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);  // Dummy head
        ListNode curr = dummy;
        int carry = 0;

        // Continue while either list has nodes OR there's a carry
        while (l1 != null || l2 != null || carry != 0) {
            int sum = carry;  // Start with carry

            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }
            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }

            carry = sum / 10;           // New carry
            curr.next = new ListNode(sum % 10);  // New digit
            curr = curr.next;
        }

        return dummy.next;  // Skip dummy head
    }
}
```

**Time:** O(max(N,M)) | **Space:** O(max(N,M)+1) for result

**Dry Run:** l1=[2,4,3], l2=[5,6,4]
```
Step 1: sum=2+5+0=7, carry=0, node=7. l1=[4,3], l2=[6,4]
Step 2: sum=4+6+0=10, carry=1, node=0. l1=[3], l2=[4]
Step 3: sum=3+4+1=8, carry=0, node=8. l1=null, l2=null
Both null, carry=0 → STOP
Result: 7→0→8 ✅ (represents 807)
```

---

## ⚠️ Common Mistakes

1. ❌ Forgetting to handle remaining carry at end
2. ❌ Not using dummy head (makes head insertion complex)
3. ❌ Stopping loop when one list is null (other may still have nodes)

---

## 🎯 Final Summary

- Use dummy head for clean code
- Process both lists simultaneously + carry
- Loop condition: `l1 != null || l2 != null || carry != 0`
- Extract digit: `sum % 10`, extract carry: `sum / 10`
- O(max(N,M)) time, handles lists of different lengths
