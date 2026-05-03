# 458–459: Maximum XOR of Two Numbers in Array

**LeetCode:** https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/
**Difficulty:** 🔴 Hard | **Q# in sheet: 458–459**

---
## 🧠 Intuition
Find max XOR of any two numbers in array.
nums=[3,10,5,25,2,8] → 25 XOR 5 = 28 → max!

**Core Insight:** XOR is maximum when bits differ as much as possible.
Use a Binary Trie: insert binary representation of each number.
For each number n, greedily find the number in Trie that maximizes XOR.

**Strategy:** For bit b (from MSB to LSB): if opposite bit exists in Trie → take it (XOR = 1). Else take same bit.

---
## 🚀 Solution
```java
class MaxXOR {
    static class TrieNode {
        TrieNode[] children = new TrieNode[2];  // 0 or 1
    }

    private TrieNode root = new TrieNode();

    // Insert number as 32-bit binary
    public void insert(int num) {
        TrieNode curr = root;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (curr.children[bit] == null)
                curr.children[bit] = new TrieNode();
            curr = curr.children[bit];
        }
    }

    // Find max XOR of num with any number in Trie
    public int maxXOR(int num) {
        TrieNode curr = root;
        int xor = 0;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            int want = 1 - bit;  // We want the opposite bit for max XOR
            if (curr.children[want] != null) {
                xor |= (1 << i);        // This bit contributes to XOR
                curr = curr.children[want];
            } else {
                curr = curr.children[bit];  // Must take same bit
            }
        }
        return xor;
    }

    public static int findMaximumXOR(int[] nums) {
        MaxXOR trie = new MaxXOR();
        for (int num : nums) trie.insert(num);

        int maxXor = 0;
        for (int num : nums)
            maxXor = Math.max(maxXor, trie.maxXOR(num));
        return maxXor;
    }

    public static void main(String[] args) {
        System.out.println(findMaximumXOR(new int[]{3,10,5,25,2,8})); // 28
    }
}
```
**Time:** O(N×32) = O(N) | **Space:** O(N×32)

---
## 🔑 Key Concept
- Binary Trie stores bit patterns
- For each number: greedily pick opposite bit at each level → maximize XOR
- If opposite bit unavailable → take same bit (XOR = 0 for that bit)

---
## 🎯 Summary
- Build Binary Trie (32 levels for 32-bit integers)
- For each num: traverse Trie picking opposite bits → max XOR
- O(32N) time — linear in number of elements
- Pattern reused in: Max XOR with element from array (LeetCode 1707)
