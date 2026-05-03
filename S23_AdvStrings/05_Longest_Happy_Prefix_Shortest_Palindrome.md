# 466–467: Longest Happy Prefix & Shortest Palindrome

---
## 🧠 Q467: Longest Happy Prefix (LeetCode 1392)
https://leetcode.com/problems/longest-happy-prefix/

**Problem:** Find longest prefix that is also a suffix (not the whole string).
This is exactly the **KMP LPS array** last value!

```java
public String longestPrefix(String s) {
    int n = s.length();
    int[] lps = new int[n];
    int len = 0, i = 1;

    while (i < n) {
        if (s.charAt(i) == s.charAt(len)) {
            lps[i++] = ++len;
        } else if (len != 0) {
            len = lps[len - 1];
        } else {
            lps[i++] = 0;
        }
    }
    return s.substring(0, lps[n-1]);  // lps[n-1] = length of longest happy prefix
}
```
**Time:** O(N) | **Space:** O(N)

---
## 🧠 Q466: Shortest Palindrome (LeetCode 214)
https://leetcode.com/problems/shortest-palindrome/

**Problem:** Add min chars to FRONT of string to make it palindrome.
"aacecaaa" → "aaacecaaa"

**Trick:** Find longest palindrome prefix. The suffix after it needs to be reversed and prepended.
Use KMP: combine `s + "#" + reverse(s)`, find LPS. LPS last value = longest palindrome prefix length.

```java
public String shortestPalindrome(String s) {
    String rev = new StringBuilder(s).reverse().toString();
    String combined = s + "#" + rev;
    int n = combined.length();

    int[] lps = new int[n];
    int len = 0, i = 1;
    while (i < n) {
        if (combined.charAt(i) == combined.charAt(len)) lps[i++] = ++len;
        else if (len != 0) len = lps[len-1];
        else lps[i++] = 0;
    }

    int palindromeLen = lps[n-1];
    // Chars after palindrome prefix: reverse and prepend
    return rev.substring(0, s.length() - palindromeLen) + s;
}
```
**Time:** O(N) | **Space:** O(N)

---
## 🎯 Summary
- Longest Happy Prefix = KMP LPS last value
- Shortest Palindrome: find longest palindrome prefix using KMP on s+"#"+rev(s)
- Both reduce to computing LPS array — KMP core concept reused!
