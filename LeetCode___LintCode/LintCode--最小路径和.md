# 题目
给定一个只含非负整数的m*n网格，找到一条从左上角到右下角的可以使数字和最小的路径。

** 注意事项
你在同一时间只能向下或者向右移动一步 **

# 分析
这是一道典型的动态规划的题目。
我们从尾分析，达到右下角的只有两种可能，一种是通过向下走到右下角，一种是通过向右走到右下角。那么到达右下角的最小代价，就是这两种情况下的相对小的那个加上右下角这一步的代价。
f(i,j) = MIN(f(i-1,j),f(i,j-1)) + grid(i,j)

# 代码
```
public class Solution {
    /**
     * @param grid: a list of lists of integers.
     * @return: An integer, minimizes the sum of all numbers along its path
     */
    public int minPathSum(int[][] grid) {
        // write your code here
        int m = grid.length;
        int n = grid[0].length;
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        for(int i=1;i<m;i++)
        	dp[i][0] = dp[i-1][0] + grid[i][0];
        for(int j=1;j<n;j++)
        	dp[0][j] = dp[0][j-1] + grid[0][j];
        for(int i=1;i<m;i++)
        	for(int j=1;j<n;j++)
        		dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
        return dp[m-1][n-1];
    }
    
}
```
