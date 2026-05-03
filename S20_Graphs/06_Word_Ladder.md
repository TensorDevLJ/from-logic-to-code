# 360–361: Word Ladder I & II

**LeetCode I:** https://leetcode.com/problems/word-ladder/
**LeetCode II:** https://leetcode.com/problems/word-ladder-ii/
**Difficulty:** 🔴 Hard | **Q# in sheet: 360–361**

---
## 🧠 Intuition
Transform beginWord → endWord changing one letter at a time. Each intermediate word must be in wordList.
Find the SHORTEST transformation sequence length (Word Ladder I).

**Model as Graph:** Each word = node. Edge between words that differ by exactly 1 character.
Shortest path = BFS.

---
## 📌 Example
```
beginWord="hit", endWord="cog"
wordList=["hot","dot","dog","lot","log","cog"]
Answer: 5  →  hit→hot→dot→dog→cog
```

---
## 🚀 Word Ladder I — BFS
```java
public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    Set<String> wordSet = new HashSet<>(wordList);
    if (!wordSet.contains(endWord)) return 0;

    Queue<String> q = new LinkedList<>();
    q.offer(beginWord);
    int steps = 1;

    while (!q.isEmpty()) {
        int size = q.size();
        for (int k = 0; k < size; k++) {
            String word = q.poll();
            char[] chars = word.toCharArray();

            // Try changing each character to 'a'-'z'
            for (int i = 0; i < chars.length; i++) {
                char orig = chars[i];
                for (char c = 'a'; c <= 'z'; c++) {
                    if (c == orig) continue;
                    chars[i] = c;
                    String newWord = new String(chars);
                    if (newWord.equals(endWord)) return steps + 1;
                    if (wordSet.contains(newWord)) {
                        wordSet.remove(newWord);  // Mark visited
                        q.offer(newWord);
                    }
                }
                chars[i] = orig;  // Restore
            }
        }
        steps++;
    }
    return 0;
}
```
**Time:** O(N × L × 26) where N=wordList size, L=word length
**Space:** O(N)

---
## 🔑 Key Tricks
- Remove word from set when visited (don't use separate visited set)
- Try ALL 26 char replacements for each position
- Level-by-level BFS = shortest path automatically

---
## 🎯 Summary
- BFS on implicit graph (words differing by 1 letter = neighbors)
- Change each char to a-z, check if in wordSet
- Remove from set on visit (marks visited, avoids revisit)
- Word Ladder II: BFS for levels + DFS/backtrack to collect all paths
