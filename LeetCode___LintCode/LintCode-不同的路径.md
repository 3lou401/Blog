# 题目
有一个机器人的位于一个M×N个网格左上角（下图中标记为'Start'）。
机器人每一时刻只能向下或者向右移动一步。机器人试图达到网格的右下角（下图中标记为'Finish'）。
问有多少条不同的路径？

 ** 注意事项
n和m均不超过100 **

**样例**

![diffRoute.PNG](http://upload-images.jianshu.io/upload_images/1234352-1df8d96050ceb59e.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以上3 x 7的网格中，有多少条不同的路径？

# 分析
典型的动态规划问题
容易分析出状态转移方程：
dp[i][j] = dp[i-1][j] + dp[i][j-1];
初始条件，就是第一列和第一行，因为只能向下或向右走，所以，第一列和第一行的值都为1.

# 代码
```
public class Solution {
    /**
     * @param n, m: positive integer (1 <= n ,m <= 100)
     * @return an integer
     */
    public int uniquePaths(int m, int n) {
        // write your code here 
        int[][] dp = new int[m][n];
        for(int i = 0; i < n; i++) {
            dp[0][i] = 1;
        }
        for(int i = 0; i < m; i++) {
            dp[i][0] = 1;
        }
        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
}
```
