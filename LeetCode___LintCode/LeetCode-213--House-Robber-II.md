> **Note:** This is an extension of [House Robber](https://leetcode.com/problems/house-robber/).
After robbing those houses on that street, the thief has found himself a new place for his thievery so that he will not get too much attention. This time, all houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, the security system for these houses remain the same as for those in the previous street.
Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.
**Credits:**Special thanks to [@Freezen](https://oj.leetcode.com/discuss/user/Freezen) for adding this problem and creating all test cases.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
在上次打劫完一条街道之后，窃贼又发现了一个新的可以打劫的地方，但这次所有的房子围成了一个圈，这就意味着第一间房子和最后一间房子是挨着的。每个房子都存放着特定金额的钱。你面临的唯一约束条件是：相邻的房子装着相互联系的防盗系统，且 当相邻的两个房子同一天被打劫时，该系统会自动报警。
给定一个非负整数列表，表示每个房子中存放的钱， 算一算，如果今晚去打劫，你最多可以得到多少钱 在不触动报警装置的情况下。
**注意事项**
这题是[House Robber](http://www.lintcode.com/problem/house-robber/)的扩展，只不过是由直线变成了圈
**样例**
给出nums = [3,6,4], 返回　6，　你不能打劫3和4所在的房间，因为它们围成一个圈，是相邻的．

# 分析
打劫房屋问题的扩展，要求首尾不能相连，那就调用上一题中的方法两次，第一次不包括尾，第二次不包括首，最后求出最大就可以了

# 代码
```
public class Solution {
    public int rob(int[] nums) {
	        if (nums.length == 0) {
	            return 0;
	        }
	        if (nums.length == 1) {
	            return nums[0];
	        }
	        return Math.max(robber1(nums, 0, nums.length - 2), robber1(nums, 1, nums.length - 1));
	    }
	    public int robber1(int[] nums, int start, int end) {
	        if(start == end)
	            return nums[start];
	        if(end == start+1)
	            return Math.max(nums[start], nums[start+1]);
	        int[] dp = new int[end-start+2];
	        dp[start] = nums[start];
	        dp[start+1] = Math.max(nums[start], nums[start+1]);
	        
	        for(int i=start+2;i<=end;i++)
	        	dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i]);
	        
	        return dp[end];
	    }
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-6332a3c313c23e85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 方法二
```
public class Solution {
    public int rob(int[] nums) {
	        if (nums.length == 1) return nums[0];
    return Math.max(rob(nums, 0, nums.length - 2), rob(nums, 1, nums.length - 1));
	    }
	private int rob(int[] num, int lo, int hi) {
    int include = 0, exclude = 0;
    for (int j = lo; j <= hi; j++) {
        int i = include, e = exclude;
        include = e + num[j];
        exclude = Math.max(e, i);
    }
    return Math.max(include, exclude);
}
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-5da18041f17d4a95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
