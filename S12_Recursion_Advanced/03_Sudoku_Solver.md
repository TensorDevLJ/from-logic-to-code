# 03. Sudoku Solver

**LeetCode:** https://leetcode.com/problems/sudoku-solver/
**Difficulty:** 🔴 Hard | **Pattern:** Backtracking

---

## 🧠 Intuition

**In Simple Words:**
Fill empty cells ('.') in a 9×9 Sudoku board. Each row, column, 3×3 box must contain 1-9 exactly once.

**Core Idea:**
For each empty cell, try placing digits 1-9. Check if valid. If valid, place and recurse. If no digit works → backtrack.

---

## 🚀 Solution

```java
public class SudokuSolver {
    public static void solveSudoku(char[][] board) {
        solve(board);
    }

    private static boolean solve(char[][] board) {
        for (int row = 0; row < 9; row++) {
            for (int col = 0; col < 9; col++) {
                if (board[row][col] == '.') {  // Empty cell
                    for (char c = '1'; c <= '9'; c++) {
                        if (isValid(board, row, col, c)) {
                            board[row][col] = c;  // Place digit

                            if (solve(board)) return true;  // Try to solve rest

                            board[row][col] = '.';  // Backtrack
                        }
                    }
                    return false;  // No valid digit found → backtrack!
                }
            }
        }
        return true;  // All cells filled → solved!
    }

    private static boolean isValid(char[][] board, int row, int col, char c) {
        for (int i = 0; i < 9; i++) {
            // Check row
            if (board[row][i] == c) return false;

            // Check column
            if (board[i][col] == c) return false;

            // Check 3×3 box
            int boxRow = 3 * (row / 3) + i / 3;
            int boxCol = 3 * (col / 3) + i % 3;
            if (board[boxRow][boxCol] == c) return false;
        }
        return true;
    }
}
```

**Time:** O(9^(N)) where N = empty cells | **Space:** O(N) recursion depth

**Key formula for 3×3 box:**
- Box starts at `(3*(row/3), 3*(col/3))`
- Traverse box using `i/3` and `i%3`

---

## 🔁 Pattern Recognition

Same backtracking template as N-Queens:
- Find empty cell → try options → recurse → backtrack if fail

---

## 🎯 Final Summary

- Find each '.' → try '1'-'9' → check validity → recurse
- Validity: check row, column, 3×3 box
- Return false when no digit works → triggers backtrack
- Return true when all cells filled → solved!
