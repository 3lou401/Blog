> Say you have an array for which the *i*th
 element is the price of a given stock on day *i*.
Design an algorithm to find the maximum profit. You may complete at most *two* transactions.
**Note:**You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
***
假设你有一个数组，它的第i个元素是一支给定的股票在第i天的价格。设计一个算法来找到最大的利润。你最多可以完成两笔交易。
 **注意事项**
你不可以同时参与多笔交易(你必须在再次购买前出售掉之前的股票)
**样例**
给出一个样例数组 [4,4,6,1,1,4,2,5], 返回 6

# solution

## Approach ##1 two dp
最多可以进行两次交易，无非就是卖出之后还要再买进一次。
所以我们用两个dp数组分别记录两次交易的最大收益。

left[i]:表示从第一天到第i天进行一次交易最大的收益
right[i]:表示从第i天到最后一天进行一次交易最大的收益
填满这两个动态规划数组之后，我们直接循环一次，找到最大值就是本题进行两次交易的答案。
```
class Solution {
    /**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    public int maxProfit(int[] prices) {
        // write your code here
        if (prices == null || prices.length <= 1) {
            return 0;
        }

        int[] left = new int[prices.length];
        int[] right = new int[prices.length];

        // DP from left to right;
        left[0] = 0;
        int min = prices[0];
        for (int i = 1; i < prices.length; i++) {
            min = Math.min(prices[i], min);
            left[i] = Math.max(left[i - 1], prices[i] - min);
        }

        //DP from right to left;
        right[prices.length - 1] = 0;
        int max = prices[prices.length - 1];
        for (int i = prices.length - 2; i >= 0; i--) {
            max = Math.max(prices[i], max);
            right[i] = Math.max(right[i + 1], max - prices[i]);
        }

        int profit = 0;
        for (int i = 0; i < prices.length; i++){
            profit = Math.max(left[i] + right[i], profit);  
        }

        return profit;
    }
};
```

## Approach ##2 dp
可以参考Best Time to Buy and Sell Stock IV的解析，将求出进行k次交易的最大收益，所以只要令其k=2就是本题的解法
