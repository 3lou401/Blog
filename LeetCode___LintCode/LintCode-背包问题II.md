# 题目
给出n个物品的体积A[i]和其价值V[i]，将他们装入一个大小为m的背包，最多能装入的总价值有多大？

** 注意事项 **
A[i], V[i], n, m均为整数。你不能将物品进行切分。你所挑选的物品总体积需要小于等于给定的m。

** 样例 **
对于物品体积[2, 3, 5, 7]和对应的价值[1, 5, 2, 4], 假设背包大小为10的话，最大能够装入的价值为9。

# 分析
此题和背包问题1是类似的，只是将重要的计算换成了价值，具体可参考问题的解析，也可以参考代码

同样有两种方法
```
public int backpackII(int m, int[] A, int[] V) {
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
				dp[0][j] = V[0];
		}
		
		/*初始化第一行和第一列就可以使用状态转移方程了
		状态转移方程为：dp[i][j] = Math.max(dp[i - 1][j - A[i]] + V[i], dp[i-1][j]);
		由状态转移方程知道，要求出dp[i][j]只需要知道它上面和左上面的值就可以了
		所以，只要初始化第一行第一列就可以求出全部的*/
		
		for(int i=1;i<A.length;i++) {
			for(int j=1;j<m+1;j++) {
				if(A[i]<j)
					dp[i][j] = Math.max(dp[i - 1][j - A[i]] + V[i], dp[i-1][j]);
				else
					dp[i][j] = dp[i-1][j];
			}
		}
			
		
		
		return dp[A.length-1][m];
	}
```

```
public int backpackII_I(int m,int[] A,int[] V) {
		int[] dp = new int[m+1];
		
		for(int i=0;i<A.length;i++) {
			for(int j=m;j>0;j--) {
				if(j>=A[i])
					dp[j] = Math.max(dp[j], dp[j-A[i]]+V[i]);
			}
		}
		
		return dp[m];
	}
```
