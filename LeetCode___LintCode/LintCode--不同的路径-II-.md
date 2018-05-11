# 题目
"[不同的路径](http://www.lintcode.com/problem/unique-paths/)" 的跟进问题：
现在考虑网格中有障碍物，那样将会有多少条不同的路径？
网格中的障碍和空位置分别用 1 和 0 来表示。

** 注意事项
m 和 n 均不超过100 **

**样例**
如下所示在3x3的网格中有一个障碍物：

![diffRoute2.PNG](http://upload-images.jianshu.io/upload_images/1234352-1c34f4716dc67295.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一共有2条不同的路径从左上角到右下角。

# 分析
依然和前述的不同路径问题一样，就是增加了障碍点，遇到障碍点我们就跳过即可。

# 代码
```
public class Solution {
    /**
     * @param obstacleGrid: A list of lists of integers
     * @return: An integer
     */
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        // write your code here
        int[][] dp = new int[100][100];  
        int r=obstacleGrid.length;  
        int c=obstacleGrid[0].length;  
  
        for(int i=0;i<r;++i){  //第一列  
            if(0==obstacleGrid[i][0])  //因为只能向下或者向右走，所以从起点来到该点方法有一种  
                dp[i][0]=1;  
            else  
                break;         //阻塞了，接下来不用考虑下面的点，肯定走不到  
        }     
        for(int i=0;i<c;++i){ //第一行 同理  
            if(0==obstacleGrid[0][i])  
                dp[0][i]=1;  
            else  
                break;  
        }     
        for(int i=1;i<r;++i)  
            for(int j=1;j<c;++j){  
                if(0==obstacleGrid[i][j])  
                    dp[i][j]=dp[i-1][j]+dp[i][j-1]; //从起点走到该点有几种途径等于  
                                                    //从起点到该点上面的点和该点左  
                                                    //边的点途径之和  
                else                                  
                    dp[i][j]=0;  
            }  
        return dp[r-1][c-1];  
    }
}
```
