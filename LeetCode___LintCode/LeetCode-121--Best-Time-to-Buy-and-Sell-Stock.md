> Say you have an array for which the *i*th
 element is the price of a given stock on day *i*.
If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.
**Example 1:**
Input: [7, 1, 5, 3, 6, 4]Output: 5max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)
**Example 2:**
Input: [7, 6, 4, 3, 1]Output: 0In this case, no transaction is done, i.e. max profit = 0.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.

# 题目
假设有一个数组，它的第i个元素是一支给定的股票在第i天的价格。如果你最多只允许完成一次交易(例如,一次买卖股票),设计一个算法来找出最大利润。
**样例**
给出一个数组样例 [3,2,3,1,2], 返回 1 

# Solution

## Approach #1 (Brute Force) 暴力搜索
思路很简单，我们就要找出数组中，差值最大的两个数，要求是前一个数小，后一个数大，那么遍历两边即可，找出所有这样的差值，找出其中最大的一个
```
public int maxProfit(int prices[]) {
        int maxprofit = 0;
        for (int i = 0; i < prices.length - 1; i++) {
            for (int j = i + 1; j < prices.length; j++) {
                int profit = prices[j] - prices[i];
                if (profit > maxprofit)
                    maxprofit = profit;
            }
        }
        return maxprofit;
    }
```

## Approach #2 (One Pass) Greedy 贪婪法
 通过一遍遍历，找出到此为止之前最小的min,并与当前的值求差，保存最大的差值，遍历完，最后的差值即是最大差值。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-9891500646221b4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
public int maxProfit(int prices[]) {
        int minprice = Integer.MAX_VALUE;
        int maxprofit = 0;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < minprice)
                minprice = prices[i];
            else if (prices[i] - minprice > maxprofit)
                maxprofit = prices[i] - minprice;
        }
        return maxprofit;
    }
```
