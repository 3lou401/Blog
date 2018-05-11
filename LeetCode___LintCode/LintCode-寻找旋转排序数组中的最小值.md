# 题目
假设一个旋转排序的数组其起始位置是未知的（比如0 1 2 4 5 6 7 可能变成是4 5 6 7 0 1 2）。

你需要找到其中最小的元素。

你可以假设数组中不存在重复的元素。

样例
给出[4,5,6,7,0,1,2]  返回 0

# 分析
很简单的二分法，多加了两种情况

# 代码
```
public class Solution {
    /**
     * @param nums: a rotated sorted array
     * @return: the minimum number in the array
     */
    public int findMin(int[] nums) {
        // write your code here
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int start = 0, end = nums.length - 1;
        //int target = nums[nums.length - 1];
        
        // find the first element <= target
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if ( nums[mid] > nums[start] && nums[mid] < nums[end] ) {
                return nums[0];
            } else if(nums[mid] > nums[end]) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if (nums[start] < nums[end]) {
            return nums[start];
        } 
            return nums[end];
    }
}
```
