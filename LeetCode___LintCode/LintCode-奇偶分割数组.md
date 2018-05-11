# 题目
分割一个整数数组，使得奇数在前偶数在后。

**样例**
给定[1, 2, 3, 4]，返回[1, 3, 2, 4]。

# 分析
这道题其实很熟悉。将奇数排在前，偶数排在后

是不是和快速排序中的partiton算法很类似。其实是类似的。

设置两个指针，一头一尾，分别寻找偶数和奇数

# 代码
```
public class Solution {
    /**
     * @param nums: an array of integers
     * @return: nothing
     */
    public void partitionArray(int[] nums) {
        // write your code here;
        int i=0;
        int j=nums.length - 1;
        while(i<j){
            while(nums[i]%2==1){
                i++;
            }
            while(nums[j]%2==0){
                j--;
            }
            if(i<j){
                nums[i]=nums[i]+nums[j];
                nums[j]=nums[i]-nums[j];
                nums[i]=nums[i]-nums[j];
            }

        }
    }
}
```
