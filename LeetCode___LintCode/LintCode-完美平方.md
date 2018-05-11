# 题目
给一个正整数 n, 找到若干个完全平方数(比如1, 4, 9, ... )使得他们的和等于 n。你需要让平方数的个数最少。

**样例**
给出 n = 12, 返回 3 因为 12 = 4 + 4 + 4。
给出 n = 13, 返回 2 因为 13 = 4 + 9。

# 分析
动态规划，dp[i]表示n=i时，最小的个数。
显然可以求出dp值为1的值，并以此进行判断

# 代码
```
public class Solution {
    /**
     * @param n a positive integer
     * @return an integer
     */
    public int numSquares(int n) {
        // Write your code here
        int[] dp = new int[n+1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        
        for(int i=0;i*i<=n;i++) {
        	dp[i*i] = 1;
        }
        
        for(int i=1;i<=n;i++) {
        	for(int j=1;j*j<=i;j++) {
        		dp[i] = Math.min(dp[i], dp[i-j*j]+1);
        	}
        }

        return dp[n];
    }
}
```
