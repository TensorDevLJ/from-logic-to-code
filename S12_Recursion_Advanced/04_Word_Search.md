# 04. Word Search

**LeetCode:** https://leetcode.com/problems/word-search/
**Difficulty:** 🔴 Hard | **Pattern:** Backtracking on Grid (DFS)

---

## 🧠 Intuition

**In Simple Words:**
Given 2D board of characters, find if a word exists by traversing adjacent cells (up/down/left/right). Can't reuse same cell.

**Core Idea:**
For each cell that matches word[0], start DFS/backtracking.
At each step: match current char, mark as visited, explore 4 directions, unmark.

---

## 🚀 Solution

```java
public class WordSearch {
    private static int[] dr = {0, 0, 1, -1};  // direction rows
    private static int[] dc = {1, -1, 0, 0};  // direction cols

    public static boolean exist(char[][] board, String word) {
        int m = board.length, n = board[0].length;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == word.charAt(0)) {  // Starting point
                    if (dfs(board, word, i, j, 0)) return true;
                }
            }
        }
        return false;
    }

    private static boolean dfs(char[][] board, String word, int r, int c, int idx) {
        // All characters matched!
        if (idx == word.length()) return true;

        // Out of bounds or wrong character or already visited
        if (r < 0 || r >= board.length || c < 0 || c >= board[0].length
                || board[r][c] != word.charAt(idx)) return false;

        char temp = board[r][c];
        board[r][c] = '#';  // Mark as visited (in-place!)

        for (int d = 0; d < 4; d++) {
            if (dfs(board, word, r + dr[d], c + dc[d], idx + 1)) {
                board[r][c] = temp;  // Restore before returning
                return true;
            }
        }

        board[r][c] = temp;  // Unmark (backtrack)
        return false;
    }

    public static void main(String[] args) {
        char[][] board = {
            {'A','B','C','E'},
            {'S','F','C','S'},
            {'A','D','E','E'}
        };
        System.out.println(exist(board, "ABCCED")); // true
        System.out.println(exist(board, "SEE"));    // true
        System.out.println(exist(board, "ABCB"));   // false
    }
}
```

**Time:** O(M × N × 4^L) where L=word length | **Space:** O(L) recursion

---

## ⚠️ Common Mistakes

1. ❌ Not marking cell as visited (reuses same cell)
2. ❌ Not restoring cell after backtrack (corrupts board)
3. ❌ Wrong base case order (check bounds BEFORE accessing board)

---

## 🎯 Final Summary

- Start DFS from every cell matching first character
- Mark visited with '#' (in-place trick avoids extra visited array)
- Unmark (restore) on backtrack
- 4 directions: up, down, left, right
- O(M×N×4^L) time — exponential but pruning makes it fast in practice
