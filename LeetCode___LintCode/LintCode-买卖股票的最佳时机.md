# 题目
假设有一个数组，它的第i个元素是一支给定的股票在第i天的价格。如果你最多只允许完成一次交易(例如,一次买卖股票),设计一个算法来找出最大利润。

样例
给出一个数组样例 [3,2,3,1,2], 返回 1 

# 分析
贪心法，一个记录目前为止的最小值，一个记录目前的最大profit，随时更新即可

# 代码
```
public class Solution {
    /**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    public int maxProfit(int[] prices) {
        // write your code here
        if(prices == null || prices.length == 0)
    		return 0;
    	
    	int min = Integer.MAX_VALUE;  //store the min value that until now;
    	int profit = 0;   //store the profit, initial value is zero;
    	
    	for(int i:prices) {
    		min = Math.min(min, i);
    		profit = Math.max(profit, i-min);
    	}
    	return profit;
    }
}
```
