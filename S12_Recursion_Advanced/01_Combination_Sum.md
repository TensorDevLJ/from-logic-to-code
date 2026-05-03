# 01. Combination Sum

**LeetCode:** https://leetcode.com/problems/combination-sum/
**Difficulty:** 🟡 Medium | **Pattern:** Backtracking / Recursion

---

## 🧠 Intuition

**In Simple Words:**
Given array of distinct integers, find all combinations that sum to target.
Same number can be used unlimited times.
candidates=[2,3,6,7], target=7 → [[2,2,3],[7]]

**Real-Life Analogy:**
Making change for a dollar using unlimited coins. Find all ways to combine coins to reach exactly $1.

**Core Idea — Backtracking:**
Try each candidate. Add it to current combination, recurse with same candidate (can reuse).
If sum exceeds target → backtrack. If sum equals target → add to result.

**Backtracking Template:**
```
choose → recurse → unchoose (backtrack)
```

---

## 🚀 Solution

```java
import java.util.*;

public class CombinationSum {
    public static List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(candidates);  // Sort for pruning optimization
        backtrack(candidates, target, 0, new ArrayList<>(), result);
        return result;
    }

    private static void backtrack(
            int[] candidates, int remaining, int start,
            List<Integer> current, List<List<Integer>> result) {

        // Base case 1: Found valid combination
        if (remaining == 0) {
            result.add(new ArrayList<>(current));  // Deep copy!
            return;
        }

        // Try each candidate starting from 'start' (avoid duplicates)
        for (int i = start; i < candidates.length; i++) {
            // Pruning: if candidate > remaining, skip (sorted, so all after also too big)
            if (candidates[i] > remaining) break;

            current.add(candidates[i]);              // Choose
            backtrack(candidates, remaining - candidates[i], i, current, result); // i not i+1 (reuse allowed)
            current.remove(current.size() - 1);     // Unchoose (backtrack)
        }
    }

    public static void main(String[] args) {
        System.out.println(combinationSum(new int[]{2,3,6,7}, 7));
        // [[2,2,3],[7]]
        System.out.println(combinationSum(new int[]{2,3,5}, 8));
        // [[2,2,2,2],[2,3,3],[3,5]]
    }
}
```

**Time:** O(N^(T/M)) where T=target, M=min candidate  
**Space:** O(T/M) recursion depth

**Dry Run:** candidates=[2,3,6,7], target=7
```
backtrack(remaining=7, start=0, current=[])
  i=0 (val=2): current=[2], backtrack(5, 0, [2])
    i=0 (val=2): current=[2,2], backtrack(3, 0, [2,2])
      i=0 (val=2): current=[2,2,2], backtrack(1, 0, [2,2,2])
        i=0 (val=2): 2>1 → break (pruning!)
      remove 2 → current=[2,2]
      i=1 (val=3): current=[2,2,3], backtrack(0, 1, [2,2,3])
        remaining=0 → ADD [2,2,3] to result! ✅
      remove 3 → current=[2,2]
      i=2 (val=6): 6>3 → break
    remove 2 → current=[2]
    ...eventually...
  i=3 (val=7): current=[7], backtrack(0, 3, [7])
    remaining=0 → ADD [7] to result! ✅

Result: [[2,2,3],[7]] ✅
```

---

## 🔁 Pattern Recognition

**Pattern:** Backtracking — Try → Explore → Undo
**Variations:**
- Combination Sum II (each used once) → i+1 in recursive call, skip duplicates
- Combination Sum III (exactly K numbers) → add constraint
- Subset Sum → similar structure

---

## 🧠 How to Think (Interview View)

**Backtracking Template — Memorize this:**
```
void backtrack(state, choices) {
    if (base_case) { record_solution; return; }
    for (choice in choices) {
        make_choice(choice);
        backtrack(new_state, new_choices);
        undo_choice(choice);   // This is the BACKTRACK step!
    }
}
```

**Key decisions:**
1. When to stop? (remaining == 0)
2. Start index? (i not 0, to avoid reordering duplicates)
3. Allow reuse? (pass i for reuse, i+1 for no reuse)

---

## ⚠️ Common Mistakes

1. ❌ `result.add(current)` — adds REFERENCE not copy! Use `new ArrayList<>(current)`
2. ❌ Not removing after recursion (forgetting the "unchoose" step)
3. ❌ Passing i+1 when reuse is allowed (use i for same element reuse)
4. ❌ Not sorting before pruning (pruning only works on sorted array)

---

## 🧩 Variations

1. **Combination Sum II** — each element used once, no duplicate combos → LeetCode 40
2. **Combination Sum III** — exactly k numbers summing to n → LeetCode 216
3. **Subsets I & II** — without target constraint → LeetCode 78, 90
4. **Permutations** — order matters → LeetCode 46

---

## 🧠 Memory Trick

**"CHOOSE → RECURSE → UNCHOOSE"**
Always undo your choice after recursion. That's backtracking!

---

## 🎯 Final Summary

- Backtracking: Try each candidate, recurse, undo
- Pass index `i` (not i+1) to allow reuse of same element
- `remaining == 0` → valid combination found
- Pruning: break when candidate > remaining (after sorting)
- Deep copy result: `new ArrayList<>(current)` not `current`
