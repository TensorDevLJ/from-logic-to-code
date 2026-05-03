# 456: Number of Distinct Substrings in a String

**GFG:** https://practice.geeksforgeeks.org/problems/number-of-distinct-substrings-in-a-string/1
**Difficulty:** 🟡 Medium | **Q# in sheet: 456**

---
## 🧠 Intuition
Count number of distinct substrings in string s.
s="abab" → substrings: a,b,ab,ba,ab,bab,aba,abab → distinct: {a,b,ab,ba,aba,bab,abab} = 7

**Approach:** Insert ALL suffixes into Trie. Count total nodes created = distinct substrings.
(Each path from root = unique prefix of some suffix = unique substring)

```java
public int countDistinctSubstrings(String s) {
    int n = s.length();
    // TrieNode array-based (memory efficient)
    int[][] trie = new int[n*n][26];  // Max nodes = O(N²)
    int nodeCount = 1;                 // Node 0 = root
    int distinctCount = 0;

    // Insert each suffix
    for (int i = 0; i < n; i++) {
        int curr = 0;
        for (int j = i; j < n; j++) {
            int c = s.charAt(j) - 'a';
            if (trie[curr][c] == 0) {
                trie[curr][c] = nodeCount++;  // Create new node
                distinctCount++;               // New node = new distinct substring
            }
            curr = trie[curr][c];
        }
    }
    return distinctCount;
}
```
**Time:** O(N²) | **Space:** O(N²)

---
## 🎯 Summary
- Insert all suffixes into Trie
- Each new Trie node created = one new distinct substring
- Total nodes created = answer
- O(N²) time and space (N suffixes, each up to N chars)
