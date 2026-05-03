# 05. Armstrong Number

**LeetCode:** https://leetcode.com/problems/armstrong-number/
**GFG:** https://practice.geeksforgeeks.org/problems/armstrong-numbers2727/1
**Difficulty:** 🟢 Easy | **Pattern:** Digit Manipulation + Math

---

## 🧠 Intuition

**In Simple Words:**
A number is Armstrong if sum of its digits each raised to the power of (number of digits) equals the number itself.
153 → 1³ + 5³ + 3³ = 1 + 125 + 27 = 153 ✅ (3-digit Armstrong)
1634 → 1⁴ + 6⁴ + 3⁴ + 4⁴ = 1+1296+81+256 = 1634 ✅ (4-digit)

**Real-Life Analogy:**
Think of it as a "self-describing" number — it's built entirely by its own digits!

**Core Idea:**
1. Count digits (length k)
2. For each digit d: compute d^k
3. Sum all: if sum == original → Armstrong

---

## 📌 Problem Understanding

- **Input:** Integer N
- **Output:** true if Armstrong, false otherwise

| N | Digits | Calculation | Armstrong? |
|---|--------|-------------|------------|
| 153 | 3 | 1³+5³+3³=153 | ✅ |
| 370 | 3 | 3³+7³+0³=370 | ✅ |
| 1634 | 4 | 1⁴+6⁴+3⁴+4⁴=1634 | ✅ |
| 123 | 3 | 1³+2³+3³=36≠123 | ❌ |

---

## 🚀 Optimal Approach

```java
public class ArmstrongNumber {
    public static boolean isArmstrong(int n) {
        int original = n;

        // Step 1: Count digits
        int k = 0;
        int temp = n;
        while (temp > 0) {
            k++;
            temp /= 10;
        }

        // Step 2: Compute sum of digits^k
        int sum = 0;
        temp = n;
        while (temp > 0) {
            int digit = temp % 10;
            sum += (int) Math.pow(digit, k);  // digit^k
            temp /= 10;
        }

        // Step 3: Compare
        return sum == original;
    }

    public static void main(String[] args) {
        System.out.println(isArmstrong(153));   // true
        System.out.println(isArmstrong(1634));  // true
        System.out.println(isArmstrong(123));   // false
        System.out.println(isArmstrong(9));     // true (9^1=9)
    }
}
```

**Time:** O(d) where d = number of digits = O(log N)  
**Space:** O(1)

**Dry Run:** N = 153
```
Step 1: Count digits → k = 3
Step 2: 
  digit=3: 3^3=27, sum=27, temp=15
  digit=5: 5^3=125, sum=152, temp=1
  digit=1: 1^3=1, sum=153, temp=0
Step 3: sum(153) == original(153) → true ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Digit Manipulation (% 10 loop) + Math.pow
**Requires knowledge of:** Count digits → then digit extraction

---

## 🧠 How to Think (Interview View)

1. Know the definition: sum(digit^numDigits) == N
2. Two-pass: first count digits, then compute sum
3. Or one-pass if you know digit count upfront (log10)

---

## ⚠️ Common Mistakes

1. ❌ Using wrong power (k should be TOTAL digits, not local)
2. ❌ Not saving original before modifying n
3. ❌ Using float pow without casting (floating point errors)
4. ❌ Forgetting single digit: 1,2,...,9 all Armstrong (d^1=d)

---

## 🧩 Variations

1. **Find all Armstrong numbers up to N**
2. **Strong number** — sum of factorials of digits = number
3. **Perfect number** — sum of proper divisors = number
4. **Neon number** — sum of digits of n² = n

---

## 🧠 Memory Trick

**"ARM = All digits Raised to (# digits) and suMmed"**

---

## 🎯 Final Summary

- Armstrong: sum(each_digit ^ total_digit_count) == original number
- Two steps: count digits k, then sum digit^k
- Classic 3-digit examples: 153, 370, 371, 407
- Use Math.abs() for safety with pow
- Pattern: digit extraction + power computation
