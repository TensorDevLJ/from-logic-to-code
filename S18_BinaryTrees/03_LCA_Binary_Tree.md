# 03. Lowest Common Ancestor (LCA) of Binary Tree

**LeetCode:** https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/
**Difficulty:** 🔴 Hard | **Pattern:** Tree DFS / Post-order

---

## 🧠 Intuition

**In Simple Words:**
Given two nodes p and q, find their LCA — deepest node that has both p and q as descendants.
(A node is considered a descendant of itself)

**Real-Life Analogy:**
Family tree. LCA of two cousins = their common grandparent.

**Core Insight:**
During post-order traversal:
- If node is null: return null
- If node == p or node == q: return node (found it!)
- If left returns non-null AND right returns non-null: current node IS the LCA!
- If only one side returns non-null: that's the answer (both are in that subtree)

---

## 🚀 Solution

```java
public class LowestCommonAncestor {
    public static TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Base cases
        if (root == null) return null;        // Not found
        if (root == p || root == q) return root; // Found p or q

        // Search in left and right subtrees
        TreeNode leftResult = lowestCommonAncestor(root.left, p, q);
        TreeNode rightResult = lowestCommonAncestor(root.right, p, q);

        // If BOTH sides found something → current root is LCA!
        if (leftResult != null && rightResult != null) return root;

        // If only one side found → return that side
        return leftResult != null ? leftResult : rightResult;
    }

    public static void main(String[] args) {
        //          3
        //         / \
        //        5   1
        //       / \ / \
        //      6  2 0  8
        //        / \
        //       7   4
        TreeNode root = new TreeNode(3);
        root.left = new TreeNode(5);
        root.right = new TreeNode(1);
        root.left.left = new TreeNode(6);
        root.left.right = new TreeNode(2);
        root.right.left = new TreeNode(0);
        root.right.right = new TreeNode(8);
        root.left.right.left = new TreeNode(7);
        root.left.right.right = new TreeNode(4);

        // LCA of 5 and 1 = 3
        System.out.println(lowestCommonAncestor(root, root.left, root.right).val); // 3

        // LCA of 5 and 4 = 5
        System.out.println(lowestCommonAncestor(root, root.left, root.left.right.right).val); // 5
    }
}
```

**Time:** O(N) | **Space:** O(H)

**Dry Run:** Find LCA(5, 4)
```
LCA(3, p=5, q=4):
  left = LCA(5, p=5, q=4):
    root==p → return node(5) immediately!
  left = node(5)
  right = LCA(1, p=5, q=4):
    LCA(0): null. LCA(8): null. → return null
  right = null

Only leftResult non-null → return leftResult = node(5)

LCA = 5 ✅ (5 is ancestor of 4, and 5 itself)
```

---

## 🔁 Pattern Recognition

**Pattern:** Post-order DFS — process children first, use results at parent

---

## ⚠️ Common Mistakes

1. ❌ Thinking LCA always splits the two nodes (wrong for ancestor case like p=5, q=4)
2. ❌ Not returning node when it equals p or q
3. ❌ Using BFS (harder) instead of DFS recursion

---

## 🎯 Final Summary

- If root is null: return null
- If root == p or q: return root
- Recurse left and right
- Both non-null: root is LCA
- One non-null: return that one (both nodes are in that subtree)
- O(N) single pass — elegant and interview-perfect
