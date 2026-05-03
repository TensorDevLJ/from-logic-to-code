# 04. Top View & Bottom View of Binary Tree

**GFG Top View:** https://practice.geeksforgeeks.org/problems/top-view-of-binary-tree/1
**GFG Bottom View:** https://practice.geeksforgeeks.org/problems/bottom-view-of-binary-tree/1
**Difficulty:** 🟡 Medium | **Pattern:** Level Order BFS + HashMap

---

## 🧠 Intuition

**Top View:** For each horizontal distance (column), the FIRST node seen from top.
**Bottom View:** For each horizontal distance, the LAST node seen from top (or first from bottom).

**Core Idea:**
Assign each node a horizontal distance (HD):
- Root: HD = 0
- Left child: parent's HD - 1
- Right child: parent's HD + 1

For Top View: first node at each HD (BFS level order)
For Bottom View: last node at each HD (overwrite with BFS)

---

## 🚀 Solution

```java
import java.util.*;

public class TopBottomView {
    // Pair class to store (node, horizontal distance)
    static class Pair {
        TreeNode node;
        int hd;
        Pair(TreeNode n, int h) { node = n; hd = h; }
    }

    // TOP VIEW
    public static List<Integer> topView(TreeNode root) {
        if (root == null) return new ArrayList<>();

        Map<Integer, Integer> hdToVal = new TreeMap<>();  // HD → first node value
        Queue<Pair> queue = new LinkedList<>();
        queue.offer(new Pair(root, 0));

        while (!queue.isEmpty()) {
            Pair curr = queue.poll();
            int hd = curr.hd;

            // Only add FIRST node at each HD (top view)
            if (!hdToVal.containsKey(hd)) {
                hdToVal.put(hd, curr.node.val);
            }

            if (curr.node.left != null) queue.offer(new Pair(curr.node.left, hd - 1));
            if (curr.node.right != null) queue.offer(new Pair(curr.node.right, hd + 1));
        }

        return new ArrayList<>(hdToVal.values());  // TreeMap keeps sorted by HD
    }

    // BOTTOM VIEW
    public static List<Integer> bottomView(TreeNode root) {
        if (root == null) return new ArrayList<>();

        Map<Integer, Integer> hdToVal = new TreeMap<>();  // HD → last node value
        Queue<Pair> queue = new LinkedList<>();
        queue.offer(new Pair(root, 0));

        while (!queue.isEmpty()) {
            Pair curr = queue.poll();
            int hd = curr.hd;

            // ALWAYS overwrite (last one at each HD = bottom view)
            hdToVal.put(hd, curr.node.val);

            if (curr.node.left != null) queue.offer(new Pair(curr.node.left, hd - 1));
            if (curr.node.right != null) queue.offer(new Pair(curr.node.right, hd + 1));
        }

        return new ArrayList<>(hdToVal.values());
    }

    public static void main(String[] args) {
        //         1
        //        / \
        //       2   3
        //      / \ / \
        //     4  5 6  7
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2); root.right = new TreeNode(3);
        root.left.left = new TreeNode(4); root.left.right = new TreeNode(5);
        root.right.left = new TreeNode(6); root.right.right = new TreeNode(7);

        System.out.println("Top View: " + topView(root));    // [4, 2, 1, 3, 7]
        System.out.println("Bottom View: " + bottomView(root)); // [4, 2, 5or6, 3, 7]
    }
}
```

**HD assignments for example:**
```
          1 (HD=0)
         / \
      2(-1) 3(+1)
      /  \  / \
  4(-2) 5(0) 6(0) 7(+2)

Top view: HD=-2:4, HD=-1:2, HD=0:1 (first seen), HD=1:3, HD=2:7 → [4,2,1,3,7]
Bottom view: HD=-2:4, HD=-1:2, HD=0: 5 or 6 (last seen), HD=1:3, HD=2:7
```

---

## 🔁 Pattern Recognition

**Pattern:** BFS + HashMap indexed by horizontal distance
**Same pattern:** Vertical order traversal (all nodes at each HD), Right/Left view (first/last at each level)

---

## 🎯 Final Summary

- Assign HD: root=0, left child=HD-1, right child=HD+1
- BFS (level order) + TreeMap (sorted by HD)
- Top view: putIfAbsent (first node wins)
- Bottom view: always put (last node wins)
- TreeMap ensures left-to-right ordering of HD values
