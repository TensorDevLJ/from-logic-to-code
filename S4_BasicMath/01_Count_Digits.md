# 01. Count All Digits of a Number

**GFG:** https://practice.geeksforgeeks.org/problems/count-digits5716/1  
**Difficulty:** 🟢 Easy | **Pattern:** Digit Manipulation

---

## 🧠 Intuition

**In Simple Words:**
Given a number N, count how many digits it has.  
Example: 12345 → 5 digits | 1000 → 4 digits | 0 → 1 digit

**Real-Life Analogy:**
Imagine counting pages of a book by tearing off one page at a time and marking a tally. Each tear = one digit revealed.

**Core Idea:**
Every time you divide a number by 10, you strip off one digit from the right.  
Keep doing this until the number becomes 0. The count = number of digits.

---

## 📌 Problem Understanding

- **Input:** Integer N (can be 0 or negative)
- **Output:** Count of digits in N
- **Constraints:** -10^9 ≤ N ≤ 10^9

| Input | Output | Reason |
|-------|--------|--------|
| 12345 | 5 | Five digits |
| 0 | 1 | Zero has 1 digit |
| -456 | 3 | Ignore sign |
| 1000 | 4 | Including zeros |

---

## 🐢 Brute Force Approach

**Idea:** Convert the number to String and return its length.

**Step-by-step:**
1. Handle negative → take absolute value
2. Convert number to String
3. Return String.length()

```java
public class CountDigits {
    public static int countDigitsBrute(int n) {
        if (n == 0) return 1;           // Special case
        if (n < 0) n = Math.abs(n);     // Handle negative
        return String.valueOf(n).length(); // String trick
    }
}
```

**Time:** O(log₁₀N) — length of string = number of digits  
**Space:** O(log₁₀N) — for the string

**Dry Run:** N = 12345  
String.valueOf(12345) = "12345" → length = 5 ✅

---

## 🚀 Optimal Approach

**Idea:** Division Loop — divide by 10 until N becomes 0.

**Step-by-step (Logic → Code):**
1. If N == 0, return 1 (special case)
2. If N < 0, make it positive
3. While N > 0:
   - Divide N by 10 (removes last digit)
   - Increment count
4. Return count

```java
public class CountDigits {
    // Method 1: Division Loop — O(log N), O(1) space
    public static int countDigits(int n) {
        if (n == 0) return 1;
        if (n < 0) n = Math.abs(n);
        
        int count = 0;
        while (n > 0) {
            n = n / 10;   // Strip last digit
            count++;       // Count it
        }
        return count;
    }

    // Method 2: Log formula — O(1), O(1) space
    public static int countDigitsLog(int n) {
        if (n == 0) return 1;
        if (n < 0) n = Math.abs(n);
        return (int)(Math.log10(n)) + 1;
    }

    public static void main(String[] args) {
        System.out.println(countDigits(12345));  // 5
        System.out.println(countDigits(0));      // 1
        System.out.println(countDigits(-456));   // 3
    }
}
```

**Time:** O(log₁₀N) for loop, O(1) for log formula  
**Space:** O(1) both methods

**Dry Run (Division):**
```
N = 12345
Step 1: n = 12345/10 = 1234, count = 1
Step 2: n = 1234/10  = 123,  count = 2
Step 3: n = 123/10   = 12,   count = 3
Step 4: n = 12/10    = 1,    count = 4
Step 5: n = 1/10     = 0,    count = 5
n == 0 → stop. Return 5 ✅
```

**Dry Run (Log):**
```
N = 12345
log10(12345) = 4.0915...
(int)(4.0915) + 1 = 4 + 1 = 5 ✅
```

---

## 🔁 Pattern Recognition

- **Pattern:** Digit Manipulation (loop % 10 to get digit, /10 to remove)
- **Same pattern used in:** Reverse Number, Palindrome Number, Armstrong Number, Sum of Digits

---

## 🧠 How to Think (Interview View)

**Thought process:**
1. "I need to count digits" → How do I isolate digits?
2. `n % 10` = last digit → `n / 10` = remove last digit
3. Loop until n == 0, count each iteration

**Hints interviewer might give:**
- "Can you avoid string conversion?" → Division method
- "Can you do it in O(1)?" → Log₁₀ method
- "What if n = 0?" → Special case, return 1

---

## ⚠️ Common Mistakes

1. ❌ Forgetting n = 0 (Math.log10(0) = -Infinity → crash!)
2. ❌ Not handling negative numbers
3. ❌ Off-by-one in log formula (must add +1)
4. ❌ Using `n > 0` but not initializing count = 0

---

## 🧩 Variations

1. **Sum of Digits** — use `n % 10` to extract each digit and sum
2. **Count digits that divide N** — LeetCode 1837
3. **Armstrong Number** — uses digit count to compute power
4. **Product of digits**
5. **Digital Root** — keep summing digits until single digit

---

## 🧠 Memory Trick

**"% gets, / removes"**  
`n % 10` → **gets** the last digit  
`n / 10` → **removes** the last digit  
Loop until n = 0, count iterations = digit count

---

## 🎯 Final Summary

- Count digits = number of times you can divide by 10 before reaching 0
- Special case: 0 has exactly 1 digit
- Two approaches: Loop O(log N) or log₁₀ formula O(1)
- Always take absolute value for negatives
- Foundation for ALL digit manipulation problems (reverse, Armstrong, palindrome)
