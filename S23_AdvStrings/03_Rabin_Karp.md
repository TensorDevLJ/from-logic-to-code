# 463: Rabin-Karp Algorithm (Rolling Hash)

**LeetCode:** https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/
**Difficulty:** 🔴 Hard | **Q# in sheet: 463**

---
## 🧠 Intuition
**Rolling Hash:** Compute hash of pattern. Slide window over text, recompute hash efficiently.
If hash matches → verify (handle collisions). Average O(N+M).

**Rolling Hash Formula:**
hash(s[i..i+m-1]) = (s[i]*p^(m-1) + s[i+1]*p^(m-2) + ... + s[i+m-1]) mod MOD

**Sliding:** Remove leftmost char, add new rightmost char in O(1).

```java
public class RabinKarp {
    public static List<Integer> search(String text, String pattern) {
        List<Integer> result = new ArrayList<>();
        int n = text.length(), m = pattern.length();
        if (m > n) return result;

        long MOD = 1_000_000_007L;
        long BASE = 31;
        long power = 1;

        // Compute BASE^(m-1) mod MOD
        for (int i = 0; i < m - 1; i++) power = (power * BASE) % MOD;

        // Compute hash of pattern and first window
        long patHash = 0, winHash = 0;
        for (int i = 0; i < m; i++) {
            patHash = (patHash * BASE + pattern.charAt(i)) % MOD;
            winHash = (winHash * BASE + text.charAt(i)) % MOD;
        }

        for (int i = 0; i <= n - m; i++) {
            if (winHash == patHash) {
                // Verify (handle hash collision)
                if (text.substring(i, i + m).equals(pattern))
                    result.add(i);
            }

            // Slide window (rolling hash)
            if (i < n - m) {
                winHash = (winHash - text.charAt(i) * power % MOD + MOD) % MOD;
                winHash = (winHash * BASE + text.charAt(i + m)) % MOD;
            }
        }
        return result;
    }

    public static void main(String[] args) {
        System.out.println(search("GEEKS FOR GEEKS", "GEEK")); // [0, 10]
    }
}
```
**Time:** O(N+M) average, O(NM) worst case | **Space:** O(1)

---
## 🔑 Key: Rolling Hash Sliding
```
Remove leftmost: winHash -= text[i] * BASE^(m-1)
Add rightmost:   winHash = winHash * BASE + text[i+m]
Apply mod throughout to avoid overflow!
```

---
## 🎯 Summary
- Compute hash of pattern, slide window with rolling hash
- Hash match → verify (string comparison) to handle collisions
- O(N+M) average — great for multiple pattern searches
- Use large MOD and BASE to minimize collisions
