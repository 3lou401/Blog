# 题目
假设有一个数组，它的第i个元素是一个给定的股票在第i天的价格。设计一个算法来找到最大的利润。你可以完成尽可能多的交易(多次买卖股票)。然而,你不能同时参与多个交易(你必须在再次购买前出售股票)。

样例
给出一个数组样例[2,1,2,0,1], 返回 2

# 分析

这个问题可以被看作找到数组中所有的上升序列问题。例如，对于给定的5，1，2，3，4
从1买进然后从4卖出，和从1买进，2卖出，2买进，3卖出效果是一样的.
所以我们遍历一遍数组，找到所有上升的的序列

# 代码
```
class Solution {
    /**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    public int maxProfit(int[] prices) {
        // write your code here
        int profit = 0;
    	for(int i=1;i<prices.length;i++) {
    		int diff = prices[i] - prices[i-1];
    		if(diff > 0)
    			profit += diff;
    	}
    	return profit;
    }
};
```
