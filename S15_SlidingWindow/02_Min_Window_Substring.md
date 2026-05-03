# 02. Minimum Window Substring

**LeetCode:** https://leetcode.com/problems/minimum-window-substring/
**Difficulty:** 🔴 Hard | **Pattern:** Sliding Window + HashMap

---

## 🧠 Intuition

**In Simple Words:**
Find the minimum length substring of s that contains ALL characters of t.
s="ADOBECODEBANC", t="ABC" → "BANC"

**Core Idea:**
Use two maps: one for required chars (from t), one for current window.
Expand right until all required chars covered. Then shrink left as much as possible.
Track minimum valid window.

---

## 🚀 Solution

```java
import java.util.*;

public class MinWindowSubstring {
    public static String minWindow(String s, String t) {
        if (s.isEmpty() || t.isEmpty()) return "";

        // Count required chars
        Map<Character, Integer> required = new HashMap<>();
        for (char c : t.toCharArray()) required.merge(c, 1, Integer::sum);

        int formed = 0;                  // How many unique chars satisfied
        int required_count = required.size(); // How many unique chars needed

        Map<Character, Integer> window = new HashMap<>();
        int left = 0, minLen = Integer.MAX_VALUE;
        int minLeft = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            window.merge(c, 1, Integer::sum);

            // Check if this char's count now satisfies requirement
            if (required.containsKey(c) && window.get(c).equals(required.get(c))) {
                formed++;
            }

            // Try to shrink from left when all chars satisfied
            while (formed == required_count && left <= right) {
                // Update minimum window
                if (right - left + 1 < minLen) {
                    minLen = right - left + 1;
                    minLeft = left;
                }

                // Remove leftmost char from window
                char leftChar = s.charAt(left);
                window.merge(leftChar, -1, Integer::sum);
                if (required.containsKey(leftChar) && window.get(leftChar) < required.get(leftChar)) {
                    formed--;  // No longer satisfied
                }
                left++;
            }
        }

        return minLen == Integer.MAX_VALUE ? "" : s.substring(minLeft, minLeft + minLen);
    }

    public static void main(String[] args) {
        System.out.println(minWindow("ADOBECODEBANC", "ABC")); // "BANC"
        System.out.println(minWindow("a", "a"));               // "a"
        System.out.println(minWindow("a", "aa"));              // ""
    }
}
```

**Time:** O(|S| + |T|) | **Space:** O(|S| + |T|)

**Dry Run:** s="ADOBECODEBANC", t="ABC"
```
required={A:1,B:1,C:1}, required_count=3

Expand right until formed=3:
right=0(A): window={A:1}, formed=1
right=1(D): window={A:1,D:1}
right=2(O): window+=O
right=3(B): window={B:1}, formed=2
right=4(E): ...
right=5(C): window={C:1}, formed=3 ✅ → try to shrink!
  Window "ADOBEC" (len 6). left=0 (A): remove A → formed=2 (A not satisfied). left=1
right=6(O): formed=2, keep expanding
right=7(D): ...
right=8(E): ...
right=9(B): window={B:2}, no change to formed
right=10(A): formed=3 ✅ → shrink!
  Window is "ODEBANC" (len 7). Shrink: remove O,D,E
  left=5(C): remove C → formed=2. left=6
right=11(N): ...
right=12(C): formed=3 ✅ → shrink!
  Window "BANC" (len 4). left=10(A): remove A → formed=2. left=11

Final min window: "BANC" ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Sliding Window with frequency maps and "formed" counter
Same pattern for:
- Contains All Characters
- Minimum Window Subsequence
- Permutation in String

---

## ⚠️ Common Mistakes

1. ❌ Using `==` instead of `.equals()` for Integer comparison
2. ❌ Updating `formed` for characters NOT in t
3. ❌ Off-by-one in substring extraction

---

## 🎯 Final Summary

- Two hashmaps: required (from t) and window (current)
- `formed` counter tracks satisfied requirements
- Expand right → shrink left when all formed → update min
- O(|S|+|T|) time — classic sliding window with counters
