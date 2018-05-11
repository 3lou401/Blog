# 题目
给定一个整数数组，找到一个具有最小和的子数组。返回其最小和。

** 注意事项
子数组最少包含一个数字 **

**样例**
给出数组**[1, -1, -2, 1]**，返回 -3

# 分析
判断加与不加的情况，这道题的解法很巧妙，类似于背包问题。
每个数组的元素都有两种情况，加与不加，所以我们从第一个元素开始判断，包括第一个元素时，和不包括第一个元素的情况取最小值，进行判断选择。

# 代码
```
public class Solution {
    /**
     * @param nums: a list of integers
     * @return: A integer indicate the sum of minimum subarray
     */
    public int minSubArray(ArrayList<Integer> nums) {
        // write your code
        int min_ending_here = nums.get(0);
        int min_so_far = nums.get(0);
        for(int i=1;i<nums.size();i++)
        {
        	min_ending_here = Math.min(nums.get(i), nums.get(i)+ min_ending_here);
        	min_so_far = Math.min(min_ending_here, min_so_far);
        }
        return min_so_far;
    }
}
```
# 最大子数组
这道题的思路和最小子数组是一样的，只要更改判断小为判断大就可以了
这里就直接贴出代码
```
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A integer indicate the sum of max subarray
     */
    public int maxSubArray(int[] nums) {
        // write your code
        int max_ending_here = nums[0];
        int max_so_far = nums[0];
        for( int i =1 ;i<nums.length; i++) {
            max_ending_here = Math.max( nums[i] , nums[i] + max_ending_here );
            max_so_far = Math.max( max_so_far, max_ending_here);
        }
        return max_so_far;
    }
}
```
