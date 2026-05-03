# 06. DP on Stocks — All Variants

**LeetCode I:** https://leetcode.com/problems/best-time-to-buy-and-sell-stock/
**LeetCode II:** https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/
**LeetCode III:** https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/
**Difficulty:** 🟡 Medium | **Pattern:** DP / Greedy

---

## 🧠 Intuition

All stock problems are about finding the optimal buying and selling days.

---

## 🚀 All Stock Variants

```java
public class StockProblems {

    // VARIANT 1: At most 1 transaction
    // Simple: track min price seen, max profit
    public static int maxProfit1(int[] prices) {
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;

        for (int price : prices) {
            minPrice = Math.min(minPrice, price);
            maxProfit = Math.max(maxProfit, price - minPrice);
        }

        return maxProfit;
    }

    // VARIANT 2: Unlimited transactions (buy/sell any day, one at a time)
    // Greedy: collect every upward slope
    public static int maxProfit2(int[] prices) {
        int profit = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i-1]) {
                profit += prices[i] - prices[i-1];  // Take every gain!
            }
        }
        return profit;
    }

    // VARIANT 3: At most 2 transactions
    // DP with states: [day][#transactions used][holding stock?]
    public static int maxProfit3(int[] prices) {
        int buy1 = Integer.MIN_VALUE;   // Best profit after 1st buy
        int sell1 = 0;                  // Best profit after 1st sell
        int buy2 = Integer.MIN_VALUE;   // Best profit after 2nd buy
        int sell2 = 0;                  // Best profit after 2nd sell

        for (int price : prices) {
            buy1  = Math.max(buy1, -price);           // Buy on this day
            sell1 = Math.max(sell1, buy1 + price);    // Sell on this day
            buy2  = Math.max(buy2, sell1 - price);    // Buy again
            sell2 = Math.max(sell2, buy2 + price);    // Sell again
        }

        return sell2;
    }

    // VARIANT 4: At most K transactions
    public static int maxProfitK(int k, int[] prices) {
        int n = prices.length;
        if (k >= n/2) return maxProfit2(prices);  // Unlimited effectively

        int[] buy = new int[k];    // Best profit after ith buy
        int[] sell = new int[k];   // Best profit after ith sell
        Arrays.fill(buy, Integer.MIN_VALUE);

        for (int price : prices) {
            for (int i = 0; i < k; i++) {
                buy[i] = Math.max(buy[i], (i > 0 ? sell[i-1] : 0) - price);
                sell[i] = Math.max(sell[i], buy[i] + price);
            }
        }

        return sell[k-1];
    }

    // VARIANT 5: With cooldown (can't buy day after selling)
    public static int maxProfitCooldown(int[] prices) {
        int sell = 0, prev_sell = 0, buy = Integer.MIN_VALUE;

        for (int price : prices) {
            int prev_buy = buy;
            buy = Math.max(buy, prev_sell - price);    // Buy: use profit before prev sell
            prev_sell = sell;
            sell = Math.max(sell, prev_buy + price);   // Sell
        }

        return sell;
    }

    // VARIANT 6: With transaction fee
    public static int maxProfitFee(int[] prices, int fee) {
        int cash = 0, hold = Integer.MIN_VALUE;

        for (int price : prices) {
            cash = Math.max(cash, hold + price - fee);  // Sell (pay fee)
            hold = Math.max(hold, cash - price);         // Buy
        }

        return cash;
    }

    public static void main(String[] args) {
        int[] p = {3,3,5,0,0,3,1,4};
        System.out.println(maxProfit1(p));          // 4
        System.out.println(maxProfit2(p));          // 8 (0+3+3=6? actually 3+4=7? let me check: 3+3=6)
        System.out.println(maxProfit3(p));          // 6
        System.out.println(maxProfitK(2, p));       // 6
    }
}
```

---

## 🔁 Pattern Summary Table

| Variant | Approach | Key Insight |
|---------|----------|-------------|
| 1 tx | Greedy | Track min price, max profit |
| Unlimited | Greedy | Sum all positive differences |
| 2 tx | DP with states | buy1, sell1, buy2, sell2 |
| K tx | DP with arrays | buy[k], sell[k] arrays |
| Cooldown | State machine | Extra prev_sell state |
| With fee | State machine | Subtract fee on sell |

---

## 🎯 Final Summary

- Variant 1: min price → max profit with single buy-sell
- Variant 2: Collect every rising day → maxProfit
- Variant 3: 4 variables: buy1, sell1, buy2, sell2
- Variant K: Arrays of size K for buy/sell states
- Key pattern: state machine with buy/sell/hold states
