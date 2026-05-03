# 02. N-Queens Problem

**LeetCode:** https://leetcode.com/problems/n-queens/
**Difficulty:** 🔴 Hard | **Pattern:** Backtracking

---

## 🧠 Intuition

**In Simple Words:**
Place N queens on N×N chess board such that no two queens attack each other.
Queens attack in same row, column, or diagonal.

**Real-Life Analogy:**
Placing N aggressive CEOs in N offices arranged in a grid, such that none can see (attack) any other directly left/right, up/down, or diagonally!

**Core Idea:**
Place one queen per row (since queens in same row always conflict).
For each row, try each column. Check if placement is SAFE. If safe, place and recurse to next row. If no valid column → backtrack.

**Safety Check:**
For position (row, col):
- No queen in same COLUMN (check col array)
- No queen in upper-left diagonal
- No queen in upper-right diagonal

---

## 🚀 Solution (Optimized with boolean arrays)

```java
import java.util.*;

public class NQueens {
    public static List<List<String>> solveNQueens(int n) {
        List<List<String>> result = new ArrayList<>();
        char[][] board = new char[n][n];

        // Initialize board with dots
        for (char[] row : board) Arrays.fill(row, '.');

        // Track which columns and diagonals are under attack
        boolean[] cols = new boolean[n];          // column under attack
        boolean[] diag1 = new boolean[2 * n - 1]; // top-left to bottom-right diagonal
        boolean[] diag2 = new boolean[2 * n - 1]; // top-right to bottom-left diagonal

        backtrack(board, 0, n, cols, diag1, diag2, result);
        return result;
    }

    private static void backtrack(
            char[][] board, int row, int n,
            boolean[] cols, boolean[] diag1, boolean[] diag2,
            List<List<String>> result) {

        // All rows filled → valid solution!
        if (row == n) {
            result.add(boardToList(board));
            return;
        }

        for (int col = 0; col < n; col++) {
            // Check if safe: column and both diagonals free
            int d1 = row - col + n - 1;  // diag1 index
            int d2 = row + col;           // diag2 index

            if (cols[col] || diag1[d1] || diag2[d2]) continue; // Unsafe!

            // Place queen
            board[row][col] = 'Q';
            cols[col] = diag1[d1] = diag2[d2] = true;

            // Recurse to next row
            backtrack(board, row + 1, n, cols, diag1, diag2, result);

            // Remove queen (backtrack)
            board[row][col] = '.';
            cols[col] = diag1[d1] = diag2[d2] = false;
        }
    }

    private static List<String> boardToList(char[][] board) {
        List<String> list = new ArrayList<>();
        for (char[] row : board) list.add(new String(row));
        return list;
    }

    public static void main(String[] args) {
        List<List<String>> solutions = solveNQueens(4);
        for (List<String> solution : solutions) {
            for (String row : solution) System.out.println(row);
            System.out.println();
        }
        // Two solutions for N=4
    }
}
```

**Time:** O(N!) | **Space:** O(N²)

**Dry Run:** N=4, Row 0
```
Row 0: Try col 0 → place Q at (0,0). Mark col0, diag1[3], diag2[0]
  Row 1: col0? NO (col0 blocked). col1? diag2[2]? NO (row+col=1+1=2=d2[0] blocked? 
  
Let me use the 4-queens solution directly:
Solution 1:  Solution 2:
.Q..         ..Q.
...Q         Q...
Q...         ...Q
..Q.         .Q..
```

**Diagonal indices explained:**
- diag1 (top-left to bottom-right): row - col + (n-1) is same for queens on same diagonal
- diag2 (top-right to bottom-left): row + col is same for queens on same diagonal

---

## 🔁 Pattern Recognition

**Pattern:** Backtracking on Grid
**Same pattern:** Sudoku Solver, Rat in Maze, Word Search, M-Coloring

---

## 🧠 How to Think (Interview View)

1. "N-Queens" → one queen per row (key insight!)
2. Track attacks using arrays (O(1) check) not by scanning board
3. Backtrack when no valid column found in current row

**Why one per row?** N queens on N×N board → pigeonhole principle: exactly 1 per row!

---

## ⚠️ Common Mistakes

1. ❌ O(N²) safety check (scanning row/col/diagonal) — use boolean arrays!
2. ❌ Forgetting to reset boolean arrays when backtracking
3. ❌ Wrong diagonal index formulas

---

## 🎯 Final Summary

- One queen per row — key insight that drives algorithm
- Track column and diagonal attacks with boolean arrays (O(1) check)
- Backtrack when column is unsafe or exhausted
- N=1: trivial. N=2,3: no solution. N=4+: solutions exist
- Time O(N!), but with pruning much faster in practice
