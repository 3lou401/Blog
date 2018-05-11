> You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.
Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.
**Credits:**Special thanks to [@ifanchu](https://oj.leetcode.com/discuss/user/ifanchu) for adding this problem and creating all test cases. Also thanks to [@ts](https://oj.leetcode.com/discuss/user/ts) for adding additional test cases.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
假设你是一个专业的窃贼，准备沿着一条街打劫房屋。每个房子都存放着特定金额的钱。你面临的唯一约束条件是：相邻的房子装着相互联系的防盗系统，且 当相邻的两个房子同一天被打劫时，该系统会自动报警。
给定一个非负整数列表，表示每个房子中存放的钱， 算一算，如果今晚去打劫，你最多可以得到多少钱 在不触动报警装置的情况下。
**样例**
给定 [3, 8, 4], 返回 8.

# 分析
动态规划解决。
dp[i]:打劫前i个房子最多可以得到的钱
遇到第i个房子，两种选择打劫或者不打劫，所以
状态转移方程：
dp[i] = Max(dp[i-1],res[i-2]+A[i-1])
初始条件：
dp[0]= 0
dp[1] = A[0]

# 代码
```
public class Solution {
    /**
     * @param A: An array of non-negative integers.
     * return: The maximum amount of money you can rob tonight
     */
    //---方法一---
    public long houseRobber(int[] A) {
        // write your code here
        int n = A.length;
        if(n == 0)
            return 0;
        long []res = new long[n+1];
        
        
        res[0] = 0;
        res[1] = A[0];
        for(int i = 2; i <= n; i++) {
            res[i] = Math.max(res[i-1], res[i-2] + A[i-1]);
        }
        return res[n];
    }
    //---方法二---
    public long houseRobber(int[] A) {
        // write your code here
        int n = A.length;
        if(n == 0)
            return 0;
        long []res = new long[2];
        
        
        res[0] = 0;
        res[1] = A[0];
        for(int i = 2; i <= n; i++) {
            res[i%2] = Math.max(res[(i-1)%2], res[(i-2)%2] + A[i-1]);
        }
        return res[n%2];
    }
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-3ccbac4483dc6c8a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 方法二
同样的是动态规划，由于每个点只存在两个状态，即打劫或者不打劫，所以我们可以使用一个变量来保存这个状态，所以利用一个二维数组
dp[i][0]:表示不打劫第i个房屋的最大价值
dp[i][1]:表示打劫第i个房屋的最大价值

```
public class Solution {
    public int rob(int[] nums) {
        if(nums == null || nums.length == 0)
	    		return 0;
	    	int[][] dp = new int[nums.length+1][2];
	    	
	    	for(int i=1;i<=nums.length;i++) {
	    		dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1]);
	    		dp[i][1] = nums[i-1] + dp[i-1][0];
	    	}
	    	
	    	return Math.max(dp[nums.length][0], dp[nums.length][1]);
    }
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-8f8f41484c7a3afc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
