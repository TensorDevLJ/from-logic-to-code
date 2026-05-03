# 02. Fibonacci Number

**LeetCode:** https://leetcode.com/problems/fibonacci-number/
**Difficulty:** 🟢 Easy | **Pattern:** Recursion / DP Introduction

---

## 🧠 Intuition

**In Simple Words:**
Fibonacci: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34...
Each number = sum of previous two.
F(0)=0, F(1)=1, F(n) = F(n-1) + F(n-2)

**Real-Life Analogy:**
Rabbit population: Start with 1 pair. Each month, every adult pair breeds one new pair. Month counts match Fibonacci sequence!

**Core Idea:**
fib(n) = fib(n-1) + fib(n-2)
Two base cases: fib(0)=0, fib(1)=1

---

## 📌 Problem Understanding

| N | F(N) |
|---|------|
| 0 | 0 |
| 1 | 1 |
| 5 | 5 |
| 10 | 55 |

---

## 🐢 Naive Recursion (DON'T use in interviews alone!)

```java
public static int fibNaive(int n) {
    if (n <= 1) return n;  // Base cases
    return fibNaive(n-1) + fibNaive(n-2);
}
```

**Time:** O(2^N) — exponential! Terrible for large N.  
**Problem:** Recalculates same values repeatedly!
```
fib(5) calls fib(4) and fib(3)
fib(4) calls fib(3) and fib(2)  ← fib(3) computed TWICE!
```

---

## ⚡ Better: Memoization (Top-Down DP)

```java
public static int fibMemo(int n, int[] memo) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];  // Already computed!
    memo[n] = fibMemo(n-1, memo) + fibMemo(n-2, memo);
    return memo[n];
}
// Call: fibMemo(n, new int[n+1] filled with -1)
```

**Time:** O(N) — each value computed once  
**Space:** O(N) memo + O(N) stack

---

## 🚀 Optimal: Iterative (Bottom-Up DP)

```java
public class Fibonacci {
    public static int fib(int n) {
        if (n <= 1) return n;

        int prev2 = 0;  // F(0)
        int prev1 = 1;  // F(1)

        for (int i = 2; i <= n; i++) {
            int curr = prev1 + prev2;  // F(i) = F(i-1) + F(i-2)
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }

    public static void main(String[] args) {
        for (int i = 0; i <= 10; i++) {
            System.out.print(fib(i) + " ");
        }
        // Output: 0 1 1 2 3 5 8 13 21 34 55
    }
}
```

**Time:** O(N) | **Space:** O(1) — only two variables!

**Dry Run:** fib(6)
```
prev2=0, prev1=1
i=2: curr=0+1=1, prev2=1, prev1=1   (F(2)=1)
i=3: curr=1+1=2, prev2=1, prev1=2   (F(3)=2)
i=4: curr=1+2=3, prev2=2, prev1=3   (F(4)=3)
i=5: curr=2+3=5, prev2=3, prev1=5   (F(5)=5)
i=6: curr=3+5=8, prev2=5, prev1=8   (F(6)=8)
return 8 ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** This is the GATEWAY to Dynamic Programming!
- Overlapping subproblems (same fib values computed multiple times)
- Optimal substructure (fib(n) built from fib(n-1), fib(n-2))
- DP techniques: memoization (top-down) or tabulation (bottom-up)

---

## 🧠 How to Think (Interview View)

1. Naive recursion → exponential → show you know it's bad
2. Memoization → fix overlapping subproblems
3. Tabulation → iterative, O(1) space
4. "What's the space optimization?" → Just two variables!

---

## ⚠️ Common Mistakes

1. ❌ Using naive recursion for large N (TLE!)
2. ❌ Base case: return n (not return n-1 or n+1)
3. ❌ Forgetting to initialize memo array
4. ❌ Off-by-one in loop (i=2 to n inclusive)

---

## 🧩 Variations

1. **Climbing Stairs** — LeetCode 70: same as Fibonacci!
2. **Frog Jump** — Striver problem: DP on Fibonacci idea
3. **Min cost climbing stairs** — LeetCode 746
4. **Tribonacci** — 3 previous terms

---

## 🧠 Memory Trick

**"F = PREV + PREV-PREV"**
Roll two variables: update both after each iteration.

---

## 🎯 Final Summary

- F(n) = F(n-1) + F(n-2), F(0)=0, F(1)=1
- Naive recursion: O(2^N) — avoid in interviews
- Memoized: O(N) time, O(N) space — top-down DP
- Iterative: O(N) time, O(1) space — best approach
- KEY INSIGHT: This IS DP — climbing stairs, house robber all same idea!
