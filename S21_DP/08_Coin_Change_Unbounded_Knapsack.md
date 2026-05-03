# 416–420: Coin Change & Unbounded Knapsack

---
## 🧠 Q416: Minimum Coins (LeetCode 322)
https://leetcode.com/problems/coin-change/

**Problem:** Given coin denominations (unlimited supply), find minimum coins to make amount.

**Intuition:** UNBOUNDED Knapsack — each coin can be used multiple times.
dp[j] = min coins to make amount j.

```java
public int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);  // "Infinity" (impossible value)
    dp[0] = 0;                    // 0 coins to make amount 0

    for (int coin : coins) {
        for (int j = coin; j <= amount; j++) {  // LEFT to RIGHT (unbounded!)
            dp[j] = Math.min(dp[j], dp[j - coin] + 1);
        }
    }

    return dp[amount] > amount ? -1 : dp[amount];
}
```

**Dry Run:** coins=[1,2,5], amount=11
```
dp[0]=0, dp[1]=1, dp[2]=1, dp[3]=2, dp[4]=2, dp[5]=1
dp[6]=2, dp[7]=2, dp[8]=3, dp[9]=3, dp[10]=2, dp[11]=3
Answer: 3 (5+5+1) ✅
```
**Time:** O(N×Amount) | **Space:** O(Amount)

---
## 🧠 Q418: Coin Change II — Count Ways (LeetCode 518)
https://leetcode.com/problems/coin-change-ii/

**Problem:** Count number of combinations that make up amount.

```java
public int change(int amount, int[] coins) {
    int[] dp = new int[amount + 1];
    dp[0] = 1;  // 1 way to make 0

    for (int coin : coins)
        for (int j = coin; j <= amount; j++)  // Unbounded: left to right
            dp[j] += dp[j - coin];

    return dp[amount];
}
```
**Key difference from 0/1:** Left-to-right traversal allows reusing same coin.

---
## 🧠 Q419: Unbounded Knapsack
**Problem:** Items with unlimited quantities, maximize value within weight W.

```java
public int unboundedKnapsack(int[] wt, int[] val, int W) {
    int[] dp = new int[W + 1];

    for (int i = 0; i < wt.length; i++)
        for (int j = wt[i]; j <= W; j++)  // LEFT to RIGHT = reuse allowed!
            dp[j] = Math.max(dp[j], dp[j - wt[i]] + val[i]);

    return dp[W];
}
```

---
## 🔑 KEY: Left vs Right Traversal
```
RIGHT-TO-LEFT (j from W to 0): 0/1 Knapsack → each item used AT MOST ONCE
LEFT-TO-RIGHT  (j from 0 to W): Unbounded   → each item used ANY TIMES
```

---
## 🎯 Summary
- Coin Change (min coins): fill with ∞, dp[j]=min(dp[j], dp[j-coin]+1), L→R
- Coin Change II (count ways): dp[j]+=dp[j-coin], L→R
- 0/1 Knapsack: dp[j]=max(dp[j], dp[j-w]+v), R→L
- Unbounded Knapsack: same but L→R
