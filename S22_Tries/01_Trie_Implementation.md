# 453–454: Trie Implementation & Operations

**LeetCode:** https://leetcode.com/problems/implement-trie-prefix-tree/
**Difficulty:** 🔴 Hard | **Q# in sheet: 453–454**

---
## 🧠 Intuition
**Trie (Prefix Tree):** Tree where each node = one character. Root = empty.
Path from root to node = prefix. Path to word-end node = complete word.

**Why Trie?**
- Prefix search in O(L) instead of O(N×L)
- Great for: autocomplete, spell check, word games

**Structure:**
```
Insert "cat", "car", "bat":
         root
        /        c   b
       |   |
       a   a
      / \        t   r   t
    (*)  (*) (*)    (* = end of word)
```

---
## 🚀 Full Trie Implementation
```java
class Trie {
    private TrieNode root;

    static class TrieNode {
        TrieNode[] children = new TrieNode[26];
        boolean isEnd = false;
    }

    public Trie() { root = new TrieNode(); }

    // INSERT — O(L)
    public void insert(String word) {
        TrieNode curr = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null)
                curr.children[idx] = new TrieNode();  // Create node
            curr = curr.children[idx];
        }
        curr.isEnd = true;  // Mark end of word
    }

    // SEARCH (exact word) — O(L)
    public boolean search(String word) {
        TrieNode curr = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null) return false;
            curr = curr.children[idx];
        }
        return curr.isEnd;  // Must be end of word
    }

    // STARTS WITH (prefix check) — O(L)
    public boolean startsWith(String prefix) {
        TrieNode curr = root;
        for (char c : prefix.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null) return false;
            curr = curr.children[idx];
        }
        return true;  // Prefix exists (don't care if it's a full word)
    }

    // COUNT WORDS WITH PREFIX
    public int countWordsWithPrefix(String prefix) {
        TrieNode curr = root;
        for (char c : prefix.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null) return 0;
            curr = curr.children[idx];
        }
        return countWords(curr);  // Count all words from this node
    }

    private int countWords(TrieNode node) {
        if (node == null) return 0;
        int count = node.isEnd ? 1 : 0;
        for (TrieNode child : node.children)
            count += countWords(child);
        return count;
    }

    public static void main(String[] args) {
        Trie trie = new Trie();
        trie.insert("apple");
        System.out.println(trie.search("apple"));    // true
        System.out.println(trie.search("app"));      // false (not inserted)
        System.out.println(trie.startsWith("app"));  // true
        trie.insert("app");
        System.out.println(trie.search("app"));      // true
    }
}
```

---
## 📌 Dry Run: Insert "cat", then Search "cat" and "ca"
```
Insert "cat":
root → c(new) → a(new) → t(new, isEnd=true)

Search "cat":
root → c(exists) → a(exists) → t(exists, isEnd=true) → return true ✅

Search "ca":
root → c → a(exists, isEnd=false) → return false ✅ (not a complete word)

startsWith("ca"):
root → c → a(exists) → return true ✅ (prefix exists)
```

---
## 🔁 When to Use Trie
- Autocomplete / Type-ahead search
- Spell checker
- Prefix matching in O(L)
- Word search in a dictionary
- IP routing (longest prefix match)

---
## ⚠️ Common Mistakes
1. ❌ Returning true in search() without checking `isEnd`
2. ❌ Using HashMap instead of array (slower for fixed alphabet)
3. ❌ Not initializing children array

---
## 🎯 Summary
- TrieNode: children[26] + isEnd flag
- Insert: traverse/create nodes for each char, mark isEnd at last
- Search: traverse; return isEnd at last char
- StartsWith: traverse; return true if path exists (ignore isEnd)
- O(L) for all operations where L = word/prefix length
