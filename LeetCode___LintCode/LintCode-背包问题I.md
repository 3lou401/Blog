# 题目
在n个物品中挑选若干物品装入背包，最多能装多满？假设背包的大小为m，每个物品的大小为A[i]

**样例**
如果有4个物品**[2, 3, 5, 7]**
如果背包的大小为**11**，可以选择**[2, 3, 5]**装入背包，最多可以装满**10**的空间。
如果背包的大小为**12**，可以选择**[2, 3, 7]**装入背包，最多可以装满**12**的空间。
函数需要返回最多能装满的空间大小。


# 分析
都可以简化为一位数组背包类型题一定要熟练画【表格】！对理解有很大帮助！！因为更改每一行数据时，只需要知道【上面的】和【左上面的】，所以从后向前添改不会出错。更复杂的背包（04，05，06……）就不是所有都能简化了的。

# 二维解法的代码
```
public int backpack(int m, int[] A) {
		int[][] dp = new int[A.length][m+1];
		//dp[i][j]为当背包总重量为j且有前i个物品时，背包最多装满dp[i][j]的空间
		
		//初始化动态规划矩阵的第一列
		//当背包空间为0时，不管物品多大，能放的空间都是0
		for(int i=0;i<A.length;i++)
			dp[i][0] = 0;
		
		//初始化动态规划矩阵第一行
		//当放入第一个物品时，如果背包空间大于物品，就放入，dp就等于第一个物品的重量，否则，就不放入，dp就为0
		for(int j=1;j<m+1;j++) {
			if(A[0]>j)
				dp[0][j] = 0;
			else
				dp[0][j] = A[0];
		}
		
		/*初始化第一行和第一列就可以使用状态转移方程了
		状态转移方程为：dp[i][j] = Math.max(dp[i - 1][j - A[i]] + A[i], dp[i-1][j]);
		由状态转移方程知道，要求出dp[i][j]只需要知道它上面和左上面的值就可以了
		所以，只要初始化第一行第一列就可以求出全部的*/
		
		for(int i=1;i<A.length;i++) {
			for(int j=1;j<m+1;j++) {
				if(A[i]<j)
					dp[i][j] = Math.max(dp[i - 1][j - A[i]] + A[i], dp[i-1][j]);
				else
					dp[i][j] = dp[i-1][j];
			}
		}
			
		
		
		return dp[A.length-1][m];
	}
```

# 优化空间复杂度代码
```
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        // write your code here
        int[] dp = new int[m+1];
        for (int i = 0; i < A.length; i++) {
            for (int j = m; j > 0; j--) {
                if (j >= A[i]) {
                    dp[j] = Math.max(dp[j], dp[j-A[i]] + A[i]);
                }
            }
        }
        return dp[m];
    }
}
```
