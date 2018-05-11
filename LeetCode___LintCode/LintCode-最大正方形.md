# 题目
在一个二维01矩阵中找到全为1的最大正方形

**样例**
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
返回 4

# 分析
动态规划
dp[i][j]：最大正方形的边长
状态转移方程：
if(matrix[i][j] == 1)
 {
        			dp[i][j] = Math.min(dp[i-1][j-1], Math.min(dp[i-1][j], dp[i][j-1])) + 1;
        		}
        		else
        			dp[i][j] = 0;

# 代码
```
public class Solution {
    /**
     * @param matrix: a matrix of 0 and 1
     * @return: an integer
     */
    public int maxSquare(int[][] matrix) {
        int ans = 0;
        //得到行数
        int n = matrix.length;
        int m;
        //得到列数
        if(n > 0)
            m = matrix[0].length;
        else 
            return ans;
        //dp[i][j] 最大正方形的边长
        int[][] dp = new int [n][m];
        //初始化初始条件
        for(int i=0;i<n;i++)
        	if(matrix[i][0] == 1) {
        		dp[i][0] = matrix[i][0];
        		ans = 1;
        	}
        
        for(int i=0;i<m;i++)
        	if(matrix[0][i] == 1) {
        		dp[0][i] = matrix[0][i];
        		ans = 1;
        	}
        
        //状态转移方程，用ans存储最大边长
        for(int i=1;i<n;i++) {
        	for(int j=1;j<m;j++) {
        		if(matrix[i][j] == 1) {
        			dp[i][j] = Math.min(dp[i-1][j-1], Math.min(dp[i-1][j], dp[i][j-1])) + 1;
        		}
        		else
        			dp[i][j] = 0;
        		ans = Math.max(dp[i][j], ans);
        	}
        	
        }
        
        return ans*ans;
    }
}
```
