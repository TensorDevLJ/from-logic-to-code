# 464: Z-Function for String Matching

**Difficulty:** 🔴 Hard | **Q# in sheet: 464**

---
## 🧠 Intuition
Z[i] = length of longest substring starting from s[i] that is also a prefix of s.
s="aabxaa" → Z=[6,1,0,0,2,1]

**How to use for pattern search:**
Combine pattern + "$" + text.
Z values >= len(pattern) at positions >= len(pattern)+1 = pattern occurrences!

```java
public class ZFunction {

    // Build Z array in O(N)
    public static int[] buildZ(String s) {
        int n = s.length();
        int[] z = new int[n];
        int l = 0, r = 0;

        for (int i = 1; i < n; i++) {
            if (i < r) z[i] = Math.min(r - i, z[i - l]);

            while (i + z[i] < n && s.charAt(z[i]) == s.charAt(i + z[i]))
                z[i]++;

            if (i + z[i] > r) { l = i; r = i + z[i]; }
        }
        return z;
    }

    // Search pattern in text
    public static List<Integer> search(String text, String pattern) {
        String combined = pattern + "$" + text;
        int[] z = buildZ(combined);
        int m = pattern.length();
        List<Integer> result = new ArrayList<>();

        for (int i = m + 1; i < combined.length(); i++)
            if (z[i] == m) result.add(i - m - 1);  // Found at text index

        return result;
    }

    public static void main(String[] args) {
        System.out.println(search("aabaabaab", "aab")); // [0, 3, 6]
    }
}
```
**Time:** O(N+M) | **Space:** O(N+M)

---
## 🎯 Summary
- Z[i] = longest prefix of s that matches starting at s[i]
- Pattern search: concatenate as "pattern$text", find Z[i] == pattern.length()
- O(N+M) using Z-box optimization
