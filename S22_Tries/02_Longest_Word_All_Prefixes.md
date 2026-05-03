# 455: Longest Word With All Prefixes

**GFG:** https://practice.geeksforgeeks.org/problems/longest-word-in-dictionary-through-deleting/0
**LeetCode similar:** https://leetcode.com/problems/longest-word-in-dictionary/
**Difficulty:** 🟡 Medium | **Q# in sheet: 455**

---
## 🧠 Intuition
Find longest word in dictionary such that ALL its prefixes are also in the dictionary.
words=["a","ab","abc","abcd","abcde","b","bc"] → "abcde"

**Approach:** Insert all words in Trie. Then DFS/BFS to find longest word where every node along the path has `isEnd=true`.

```java
public String longestWord(String[] words) {
    Trie trie = new Trie();
    for (String w : words) trie.insert(w);

    String longest = "";
    // DFS through trie, only going to nodes where isEnd=true
    Deque<TrieNode> stack = new ArrayDeque<>();
    Deque<String> wordStack = new ArrayDeque<>();
    stack.push(trie.root);
    wordStack.push("");

    while (!stack.isEmpty()) {
        TrieNode node = stack.pop();
        String word = wordStack.pop();

        if (word.length() > longest.length() ||
            (word.length() == longest.length() && word.compareTo(longest) < 0))
            longest = word;

        for (int i = 0; i < 26; i++) {
            if (node.children[i] != null && node.children[i].isEnd) {
                stack.push(node.children[i]);
                wordStack.push(word + (char)('a' + i));
            }
        }
    }
    return longest;
}
```
**Time:** O(N×L) | **Space:** O(N×L)

---
## 🎯 Summary
- Insert all words in Trie
- DFS but only visit nodes with isEnd=true (every prefix must be a word)
- Track longest word (and lexicographically smallest if tie)
