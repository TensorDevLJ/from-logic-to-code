# 01. Longest Substring Without Repeating Characters

**LeetCode:** https://leetcode.com/problems/longest-substring-without-repeating-characters/
**Difficulty:** 🟡 Medium | **Pattern:** Sliding Window + HashMap

---

## 🧠 Intuition

**In Simple Words:**
Find the longest substring with all unique characters.
"abcabcbb" → "abc" = length 3

**Real-Life Analogy:**
Slide a window along a string. Expand right. When duplicate found, shrink from left until no duplicate.

**Core Idea:**
Use two pointers (left, right) forming a window.
Use HashMap to track last seen position of each character.
When we see a duplicate, jump left pointer past the previous occurrence.

---

## 🚀 Optimal Solution

```java
import java.util.*;

public class LongestSubstringNoRepeat {
    public static int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> lastSeen = new HashMap<>();  // char → last index
        int maxLen = 0;
        int left = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);

            // If char was seen and is WITHIN current window
            if (lastSeen.containsKey(c) && lastSeen.get(c) >= left) {
                left = lastSeen.get(c) + 1;  // Shrink window past duplicate
            }

            lastSeen.put(c, right);  // Update last seen position
            maxLen = Math.max(maxLen, right - left + 1);  // Window size
        }

        return maxLen;
    }

    public static void main(String[] args) {
        System.out.println(lengthOfLongestSubstring("abcabcbb")); // 3 (abc)
        System.out.println(lengthOfLongestSubstring("bbbbb"));    // 1 (b)
        System.out.println(lengthOfLongestSubstring("pwwkew"));   // 3 (wke)
        System.out.println(lengthOfLongestSubstring(""));          // 0
    }
}
```

**Time:** O(N) | **Space:** O(min(N, charset)) — 26 for lowercase letters, 128 for ASCII

**Dry Run:** "abcabcbb"
```
left=0, maxLen=0
right=0 (a): not seen. map={a:0}. maxLen=1
right=1 (b): not seen. map={a:0,b:1}. maxLen=2
right=2 (c): not seen. map={a:0,b:1,c:2}. maxLen=3
right=3 (a): seen at 0, 0>=left(0) → left=1. map={a:3,b:1,c:2}. maxLen=3
right=4 (b): seen at 1, 1>=left(1) → left=2. map={a:3,b:4,c:2}. maxLen=3
right=5 (c): seen at 2, 2>=left(2) → left=3. map={a:3,b:4,c:5}. maxLen=3
right=6 (b): seen at 4, 4>=left(3) → left=5. maxLen=3
right=7 (b): seen at 6, 6>=left(5) → left=7. maxLen=3
Final: 3 ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Sliding Window with HashMap
**Template:**
```
left = 0
for right in range(n):
    // expand window
    while window_invalid:
        // shrink from left
        left++
    // update answer
    maxLen = max(maxLen, right - left + 1)
```

---

## 🧠 How to Think (Interview View)

1. "Longest substring with condition" → Sliding Window!
2. Use map to track characters in window
3. When violation: move left past the violating character

**Key optimization:** Instead of moving left one step at a time, jump directly to `lastSeen[c] + 1`

---

## ⚠️ Common Mistakes

1. ❌ Not checking `lastSeen.get(c) >= left` — char might be outside current window!
2. ❌ Using HashSet and while loop (works but O(N) worse case)
3. ❌ Forgetting to update lastSeen after moving left

---

## 🧩 Variations

1. **Longest Substring with At Most K Distinct Chars** — LeetCode 340
2. **Minimum Window Substring** — LeetCode 76
3. **Longest Repeating Char Replacement** — LeetCode 424
4. **Fruit Into Baskets** — LeetCode 904 (at most 2 distinct)

---

## 🧠 Memory Trick

**"EXPAND right freely, SHRINK left when duplicate found"**
HashMap tells you WHERE the duplicate was — jump there directly!

---

## 🎯 Final Summary

- Sliding window: expand right, shrink left on duplicates
- HashMap stores last seen index of each char
- Jump left to lastSeen[c]+1 (not step-by-step — faster!)
- Check `lastSeen.get(c) >= left` before updating left
- O(N) time, O(1) space if charset fixed (26 letters)
