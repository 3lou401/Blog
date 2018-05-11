> Say you have an array for which the *i*th
 element is the price of a given stock on day *i*.
Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
***
> 假设有一个数组，它的第i个元素是一个给定的股票在第i天的价格。设计一个算法来找到最大的利润。你可以完成尽可能多的交易(多次买卖股票)。然而,你不能同时参与多个交易(你必须在再次购买前出售股票)。
**样例**
给出一个数组样例[2,1,2,0,1], 返回 2

# SOLUTION
## Approach #1 (Simple One Pass) 贪心法
很简单，既然可以进行任意次的交易，那么我们就将所有前一天比后一天价格低的都进行交易，因为只有这样，才会增加收益，所以我们进行一次遍历之后，就可以求出的最大收益值。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-ab7484f17c0eb357.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以看到 A+B+C就等于D
```
class Solution {
    public int maxProfit(int[] prices) {
        int maxprofit = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1])
                maxprofit += prices[i] - prices[i - 1];
        }
        return maxprofit;
    }
}
```
Complexity Analysis
* Time complexity : O(n)O(n). Single pass.
* Space complexity: O(1)O(1). Constant space needed.

## Approach #2 (Peak Valley Approach)
假设给定的数组是：
[7, 1, 5, 3, 6, 4].


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-3fd2f65fd0cd597b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们观察这个图，可以得出我们的收益是由连续的山顶山谷组成的

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-05569e0e0b632afe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里最关键的点就是我们找到一个山谷就要立即跟他最近的山顶求和，否则如何越到下一个山顶求和，就不是最大的收益了。正如图中所示

```
class Solution {
    public int maxProfit(int[] prices) {
        int i = 0;
        int valley = prices[0];
        int peak = prices[0];
        int maxprofit = 0;
        while (i < prices.length - 1) {
            while (i < prices.length - 1 && prices[i] >= prices[i + 1])
                i++;
            valley = prices[i];
            while (i < prices.length - 1 && prices[i] <= prices[i + 1])
                i++;
            peak = prices[i];
            maxprofit += peak - valley;
        }
        return maxprofit;
    }
}
```
