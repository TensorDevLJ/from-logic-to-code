# 04. Print 1 to N using Recursion

**GFG:** https://practice.geeksforgeeks.org/problems/print-1-to-n-without-using-loops/1
**Difficulty:** 🟢 Easy | **Pattern:** Basic Recursion

---

## 🧠 Intuition

**Core Idea:**
To print 1 to N: first print 1 to N-1, then print N.
OR: if we're at i and i ≤ N, print i, then recurse for i+1.

---

## 🚀 Solution

```java
public class Print1ToN {
    // Approach 1: Forward (print before recursing)
    public static void print1ToN(int i, int n) {
        if (i > n) return;   // Base case: past N
        System.out.print(i + " ");
        print1ToN(i + 1, n); // Recurse for next
    }

    // Approach 2: Backtracking style (print after recursing)
    public static void printNTo1(int n) {
        if (n == 0) return;  // Base case
        printNTo1(n - 1);    // Go to 1 first
        System.out.print(n + " "); // Print on way back
    }

    public static void main(String[] args) {
        print1ToN(1, 5);    // 1 2 3 4 5
        System.out.println();
        printNTo1(5);       // 1 2 3 4 5 (backtracking!)
    }
}
```

**Dry Run: print1ToN(1, 5)**
```
call(1,5): print 1, call(2,5)
  call(2,5): print 2, call(3,5)
    call(3,5): print 3, call(4,5)
      call(4,5): print 4, call(5,5)
        call(5,5): print 5, call(6,5)
          call(6,5): 6>5 → return
Output: 1 2 3 4 5 ✅
```

**Dry Run: printNTo1(5) — KEY INSIGHT!**
```
call(5): calls call(4) first, THEN prints 5
  call(4): calls call(3) first, THEN prints 4
    call(3): calls call(2) first, THEN prints 3
      call(2): calls call(1) first, THEN prints 2
        call(1): calls call(0) first, THEN prints 1
          call(0): return (base case)
        prints 1
      prints 2
    prints 3
  prints 4
prints 5
Output: 1 2 3 4 5 — prints in FORWARD order because printing happens on way back!
```

---

## 🧠 Key Insight: Print BEFORE vs AFTER recursion

```
BEFORE recursion → prints top to bottom (1,2,3,4,5)
AFTER recursion  → prints bottom to top (on unwind)

This is the foundation of understanding backtracking!
```

---

## 🎯 Final Summary

- Forward: print i, then recurse for i+1. Print BEFORE = forward order.
- Backward: recurse to base, print on way back. Print AFTER = reverse order.
- This teaches the crucial recursion unwinding concept
- Foundation for understanding DFS traversal in trees/graphs
