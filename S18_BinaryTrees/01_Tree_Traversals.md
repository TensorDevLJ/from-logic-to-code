# 01. Binary Tree Traversals (Pre/In/Post/Level Order)

**LeetCode Inorder:** https://leetcode.com/problems/binary-tree-inorder-traversal/
**LeetCode Level Order:** https://leetcode.com/problems/binary-tree-level-order-traversal/
**Difficulty:** 🟢 Easy | **Pattern:** Tree DFS / BFS

---

## 🧠 Intuition

**In Simple Words:**
Traverse all nodes of a tree in different orders:
- **Preorder:** Root → Left → Right (Process before children)
- **Inorder:** Left → Root → Right (BST gives SORTED order!)
- **Postorder:** Left → Right → Root (Process after children)
- **Level Order:** Level by level (BFS)

**Real-Life Analogy:**
Exploring a building:
- Preorder: Enter room, note it, then explore sub-rooms
- Inorder: Go to leftmost room first, note it, come back up
- Postorder: Explore all sub-rooms, then note current room
- Level Order: Note all rooms on floor 1, then floor 2, etc.

---

## 📌 Node Definition

```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}
```

---

## 🚀 All Four Traversals

```java
import java.util.*;

public class TreeTraversals {

    // 1. PREORDER: Root → Left → Right
    public static List<Integer> preorder(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorderHelper(root, result);
        return result;
    }

    private static void preorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) return;         // Base case
        result.add(node.val);             // Process ROOT
        preorderHelper(node.left, result); // Go LEFT
        preorderHelper(node.right, result);// Go RIGHT
    }

    // 2. INORDER: Left → Root → Right
    public static List<Integer> inorder(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorderHelper(root, result);
        return result;
    }

    private static void inorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) return;
        inorderHelper(node.left, result);  // Go LEFT first
        result.add(node.val);              // Process ROOT
        inorderHelper(node.right, result); // Go RIGHT
    }

    // 3. POSTORDER: Left → Right → Root
    public static List<Integer> postorder(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        postorderHelper(root, result);
        return result;
    }

    private static void postorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) return;
        postorderHelper(node.left, result);  // Go LEFT
        postorderHelper(node.right, result); // Go RIGHT
        result.add(node.val);                // Process ROOT last
    }

    // 4. LEVEL ORDER (BFS): Level by level using Queue
    public static List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);  // Start with root

        while (!queue.isEmpty()) {
            int levelSize = queue.size();  // Nodes at current level
            List<Integer> level = new ArrayList<>();

            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);

                // Add children for next level
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }

            result.add(level);
        }

        return result;
    }

    // 5. ITERATIVE INORDER (Important for interviews!)
    public static List<Integer> inorderIterative(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode curr = root;

        while (curr != null || !stack.isEmpty()) {
            // Go as far LEFT as possible
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }

            // Process leftmost unprocessed node
            curr = stack.pop();
            result.add(curr.val);

            // Move to right subtree
            curr = curr.right;
        }

        return result;
    }

    public static void main(String[] args) {
        // Build tree:       1
        //                  / \
        //                 2   3
        //                / \
        //               4   5
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);

        System.out.println("Preorder:   " + preorder(root));     // [1,2,4,5,3]
        System.out.println("Inorder:    " + inorder(root));      // [4,2,5,1,3]
        System.out.println("Postorder:  " + postorder(root));    // [4,5,2,3,1]
        System.out.println("LevelOrder: " + levelOrder(root));   // [[1],[2,3],[4,5]]
    }
}
```

**Time:** O(N) for all traversals  
**Space:** O(H) for DFS (H = height), O(W) for BFS (W = max width)

**Dry Run on tree:**
```
        1
       / \
      2   3
     / \
    4   5

Preorder:  Visit 1, go left → Visit 2, go left → Visit 4, no children → backtrack → Visit 5 → backtrack → go right → Visit 3
Result: [1, 2, 4, 5, 3]

Inorder: Go left all way → 4 (leftmost) → back to 2 → 5 → back to 1 → 3
Result: [4, 2, 5, 1, 3]

Postorder: 4 → 5 → 2 → 3 → 1 (children before root)
Result: [4, 5, 2, 3, 1]

Level Order: Level 0: [1], Level 1: [2,3], Level 2: [4,5]
Result: [[1],[2,3],[4,5]]
```

---

## 🔁 Pattern Recognition

- **DFS** (depth-first): Pre/In/Post order — uses recursion or explicit stack
- **BFS** (breadth-first): Level order — uses Queue
- Inorder of BST = SORTED array!

---

## 🧠 Key Insights for Interviews

| Traversal | Use Case |
|-----------|----------|
| Preorder | Clone tree, serialize tree |
| Inorder | BST validation, sorted order |
| Postorder | Delete tree, compute subtree results |
| Level Order | Find level, connect nodes at same level |

---

## ⚠️ Common Mistakes

1. ❌ Wrong null check in recursive traversal
2. ❌ Level order: not tracking level size before loop!
3. ❌ Iterative inorder: wrong loop condition (`curr != null || !stack.isEmpty()`)

---

## 🎯 Final Summary

- Preorder: Root-Left-Right. Add root.val BEFORE recursive calls
- Inorder: Left-Root-Right. Add root.val BETWEEN recursive calls
- Postorder: Left-Right-Root. Add root.val AFTER recursive calls
- Level Order: BFS with Queue, process levelSize nodes per iteration
- Iterative inorder: go left until null, process, go right
