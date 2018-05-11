# 题目
给定一个排序的整数数组（升序）和一个要查找的整数target，用O(logn)的时间查找到target第一次出现的下标（从0开始），如果target不存在于数组中，返回-1。

**样例**
在数组[1, 2, 3, 3, 4, 5, 10]中二分查找3，返回2。

#分析
简单的二分查找的实现

#代码
```
class Solution {
    /**
     * @param nums: The integer array.
     * @param target: Target to find.
     * @return: The first position of target. Position starts from 0.
     */
    public int binarySearch(int[] nums, int target) {
        //write your code here
        int left = 0;
        int right = nums.length - 1;
        int mid;
        while(left < right)
        {
            mid = (right - left) / 2 + left;
            if(target > nums[mid])
                left = mid + 1;
            else if(target < nums[mid])
                right = mid - 1;
            else
                right = mid;
        }
        if(nums[right] == target)
            return right;
        return -1;
    }
}
```
