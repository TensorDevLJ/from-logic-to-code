# 06. Print All Divisors of a Number

**GFG:** https://practice.geeksforgeeks.org/problems/all-divisors-of-a-number/1
**Difficulty:** 🟢 Easy | **Pattern:** Math / Square Root Optimization

---

## 🧠 Intuition

**In Simple Words:**
Find all numbers that divide N without remainder.
Divisors of 36 = {1, 2, 3, 4, 6, 9, 12, 18, 36}

**Real-Life Analogy:**
You have 36 balls and want to arrange them in a rectangle. Each valid arrangement (rows × cols) gives a divisor pair. Like 4×9, 6×6, 3×12...

**Core Idea:**
Divisors come in **pairs**: if i divides N, then N/i also divides N.
We only need to check i from 1 to √N, and collect both i and N/i.

---

## 📌 Problem Understanding

- **Input:** Integer N
- **Output:** List of all divisors (sorted or unsorted)
- **Constraints:** 1 ≤ N ≤ 10^9

**Example:** N = 36
Divisors = [1, 2, 3, 4, 6, 9, 12, 18, 36]

---

## 🐢 Brute Force

**Idea:** Loop from 1 to N, check each.

```java
public static List<Integer> divisorsBrute(int n) {
    List<Integer> result = new ArrayList<>();
    for (int i = 1; i <= n; i++) {
        if (n % i == 0) result.add(i);
    }
    return result; // Already sorted
}
```

**Time:** O(N) — too slow for N = 10^9  
**Space:** O(number of divisors)

---

## 🚀 Optimal: Square Root Trick

**Key Insight:**
If i divides N → N/i also divides N.
So divisors come in pairs (i, N/i). Only iterate up to √N!

```java
import java.util.*;

public class PrintDivisors {
    public static List<Integer> divisors(int n) {
        List<Integer> result = new ArrayList<>();

        for (int i = 1; (long)i * i <= n; i++) {  // i up to sqrt(n)
            if (n % i == 0) {
                result.add(i);          // i is a divisor
                if (i != n / i) {       // Avoid duplicate for perfect squares
                    result.add(n / i);  // n/i is also a divisor
                }
            }
        }

        Collections.sort(result);  // Sort if order required
        return result;
    }

    public static void main(String[] args) {
        System.out.println(divisors(36));   // [1, 2, 3, 4, 6, 9, 12, 18, 36]
        System.out.println(divisors(12));   // [1, 2, 3, 4, 6, 12]
        System.out.println(divisors(7));    // [1, 7]  (prime)
        System.out.println(divisors(1));    // [1]
    }
}
```

**Time:** O(√N) for loop + O(d log d) for sorting  
**Space:** O(d) where d = number of divisors

**Dry Run:** N = 36
```
i=1: 36%1==0 → add 1 and 36/1=36 (1≠36)
i=2: 36%2==0 → add 2 and 36/2=18 (2≠18)
i=3: 36%3==0 → add 3 and 36/3=12 (3≠12)
i=4: 36%4==0 → add 4 and 36/4=9 (4≠9)
i=5: 36%5≠0 → skip
i=6: 36%6==0 → add 6, 36/6=6 → 6==6, skip duplicate
i=7: 7*7=49 > 36 → STOP

result before sort: [1,36,2,18,3,12,4,9,6]
After sort: [1,2,3,4,6,9,12,18,36] ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Square Root Optimization — very common in math problems!
**Same pattern used in:** Check Prime, Count Divisors, Factor problems

---

## 🧠 How to Think (Interview View)

1. "Print divisors" → Brute O(N) obvious
2. "Can you do better?" → Think: divisors come in pairs! → √N
3. "Watch out for perfect squares" → i==N/i, add only once

**Key condition:** `(long)i * i <= n` — use long to avoid overflow when i*i overflows int!

---

## ⚠️ Common Mistakes

1. ❌ Using `i * i <= n` with int — overflow when n > 46340!
2. ❌ Adding duplicate for perfect squares (i == n/i)
3. ❌ Returning unsorted if sorted is required
4. ❌ Looping to n instead of √n in optimal

---

## 🧩 Variations

1. **Count divisors** — just count instead of storing
2. **Sum of divisors** — sum instead of storing
3. **Check perfect number** — sum of proper divisors = n
4. **Sieve of Eratosthenes** — find all primes up to N using similar idea

---

## 🧠 Memory Trick

**"SQRT PAIR: every divisor i has a buddy n/i"**
Loop only to √n, collect both i and n/i. Watch for perfect squares.

---

## 🎯 Final Summary

- Divisors come in pairs (i, n/i) → only loop to √n
- Use `(long)i*i <= n` to avoid int overflow
- Skip duplicate when i == n/i (perfect square case)
- Brute O(N) → Optimal O(√N) — huge improvement for large N
- This √N trick is reused in prime checking!
