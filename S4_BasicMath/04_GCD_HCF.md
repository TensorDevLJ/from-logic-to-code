# 04. GCD / HCF of Two Numbers

**GFG:** https://practice.geeksforgeeks.org/problems/gcd-of-two-numbers3459/1
**Difficulty:** 🟢 Easy | **Pattern:** Math / Euclidean Algorithm

---

## 🧠 Intuition

**In Simple Words:**
GCD (Greatest Common Divisor) = largest number that divides both a and b without remainder.
GCD(12, 8) = 4. Both 12 and 8 are divisible by 4, and 4 is the largest such number.

**Real-Life Analogy:**
You have 12 chocolates and 8 candies. You want to make equal groups with no leftovers. Maximum group size = GCD(12,8) = 4. You can make 4 groups.

**Core Idea:**
Euclidean Algorithm: GCD(a, b) = GCD(b, a % b)
This works because any common divisor of a and b also divides a % b.
Keep replacing (a,b) with (b, a%b) until b = 0. Answer = a.

---

## 📌 Problem Understanding

- **Input:** Two integers a, b
- **Output:** GCD of a and b
- **Constraints:** a, b ≥ 1

| a | b | GCD |
|---|---|-----|
| 12 | 8 | 4 |
| 14 | 21 | 7 |
| 17 | 5 | 1 (coprime) |
| 100 | 10 | 10 |

---

## 🐢 Brute Force

**Idea:** Check every number from 1 to min(a,b) and find largest divisor of both.

```java
public static int gcdBrute(int a, int b) {
    int gcd = 1;
    for (int i = 1; i <= Math.min(a, b); i++) {
        if (a % i == 0 && b % i == 0) {
            gcd = i;  // Update to largest found so far
        }
    }
    return gcd;
}
```

**Time:** O(min(a,b)) — slow for large numbers  
**Space:** O(1)

---

## ⚡ Better Approach

**Idea:** Start from min(a,b) and go down — find first common divisor.

```java
public static int gcdBetter(int a, int b) {
    for (int i = Math.min(a, b); i >= 1; i--) {
        if (a % i == 0 && b % i == 0) return i;
    }
    return 1;
}
```

**Time:** O(min(a,b)) worst case still  
**Space:** O(1)

---

## 🚀 Optimal: Euclidean Algorithm

**Mathematical Proof:**
If d divides both a and b, then d divides (a - b).
Extend: d also divides (a mod b) = a - (a/b)*b.
So GCD(a,b) = GCD(b, a mod b). Keep reducing until b = 0.

```java
public class GCD {
    // Iterative Euclidean (Preferred in interviews)
    public static int gcd(int a, int b) {
        while (b != 0) {
            int temp = b;
            b = a % b;   // New b = remainder
            a = temp;    // New a = old b
        }
        return a;  // When b = 0, a holds the GCD
    }

    // Recursive Euclidean
    public static int gcdRecursive(int a, int b) {
        if (b == 0) return a;       // Base case
        return gcdRecursive(b, a % b); // Recursive case
    }

    // LCM using GCD: LCM(a,b) = (a * b) / GCD(a,b)
    public static long lcm(int a, int b) {
        return (long)a * b / gcd(a, b);  // Cast to long to avoid overflow
    }

    public static void main(String[] args) {
        System.out.println(gcd(12, 8));    // 4
        System.out.println(gcd(14, 21));   // 7
        System.out.println(lcm(4, 6));     // 12
    }
}
```

**Time:** O(log(min(a,b))) — extremely fast (Fibonacci case is worst case)  
**Space:** O(1) iterative, O(log N) recursive stack

**Dry Run:** GCD(48, 18)
```
a=48, b=18 → a%b = 48%18 = 12 → (a,b) = (18, 12)
a=18, b=12 → a%b = 18%12 = 6  → (a,b) = (12, 6)
a=12, b=6  → a%b = 12%6  = 0  → (a,b) = (6, 0)
b = 0 → return a = 6
GCD(48, 18) = 6 ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Euclidean Algorithm — mathematical reduction
**Used in:** LCM, coprime check, fraction simplification, Bezout's theorem

---

## 🧠 How to Think (Interview View)

1. "GCD" → immediately think Euclidean Algorithm
2. Remember: GCD(a, 0) = a (base case)
3. LCM = (a * b) / GCD(a, b) — always compute via GCD

**Common interview follow-ups:**
- "What's LCM?" → Use GCD: lcm = a/gcd * b (avoid overflow!)
- "Check if coprime?" → GCD == 1 → coprime
- "GCD of array?" → Reduce pairwise: gcd(arr[0], arr[1], arr[2]...) 

---

## ⚠️ Common Mistakes

1. ❌ Using a*b/gcd for LCM without checking overflow → use (long)a * b / gcd
2. ❌ Stopping loop at wrong condition (b != 0 vs b > 0)
3. ❌ Not handling a = 0 or b = 0 (GCD(0, x) = x)
4. ❌ Modifying original variables without temp

---

## 🧩 Variations

1. **GCD of array** — fold with gcd: gcd(gcd(a,b), c)...
2. **LCM of array** — same with lcm
3. **Check coprime** — GCD == 1
4. **Simplify fraction** — divide numerator and denominator by GCD
5. **Largest divisor of all array elements** — GCD of array

---

## 🧠 Memory Trick

**"Euclidean: Keep dividing, b becomes new a, remainder becomes new b"**
GCD(a,b) → GCD(b, a%b) → GCD(...) → until b=0 → answer=a

---

## 🎯 Final Summary

- GCD = largest number dividing both a and b with no remainder
- Euclidean: GCD(a,b) = GCD(b, a%b), stop when b=0, return a
- Time: O(log min(a,b)) — very efficient
- LCM(a,b) = (a/gcd) * b to avoid overflow
- Used in: fraction simplification, array GCD, coprime checks
