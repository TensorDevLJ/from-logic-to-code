# 465: KMP Algorithm (Knuth-Morris-Pratt) + LPS Array

**LeetCode:** https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/
**Difficulty:** 🔴 Hard | **Q# in sheet: 465**

---
## 🧠 Intuition
**Problem:** Find all occurrences of pattern P in text T.
Naive: O(N*M). KMP: O(N+M)!

**Core Idea — LPS (Longest Proper Prefix which is also Suffix) array:**
For pattern "AAACAAAA":
- LPS[0]=0 (A → no proper prefix=suffix)
- LPS[1]=1 (AA → "A" is both prefix and suffix)
- LPS[2]=2 (AAA → "AA" is both)
- LPS[3]=0 (AAAC → no match)
- LPS[4]=1 (AAACA → "A")
- LPS[5]=2 (AAACAA → "AA")
- LPS[6]=3 (AAACAAA → "AAA")
- LPS[7]=3 (AAACAAAA → still "AAA", not "AAAA" since that's whole string)

**When mismatch:** Don't start over! Jump back using LPS — skip chars we already know match.

---
## 🚀 Solution
```java
public class KMP {

    // Step 1: Build LPS array
    private static int[] buildLPS(String pattern) {
        int m = pattern.length();
        int[] lps = new int[m];
        lps[0] = 0;  // Always 0

        int len = 0;  // Length of previous longest prefix-suffix
        int i = 1;

        while (i < m) {
            if (pattern.charAt(i) == pattern.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            } else {
                if (len != 0) {
                    len = lps[len - 1];  // Fall back (don't increment i!)
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }
        return lps;
    }

    // Step 2: KMP Search
    public static List<Integer> search(String text, String pattern) {
        List<Integer> result = new ArrayList<>();
        int n = text.length(), m = pattern.length();
        int[] lps = buildLPS(pattern);

        int i = 0, j = 0;  // i = text index, j = pattern index
        while (i < n) {
            if (text.charAt(i) == pattern.charAt(j)) {
                i++; j++;
            }

            if (j == m) {
                result.add(i - j);  // Found at index i-j
                j = lps[j - 1];    // Use LPS to continue
            } else if (i < n && text.charAt(i) != pattern.charAt(j)) {
                if (j != 0) {
                    j = lps[j - 1]; // Jump back using LPS
                } else {
                    i++;            // Can't jump back further
                }
            }
        }
        return result;
    }

    public static void main(String[] args) {
        System.out.println(search("AABAACAADAABAAABAA", "AABA")); // [0, 9, 13]
        System.out.println(search("AAAABAAA", "AAAA"));            // [0, 4]
    }
}
```
**Time:** O(N+M) | **Space:** O(M) for LPS

---
## 📌 LPS Build Dry Run: pattern="AAACAAAA"
```
i=1,len=0: A==A → len=1, lps[1]=1, i=2
i=2,len=1: A==A → len=2, lps[2]=2, i=3
i=3,len=2: C!=A → len=lps[1]=1
i=3,len=1: C!=A → len=lps[0]=0
i=3,len=0: C!=A → lps[3]=0, i=4
i=4,len=0: A==A → len=1, lps[4]=1, i=5
i=5,len=1: A==A → len=2, lps[5]=2, i=6
i=6,len=2: A==A → len=3, lps[6]=3, i=7
i=7,len=3: A==A → len=4, lps[7]=4, i=8
LPS = [0,1,2,0,1,2,3,4]
```

---
## 🔁 Pattern Recognition
- KMP: string matching in O(N+M)
- LPS array: key component — avoid restarting from 0
- **Applications:** Find pattern in text, string periodicity, shortest period of string

---
## ⚠️ Common Mistakes
1. Forgetting to use `lps[j-1]` not `lps[j]` when match found
2. Not resetting j using LPS on mismatch (just moving i ahead)
3. Off-by-one in result index: `i - j` (not `i - m`)

---
## 🎯 Summary
- LPS[i] = length of longest proper prefix of pattern[0..i] that is also a suffix
- Build LPS: O(M) — double pointer approach
- Search: O(N) — use LPS to avoid redundant comparisons
- Total: O(N+M), Space: O(M)
