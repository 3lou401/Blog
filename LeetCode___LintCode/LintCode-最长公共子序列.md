# 题目
给出两个字符串，找到最长公共子序列(LCS)，返回LCS的长度。

**说明**
最长公共子序列的定义：

* 最长公共子序列问题是在一组序列（通常2个）中找到最长公共子序列（注意：不同于子串，LCS不需要是连续的子串）。该问题是典型的计算机科学问题，是文件差异比较程序的基础，在生物信息学中也有所应用。
* https://en.wikipedia.org/wiki/Longest_common_subsequence_problem

**样例**

给出"ABCD" 和 "EDCA"，这个LCS是 "A" (或 D或C)，返回1

给出 "ABCD" 和 "EACB"，这个LCS是"AC"返回 2

# 分析
典型的动态规划问题
dp[i][[j]：表示前i个和前j个字符最大LCS
当A[i] = B[i]的时候：
那么显然dp[i][j] = dp[i-1][j-1] + 1,因为dp[i-1][j-1]就是前最大的情况
当A[i] != B[i]:
dp[i][j] = max(dp[i][j-1],dp[i-1][j])
初始条件很简单，显然ii,j有一个为0,dp都是0

# 代码
```
public class Solution {
    /**
     * @param A, B: Two strings.
     * @return: The length of longest common subsequence of A and B.
     */
    public int longestCommonSubsequence(String A, String B) {
        int n = A.length();
        int m = B.length();
        
        int[][] dp = new int[n+1][m+1];
        
        for(int i=1;i<=n;i++)
        	for(int j=1;j<=m;j++) {
        		dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
        		if(A.charAt(i-1) == B.charAt(j-1))
        			dp[i][j] = dp[i-1][j-1] + 1;
        	}
        
        return dp[n][m];
    }
}

```
