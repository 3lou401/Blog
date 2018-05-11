# 题目
假设一个旋转排序的数组其起始位置是未知的（比如0 1 2 4 5 6 7 可能变成是4 5 6 7 0 1 2）。

你需要找到其中最小的元素。

数组中可能存在重复的元素。

样例
给出[4,4,5,6,7,0,1,2]  返回 0

# 分析
这次可以出现重复元素，很简单，只要判断的时候加等于的判断即可，如果mid等于end，那么end--最小值肯定还在其中。

# 代码
```
public class Solution {
    /**
     * @param num: a rotated sorted array
     * @return: the minimum number in the array
     */
    public int findMin(int[] nums) {
        // write your code here
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int start = 0, end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if(nums[mid]>nums[start] && nums[mid] < nums[end])
                return nums[0];
            else if (nums[mid] == nums[end]) {
                // if mid equals to end, that means it's fine to remove end
                // the smallest element won't be removed
                end--;
            } else if (nums[mid] < nums[end]) {
                end = mid;
                // of course you can merge == & <
            } else {
                start = mid;
                // or start = mid + 1
            }
        }
        
        if (nums[start] <= nums[end]) {
            return nums[start];
        }
        return nums[end];
    }
}
```
