# 02. Reverse a Number

**LeetCode:** https://leetcode.com/problems/reverse-integer/  
**Difficulty:** 🟢 Easy | **Pattern:** Digit Manipulation

---

## 🧠 Intuition

**In Simple Words:**
Given 12345, return 54321. Given 120, return 21 (leading zeros drop).

**Real-Life Analogy:**
Think of reading a phone number backwards. You read last digit first, build the number digit-by-digit in reverse.

**Core Idea:**
Extract last digit using `% 10`, build reversed number by multiplying current result by 10 and adding extracted digit.

---

## 📌 Problem Understanding

- **Input:** Integer N
- **Output:** Reversed integer
- **Constraints:** Handle overflow (LeetCode: 32-bit int)

| Input | Output |
|-------|--------|
| 12345 | 54321 |
| -123 | -321 |
| 120 | 21 |
| 0 | 0 |

---

## 🐢 Brute Force

**Idea:** Convert to string, reverse string, convert back.

```java
public static int reverseBrute(int n) {
    boolean negative = n < 0;
    String s = String.valueOf(Math.abs(n));
    String reversed = new StringBuilder(s).reverse().toString();
    int result = Integer.parseInt(reversed);
    return negative ? -result : result;
}
```

**Time:** O(d) where d = number of digits  
**Space:** O(d) for string

---

## 🚀 Optimal Approach

**Idea:** Use digit extraction loop. Extract last digit, append to result.

**How to convert logic to code:**
- `digit = n % 10` → get last digit
- `reversed = reversed * 10 + digit` → append digit to right
- `n = n / 10` → remove last digit
- Repeat until n = 0

```java
public class ReverseNumber {
    public static int reverse(int n) {
        long reversed = 0;  // Use long to detect overflow
        boolean negative = n < 0;
        if (n < 0) n = Math.abs(n);  // Work with positive

        while (n != 0) {
            int digit = n % 10;           // Extract last digit
            reversed = reversed * 10 + digit; // Build reversed
            n = n / 10;                   // Remove last digit
        }

        // Check 32-bit overflow (LeetCode constraint)
        if (reversed > Integer.MAX_VALUE) return 0;

        return negative ? (int)-reversed : (int)reversed;
    }

    public static void main(String[] args) {
        System.out.println(reverse(12345));  // 54321
        System.out.println(reverse(-123));   // -321
        System.out.println(reverse(120));    // 21
    }
}
```

**Time:** O(log₁₀N)  
**Space:** O(1)

**Dry Run:** N = 12345
```
Iteration 1: digit = 5, reversed = 0*10+5 = 5,     n = 1234
Iteration 2: digit = 4, reversed = 5*10+4 = 54,    n = 123
Iteration 3: digit = 3, reversed = 54*10+3 = 543,  n = 12
Iteration 4: digit = 2, reversed = 543*10+2 = 5432, n = 1
Iteration 5: digit = 1, reversed = 5432*10+1 = 54321, n = 0
Return 54321 ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Digit Manipulation — **% 10 + /10 loop**  
This same loop is used in: Palindrome Number, Armstrong, Count Digits, Sum of Digits

---

## 🧠 How to Think (Interview View)

1. "I need to reverse digits" → think of building a number from right to left
2. `% 10` gives me the last digit of original = first digit of reversed
3. `* 10 + digit` places digit at the right position in reversed
4. Ask about overflow — always use `long` for intermediate

---

## ⚠️ Common Mistakes

1. ❌ Using `int` for reversed (overflow on large numbers)
2. ❌ Not handling negative numbers
3. ❌ Not considering trailing zeros (120 → 21, not 021)
4. ❌ Forgetting to return 0 on overflow (LeetCode requirement)

---

## 🧩 Variations

1. **Palindrome Number** — reverse and check if equal to original
2. **Reverse digits of a long number**
3. **Next Palindrome** — uses digit reversal concept

---

## 🧠 Memory Trick

**"REV = REV * 10 + LAST"**  
Every iteration: multiply result by 10 (shift left), add last digit

---

## 🎯 Final Summary

- Extract last digit: `n % 10`, Remove last digit: `n /= 10`
- Build reversed: `rev = rev * 10 + digit`
- Use `long` to handle overflow, cast to `int` at end
- Trailing zeros in original = leading zeros in reversed (auto-dropped)
- Pattern: % and / loop — used in ALL digit manipulation problems
