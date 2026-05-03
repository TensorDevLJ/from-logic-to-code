# 03. Palindrome Number

**LeetCode:** https://leetcode.com/problems/palindrome-number/
**Difficulty:** 🟢 Easy | **Pattern:** Digit Manipulation

---

## 🧠 Intuition

**In Simple Words:**
A number is palindrome if it reads the same forwards and backwards.
121 → palindrome ✅ | 123 → not palindrome ❌

**Real-Life Analogy:**
Like reading "RACECAR" — same forwards and backwards. A palindrome number is the numeric version.

**Core Idea:**
Reverse the number. If reversed == original → palindrome.
Shortcut: Only reverse the second half and compare.

---

## 📌 Problem Understanding

- **Input:** Integer N
- **Output:** true if palindrome, false otherwise
- **Constraints:** Negative numbers are NOT palindromes (LeetCode rule)

| Input | Output | Reason |
|-------|--------|--------|
| 121 | true | 121 reversed = 121 |
| -121 | false | Negative → not palindrome |
| 10 | false | 10 reversed = 01 = 1 ≠ 10 |
| 0 | true | 0 reversed = 0 |

---

## 🐢 Brute Force

**Idea:** Reverse the entire number, compare with original.

```java
public static boolean isPalindromeBrute(int n) {
    if (n < 0) return false;       // Negative not palindrome
    int original = n;
    long reversed = 0;
    while (n != 0) {
        int digit = n % 10;
        reversed = reversed * 10 + digit;
        n /= 10;
    }
    return original == reversed;
}
```

**Time:** O(log N) | **Space:** O(1)

**Dry Run:** n = 121
reversed = 0→1→12→121. original=121==reversed=121 → true ✅

---

## 🚀 Optimal Approach (Half Reversal)

**Idea:** Only reverse the second half. This avoids overflow risk from reversing very large numbers.

**Step-by-step:**
1. Negative → false
2. Number ending in 0 (but ≠ 0) → false (can't be palindrome)
3. Reverse digits until reversed >= n (we've done the second half)
4. Check: n == reversed OR n == reversed/10 (for odd-length numbers)

```java
public class PalindromeNumber {
    public static boolean isPalindrome(int n) {
        // Edge cases
        if (n < 0) return false;
        if (n != 0 && n % 10 == 0) return false; // Trailing zero

        int reversed = 0;
        // Reverse second half only
        while (n > reversed) {
            reversed = reversed * 10 + n % 10;
            n /= 10;
        }

        // Even length: n == reversed
        // Odd length: n == reversed/10 (middle digit in reversed)
        return n == reversed || n == reversed / 10;
    }

    public static void main(String[] args) {
        System.out.println(isPalindrome(121));   // true
        System.out.println(isPalindrome(1221));  // true
        System.out.println(isPalindrome(-121));  // false
        System.out.println(isPalindrome(10));    // false
    }
}
```

**Time:** O(log N) | **Space:** O(1)

**Dry Run:** n = 1221 (even length)
```
Start: n=1221, rev=0
Step 1: rev = 0*10 + 1221%10 = 1, n = 122   → rev(1) < n(122)
Step 2: rev = 1*10 + 122%10  = 12, n = 12   → rev(12) == n(12) → STOP
n(12) == reversed(12) → true ✅
```

**Dry Run:** n = 121 (odd length)
```
Start: n=121, rev=0
Step 1: rev = 1,  n = 12  → 1 < 12
Step 2: rev = 12, n = 1   → 12 > 1 → STOP
n(1) == rev/10(12/10=1) → true ✅ (middle digit discarded)
```

---

## 🔁 Pattern Recognition

**Pattern:** Digit Manipulation → Reverse + Compare
**Same idea:** Reverse string palindrome check, but here with digits

---

## 🧠 How to Think (Interview View)

1. First instinct: Convert to string → but interviewer may say "no strings"
2. Then: Reverse the number and compare
3. Optimization: Only reverse half — avoids overflow, faster

**Hints:** "What about negative?" → Always false. "What about trailing zeros?" → False (except 0 itself)

---

## ⚠️ Common Mistakes

1. ❌ Treating negative as palindrome
2. ❌ Missing the n%10==0 edge case (e.g., 10, 100)
3. ❌ Using int for reversed (may overflow for large numbers)
4. ❌ Forgetting odd-length case in half-reversal

---

## 🧩 Variations

1. **Palindrome String** — same concept with characters
2. **Palindromic Substrings** — LeetCode 647
3. **Longest Palindromic Substring** — LeetCode 5
4. **Next Palindrome Number** — construct using digit manipulation

---

## 🧠 Memory Trick

**"Half and Check"**
Reverse only half the digits. When reversed >= n, you're done.
Even: n == rev | Odd: n == rev/10

---

## 🎯 Final Summary

- Negative numbers → never palindromes
- Trailing zero (except 0) → never palindrome
- Reverse full number and compare → simple O(log N)
- Optimal: Reverse only half, stop when rev >= n
- Odd length: compare n == rev/10 (drop middle digit)
