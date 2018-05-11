# 题目
给定一个数字三角形，找到从顶部到底部的最小路径和。每一步可以移动到下面一行的相邻数字上。
** 注意事项
如果你只用额外空间复杂度O(n)的条件下完成可以获得加分，其中n是数字三角形的总行数。**

**样例**
比如，给出下列数字三角形：

![数字三角形.PNG](http://upload-images.jianshu.io/upload_images/1234352-6cca22e33751ecb7.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从顶到底部的最小路径和为11 ( 2 + 3 + 5 + 1 = 11)。

# 分析1 （常规的动态规划解法）
类似于前篇最小路径和的分析，我们假设到i,j的代价路径和为dp[i][j]
那么走到i，j就只有两种情况，一种是从i-1,j-1过来，一种是从i-1,j过来。
找到状态转移方程：
dp[i][j] = Math.min(dp[i-1][j], dp[i-1][j-1]) + triangle[i][j];
然后我们初始化i=0的dp和i=j的dp，最后就可以利用状态转移方程算出结果

```
public class Solution {
    /**
     * @param triangle: a list of lists of integers.
     * @return: An integer, minimum path sum.
     */
    public int minimumTotal(int[][] triangle) {
        // write your code here
         //从底往上，把每一行的元素改为其下一行能与之相加的两个数得到的和的最小值
        int row = triangle.length;
        //int col = triangle[row-1].length;
        if(row < 0)
        	return row;
        if(row == 1)
        	return triangle[0][0];
        int[][] dp = new int[row+1][row+1];
        dp[0][0] = triangle[0][0];
        for(int i=1; i<row; i++) {  
            dp[i][0] = dp[i-1][0]+triangle[i][0];  
        }
        for(int i=1; i<row; i++) {  
            dp[i][i] = dp[i-1][i-1]+triangle[i][i];  
        }
        for(int i=2;i<row;i++)
        	for(int j=1;j<i;j++)
        		dp[i][j] = Math.min(dp[i-1][j], dp[i-1][j-1]) + triangle[i][j];
        
        int min = dp[row-1][0];
        for(int i=1;i<row;i++)
       	{
        	if(dp[row-1][i]< min)
        		min = dp[row-1][i];
       	}
        	
        return min;
    }
```

# 分析2 （如果你只用额外空间复杂度O(n)的条件）
从顶部到底部的最小路径和等于从底部到顶部的最小路径和 //从倒数第二层开始，从底层到每一层每个数字的最小路径长度等于，从底层到该层的下层相邻数字的最小路径长度中的较小值，加上该层该数字的值。

```
public class Solution {
    /**
     * @param triangle: a list of lists of integers.
     * @return: An integer, minimum path sum.
     */
    public int minimumTotal(int[][] triangle) {
        // write your code here
         //从底往上，把每一行的元素改为其下一行能与之相加的两个数得到的和的最小值
        int row = triangle.length;
        
        if(row < 1){
            return 0;
        }
        
        if(row == 1){
            return triangle[0][0];
        }
        
        for(int i=row-2;i>=0;i--){
            for(int j=0;j<triangle[i].length;j++){
                int min = Math.min(triangle[i+1][j], triangle[i+1][j+1]) + triangle[i][j];
                triangle[i][j] = min;
            }
        }
        
        return triangle[0][0];
    }
}
```
