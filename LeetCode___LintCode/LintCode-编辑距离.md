# 题目
给出两个单词word1和word2，计算出将word1 转换为word2的最少操作次数。

你总共三种操作方法：
* 插入一个字符
* 删除一个字符
* 替换一个字符

**样例**
给出 work1="mart" 和 work2="karma"

返回 3

#分析
dp[i][j]表示前i个字符到前j个字符的最小操作数
状态转移方程比较简单
当第i个字符与第j个字符相等的时候,自然就是不考虑第i个字符和第j个字符的距离：
dp[i][j] = dp[i-1][j-1]
如果他们不相等，那么就有三种情况，
* 替换一个字符
dp[i][j] = dp[i-1][j]+1
* 删除一个字符
dp[i][j] = dp[i][j-1]+1
* 插入一个字符
dp[i][j] = dp[i-1][j-1]+1
然后取三种情况里最小的一个

# 代码
```
public class Solution {
    /**
     * @param word1 & word2: Two string.
     * @return: The minimum number of steps.
     */
    public int minDistance(String word1, String word2) {
       int n = word1.length();
	   int m = word2.length();
	   
	   int[][] dp = new int[n+1][m+1];
	   
	   for(int i=0;i<n+1;i++)
		   dp[i][0] = i;
	   
	   for(int i=0;i<m+1;i++)
		   dp[0][i] = i;
	   
	   for(int i=1;i<n+1;i++) {
		   for(int j=1;j<m+1;j++) {
			   //dp[i][j] = dp[i-1][j];
			   if(word1.charAt(i-1) == word2.charAt(j-1))
				   dp[i][j] = dp[i-1][j-1];
			   else
				   dp[i][j] = Math.min(dp[i-1][j], Math.min(dp[i-1][j-1], dp[i][j-1]))+1;
		   }
	   }
	   return dp[n][m];
    }
}
```
