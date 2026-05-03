# 07. Check if a Number is Prime

**LeetCode:** https://leetcode.com/problems/count-primes/ (related)
**GFG:** https://practice.geeksforgeeks.org/problems/prime-number2314/1
**Difficulty:** 🟢 Easy | **Pattern:** Math / Square Root Optimization

---

## 🧠 Intuition

**In Simple Words:**
A prime number has exactly 2 divisors: 1 and itself.
2, 3, 5, 7, 11, 13... are prime.
1 is NOT prime (only 1 divisor). 4 = 2×2, not prime.

**Real-Life Analogy:**
A prime number is like a person with no friends except themselves and a stranger named 1. No other number "goes into" it evenly.

**Core Idea:**
Check if N has any divisor between 2 and √N.
If it does → not prime. If no divisor found → prime!
Why √N? Because divisors pair up (i, N/i). If there's a factor > √N, its pair < √N already found it.

---

## 📌 Problem Understanding

- **Input:** Integer N
- **Output:** true if prime, false otherwise
- **Constraints:** N ≥ 1

| N | Prime? | Reason |
|---|--------|--------|
| 1 | false | Only 1 divisor |
| 2 | true | Divisors: 1, 2 |
| 4 | false | 4 = 2×2 |
| 17 | true | No factors 2-4 |

---

## 🐢 Brute Force

```java
public static boolean isPrimeBrute(int n) {
    if (n <= 1) return false;
    for (int i = 2; i < n; i++) {  // Check all from 2 to n-1
        if (n % i == 0) return false;
    }
    return true;
}
```

**Time:** O(N) | **Space:** O(1) — too slow for large N!

---

## 🚀 Optimal: Square Root Check

```java
public class PrimeCheck {
    public static boolean isPrime(int n) {
        if (n <= 1) return false;    // 0, 1 not prime
        if (n == 2) return true;     // 2 is prime
        if (n % 2 == 0) return false; // Even > 2 → not prime

        // Only check odd numbers from 3 to sqrt(n)
        for (int i = 3; (long)i * i <= n; i += 2) {
            if (n % i == 0) return false;
        }
        return true;
    }

    // Bonus: Sieve of Eratosthenes for multiple prime checks
    public static boolean[] sieve(int limit) {
        boolean[] isPrime = new boolean[limit + 1];
        Arrays.fill(isPrime, true);
        isPrime[0] = isPrime[1] = false;

        for (int i = 2; (long)i * i <= limit; i++) {
            if (isPrime[i]) {
                for (int j = i * i; j <= limit; j += i) {
                    isPrime[j] = false; // Mark multiples of i
                }
            }
        }
        return isPrime;
    }

    public static void main(String[] args) {
        System.out.println(isPrime(2));   // true
        System.out.println(isPrime(17));  // true
        System.out.println(isPrime(100)); // false
        System.out.println(isPrime(1));   // false
    }
}
```

**Time:** O(√N) for single check, O(N log log N) for Sieve  
**Space:** O(1) single check, O(N) for Sieve

**Dry Run:** N = 17
```
n=17, not ≤1, not ==2, not even
Loop: i=3, i*i=9 ≤ 17 → 17%3 = 2 ≠ 0
      i=5, i*i=25 > 17 → STOP
return true ✅ (17 is prime)
```

**Dry Run:** N = 49
```
n=49, odd
Loop: i=3, 49%3=1 ≠ 0
      i=5, 49%5=4 ≠ 0
      i=7, 7*7=49 ≤ 49 → 49%7=0 → return false ✅ (49=7×7)
```

---

## 🔁 Pattern Recognition

**Pattern:** Square Root Optimization (same as Divisors!)
**Also used in:** Sieve of Eratosthenes, Goldbach's conjecture problems

---

## 🧠 How to Think (Interview View)

1. First: Handle edge cases (≤1, =2, even)
2. Then: Loop from 3 to √N, skip evens (i += 2)
3. For many queries: Sieve is the way

**Optimization trick:** Skip even numbers after 2 — cuts iterations in half!

---

## ⚠️ Common Mistakes

1. ❌ Returning true for 1 (1 is NOT prime!)
2. ❌ Using `i < n` instead of `i * i <= n`
3. ❌ Overflow: `i * i` can overflow int — use `(long)i * i`
4. ❌ Not handling 2 separately when skipping evens

---

## 🧩 Variations

1. **Count primes up to N** → Sieve of Eratosthenes
2. **Prime factorization** → trial division
3. **Next prime** → check isPrime(n+1), n+2...
4. **Sum of primes in range** → Sieve
5. **Twin primes** → both p and p+2 prime

---

## 🧠 Memory Trick

**"Check up to SQUARE ROOT — if no factor found by then, it's prime!"**
Because if N = a × b and a ≤ b, then a ≤ √N.

---

## 🎯 Final Summary

- Prime = exactly 2 divisors (1 and itself). 1 is NOT prime.
- Check divisors only up to √N — O(√N) vs O(N) brute force
- Skip even numbers after checking 2 — halves the work
- For bulk prime queries: Sieve of Eratosthenes O(N log log N)
- Key trick: `(long)i*i <= n` to avoid overflow
