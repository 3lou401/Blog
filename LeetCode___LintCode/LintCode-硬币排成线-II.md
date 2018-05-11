# 题目
有 n 个不同价值的硬币排成一条线。两个参赛者轮流从左边依次拿走 1 或 2 个硬币，直到没有硬币为止。计算两个人分别拿到的硬币总价值，价值高的人获胜。

请判定 第一个玩家 是输还是赢？

**样例**
给定数组 A = [1,2,2], 返回 true.

给定数组 A = [1,2,4], 返回 false.

# 分析
这道题运用动态规划来解决。
dp[i] 表示从i到end 能拿到的最大值
一个明显的情况就是当len<=2时，这时候第一个拿的只要全拿走就行了，所以肯定是第一个人赢。然后我们分析
* 当i=len的时候，dp[len]没得可拿，所以dp[len]=0
* 当i=len-1的时候，dp[len-1]只有一个可以拿，所以dp[len-1] = values[len-1];
* 当i = len-2的时候，dp[len-2]有两个可拿，当然是直接拿走,所以dp[len-2] = values[len-1]+values[len-2];
* 当i=len-3的时候，剩下最后三个，这时候如果拿一个，对方就会拿走两个，所以，这次要拿两个，所以dp[len-3] = values[len-2]+ values[len-3];
* 当i = len-4以及以后的情况中，显然可以选择拿一个或者拿两个两种情况，我们自然是选择拿最多的那个作为dp的值，那么我们就分分析这两种情况：
第一种，只拿一个,那么对手可能拿两个或者一个，对手肯定是尽可能多拿，所以我们要选择尽可能小的那个，所以dp[i] = values[i] + min(dp[i+2],dp[i+3])
第二种，拿两个，同样的情况，dp[i] = values[i]+ values[i+1]+min(dp[i+3],dp[i+4])
然后我们取这两种情况下的最大值。

# 代码
```
public class Solution {
    /**
     * @param values: an array of integers
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int[] values) {
        // write your code here
        // dp 表示从i到end 的最大值  
        // int values[] ={1,2,4,3,4,8,5,6,12};  
        int len = values.length;
        int[] dp = new int[len+1];
        if(len<=2)
        	return true;
        dp[len] = 0;
        dp[len-1] = values[len-1];
        dp[len-2] = values[len-2] + values[len-1];
        dp[len-3] = values[len-3] + values[len-2];
        for(int i = len-4;i>=0;i--) {
        	dp[i] = Math.max(values[i]+Math.min(dp[i+2],dp[i+3]), values[i]+values[i+1]+Math.min(dp[i+3], dp[i+4]));
        }
        
        int sum=0;
        for(int a:values)
        	sum+=a;
        return dp[0] > sum-dp[0]; 
    }
}
```
