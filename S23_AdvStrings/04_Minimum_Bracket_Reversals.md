# 460: Minimum Bracket Reversals

**GFG:** https://practice.geeksforgeeks.org/problems/count-the-reversals/0
**Difficulty:** 🔴 Hard | **Q# in sheet: 460**

---
## 🧠 Intuition
Count minimum bracket reversals to make string of { and } balanced.
If odd length → impossible (-1).

**Algorithm:**
1. Remove all matched pairs (like valid parentheses check using stack)
2. Remaining unmatched: some open {, some close }
   - Unmatched close } must come before unmatched {
   - Format: }}}...{{{
3. Each pair }} needs 1 reversal, each pair {{ needs 1 reversal
   → `ceil(unmatched_close/2) + ceil(unmatched_open/2)`

```java
public int countRev(String s) {
    int n = s.length();
    if (n % 2 != 0) return -1;  // Odd length → impossible

    int open = 0, close = 0;
    for (char c : s.toCharArray()) {
        if (c == '{') {
            open++;
        } else {
            if (open > 0) open--;  // Match with existing open
            else close++;          // Unmatched close
        }
    }
    // open = unmatched {, close = unmatched }
    return (open + 1) / 2 + (close + 1) / 2;
}
```
**Time:** O(N) | **Space:** O(1)

---
## 📌 Example
s = "}}{{"  → close=2 unmatched, open=2 unmatched
answer = ceil(2/2) + ceil(2/2) = 1 + 1 = 2

---
## 🎯 Summary
- Odd length → -1
- Track unmatched open and close brackets
- Answer = ceil(unmatched_close/2) + ceil(unmatched_open/2)
