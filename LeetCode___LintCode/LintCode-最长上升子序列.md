# 题目
给定一个整数序列，找到最长上升子序列（LIS），返回LIS的长度。

**说明**
最长上升子序列的定义：
最长上升子序列问题是在一个无序的给定序列中找到一个尽可能长的由低到高排列的子序列，这种子序列不一定是连续的或者唯一的。[https://en.wikipedia.org/wiki/Longest_increasing_subsequence](https://en.wikipedia.org/wiki/Longest_increasing_subsequence)

**样例**
给出 [5,4,1,2,3]，LIS 是 [1,2,3]，返回 3
给出 [4,2,4,5,3,7]，LIS 是 [2,4,5,7]，返回 4

# 分析
dp[i]：记录前i个子序列最长的上升子序列，每加入一个数，可能很多子序列都会发生变化，所以要一个内层循环判断，如果大于，就在之前的基础上加1，最后用一个变量记录最大值。

# 代码
dp[i]：记录前i个子序列最长的上升子序列，每加入一个数，可能很多子序列都会发生变化，所以要一个内层循环判断，如果大于，就在之前的基础上加1，最后用一个变量记录最大值。

```
public class Solution {
    /**
     * @param nums: The integer array
     * @return: The length of LIS (longest increasing subsequence)
     */
    public int longestIncreasingSubsequence(int[] nums) {
        if(nums==null || nums.length<1) return 0;  
          
        int[] dp = new int[nums.length];
        dp[0] = 1;
        
        int max = dp[0];
        
        for(int i=0;i<nums.length;i++) {
        	dp[i] = 1;
        	for(int j=0;j<i;j++) {
        		if(nums[i]>nums[j])
        			dp[i] = Math.max(dp[i], dp[j]+1);
        	}
        	max = Math.max(dp[i], max);
        }
        return max;
    }
}

```
