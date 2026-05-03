# 02. Height, Balanced Check, Diameter of Binary Tree

**LeetCode Height:** https://leetcode.com/problems/maximum-depth-of-binary-tree/
**LeetCode Balanced:** https://leetcode.com/problems/balanced-binary-tree/
**LeetCode Diameter:** https://leetcode.com/problems/diameter-of-binary-tree/
**Difficulty:** 🟡 Medium | **Pattern:** Tree DFS / Post-order

---

## 🧠 Intuition

**Height:** Max depth from root to any leaf.
**Balanced:** Every node's left and right subtree heights differ by at most 1.
**Diameter:** Longest path between ANY two nodes (may not pass through root).

**Core Insight for all three:**
Use POST-ORDER traversal — compute result from BOTTOM UP.
Children's results are known before computing parent's result.

---

## 🚀 Solutions

```java
public class TreeMetrics {

    // 1. MAXIMUM DEPTH / HEIGHT
    public static int maxDepth(TreeNode root) {
        if (root == null) return 0;  // Null node has depth 0

        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);

        return 1 + Math.max(leftDepth, rightDepth);  // +1 for current node
    }

    // 2. BALANCED BINARY TREE CHECK
    // Returns -1 if unbalanced, else returns height
    private static int checkHeight(TreeNode node) {
        if (node == null) return 0;

        int leftH = checkHeight(node.left);
        if (leftH == -1) return -1;  // Left subtree unbalanced

        int rightH = checkHeight(node.right);
        if (rightH == -1) return -1;  // Right subtree unbalanced

        // Check balance at current node
        if (Math.abs(leftH - rightH) > 1) return -1;  // Unbalanced!

        return 1 + Math.max(leftH, rightH);  // Return height if balanced
    }

    public static boolean isBalanced(TreeNode root) {
        return checkHeight(root) != -1;
    }

    // 3. DIAMETER OF BINARY TREE
    // Diameter at any node = leftHeight + rightHeight
    private static int maxDiameter = 0;

    public static int diameterOfBinaryTree(TreeNode root) {
        maxDiameter = 0;
        heightForDiameter(root);
        return maxDiameter;
    }

    private static int heightForDiameter(TreeNode node) {
        if (node == null) return 0;

        int leftH = heightForDiameter(node.left);
        int rightH = heightForDiameter(node.right);

        // Diameter through this node = leftH + rightH
        maxDiameter = Math.max(maxDiameter, leftH + rightH);

        return 1 + Math.max(leftH, rightH);  // Return height
    }

    // Better: avoid global variable
    public static int diameterClean(TreeNode root) {
        int[] maxD = {0};
        heightHelper(root, maxD);
        return maxD[0];
    }

    private static int heightHelper(TreeNode node, int[] maxD) {
        if (node == null) return 0;
        int l = heightHelper(node.left, maxD);
        int r = heightHelper(node.right, maxD);
        maxD[0] = Math.max(maxD[0], l + r);
        return 1 + Math.max(l, r);
    }

    public static void main(String[] args) {
        //         1
        //        / \
        //       2   3
        //      / \
        //     4   5
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);

        System.out.println("Height: " + maxDepth(root));           // 3
        System.out.println("Balanced: " + isBalanced(root));       // true
        System.out.println("Diameter: " + diameterClean(root));    // 3 (4-2-1-3 or 5-2-1-3)
    }
}
```

**Time:** O(N) for all | **Space:** O(H)

**Dry Run — Diameter on tree above:**
```
heightForDiameter(4): left=0, right=0, diameter=0. return 1
heightForDiameter(5): left=0, right=0, diameter=0. return 1
heightForDiameter(2): left=1, right=1, diameter=max(0,1+1)=2. return 2
heightForDiameter(3): left=0, right=0. return 1
heightForDiameter(1): left=2, right=1, diameter=max(2,2+1)=3. return 3
maxDiameter = 3 ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Post-order DFS — compute children first, use results at parent

**Key insight:** ANY problem where you need info from BOTH subtrees before deciding → Post-order!

---

## ⚠️ Common Mistakes

1. ❌ Balanced: O(N²) brute force (compute height at each node separately). Use O(N) single-pass!
2. ❌ Diameter: thinking it always passes through root — it doesn't!
3. ❌ Using global variables → prefer `int[]` or return pair

---

## 🎯 Final Summary

- Height: 1 + max(leftH, rightH), base: null→0
- Balanced: check height, return -1 if |leftH-rightH|>1
- Diameter: at each node, diameter = leftH + rightH (edges)
- All use post-order: solve children before parent
- Diameter doesn't have to go through root!
