# 01. Factorial of N

**LeetCode:** https://leetcode.com/problems/factorial-trailing-zeroes/ (related)
**GFG:** https://practice.geeksforgeeks.org/problems/factorial5739/1
**Difficulty:** 🟢 Easy | **Pattern:** Basic Recursion

---

## 🧠 Intuition

**In Simple Words:**
Factorial of N = N × (N-1) × (N-2) × ... × 1
5! = 5 × 4 × 3 × 2 × 1 = 120

**Real-Life Analogy:**
How many ways can 5 people sit in a row? First seat: 5 choices. Second: 4. Third: 3... = 5! = 120 arrangements.

**Core Recursive Idea:**
factorial(n) = n × factorial(n-1)
factorial(1) = 1 (base case)
"The factorial of n is n times the factorial of one less than n."

---

## 📌 Problem Understanding

- **Input:** Non-negative integer N
- **Output:** N! (factorial)
- **Constraints:** 0 ≤ N ≤ 20 (fits in long)

| N | N! |
|---|----|
| 0 | 1 |
| 1 | 1 |
| 5 | 120 |
| 10 | 3628800 |

---

## 🐢 Iterative Approach

```java
public static long factorialIterative(int n) {
    long result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}
```

**Time:** O(N) | **Space:** O(1)

---

## 🚀 Recursive Approach

**How to convert to code:**
1. Base case: if n == 0 or n == 1 → return 1
2. Recursive case: return n * factorial(n-1)

```java
public class Factorial {
    public static long factorial(int n) {
        // Base case: 0! = 1, 1! = 1
        if (n <= 1) return 1;

        // Recursive case: n! = n × (n-1)!
        return (long) n * factorial(n - 1);
    }

    public static void main(String[] args) {
        System.out.println(factorial(0));   // 1
        System.out.println(factorial(5));   // 120
        System.out.println(factorial(10));  // 3628800
    }
}
```

**Time:** O(N) | **Space:** O(N) — call stack

**Dry Run:** factorial(5)
```
factorial(5) = 5 × factorial(4)
                   factorial(4) = 4 × factorial(3)
                                      factorial(3) = 3 × factorial(2)
                                                         factorial(2) = 2 × factorial(1)
                                                                            factorial(1) = 1
                                                         returns 2 × 1 = 2
                                         returns 3 × 2 = 6
                   returns 4 × 6 = 24
returns 5 × 24 = 120 ✅
```

---

## 🔁 How Recursion Works (CRUCIAL)

**The call stack:**
```
Call stack grows DOWN:
  factorial(5) → waiting for factorial(4)
    factorial(4) → waiting for factorial(3)
      factorial(3) → waiting for factorial(2)
        factorial(2) → waiting for factorial(1)
          factorial(1) → returns 1 (BASE CASE)
        ← 2 × 1 = 2 returned
      ← 3 × 2 = 6 returned
    ← 4 × 6 = 24 returned
  ← 5 × 24 = 120 returned
```

---

## 🧠 How to Think (Interview View)

**Recursion Template:**
1. What is the base case? (smallest/simplest version)
2. What is the recursive case? (relate to smaller problem)
3. Trust that the recursion works for smaller inputs!

For factorial: "If I know factorial(n-1), how do I get factorial(n)?"
Answer: "Just multiply by n!"

---

## ⚠️ Common Mistakes

1. ❌ Wrong base case (missing n==0 or using n==0 return 0)
2. ❌ Using int instead of long (overflows at n>12)
3. ❌ No base case → infinite recursion → StackOverflow!
4. ❌ base case as n==1 alone (missing 0! = 1)

---

## 🧩 Variations

1. **Count trailing zeros in N!** — LeetCode 172: count factors of 5
2. **Factorial of large number** — use BigInteger or arrays
3. **Stirling's approximation** — estimate large factorials
4. **Catalan numbers** — involve factorials

---

## 🧠 Memory Trick

**"Factorial = number × previous factorial"**
`f(n) = n * f(n-1)`, base: `f(0) = f(1) = 1`

---

## 🎯 Final Summary

- N! = N × (N-1) × ... × 1, and 0! = 1 by definition
- Recursive: f(n) = n * f(n-1), base: f(0)=f(1)=1
- Time O(N), Space O(N) for recursive (call stack)
- Use long — int overflows after 12!
- Template for ALL recursion: base case + recursive case
