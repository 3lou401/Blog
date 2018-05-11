# 题目

给定一个排序数组，在原数组中删除重复出现的数字，使得每个元素只出现一次，并且返回新的数组的长度。

不要使用额外的数组空间，必须在原地没有额外空间的条件下完成。

**样例**给出数组A =**[1,1,2]**，你的函数应该返回长度2，此时A=**[1,2]**。

# 分析
类似这类数组原地删除问题，设置两个指针，一快一慢，快的指针负责遍历，慢的指针负责筛选保留的数据。
可参考前几篇这类的数组问题

# 代码
```
public class Solution {
    /**
     * @param A: a array of integers
     * @return : return an integer
     */
    public int removeDuplicates(int[] nums) {
        // write your code here
        if(nums.length == 0) return 0;
        int slow = 0;
        int fast = 0;
        int count = 0;
        while(fast < nums.length-1)
        {
        	if(nums[fast+1] != nums[slow])
        	{
        		nums[slow] = nums[fast];
                slow++;
                count++;
        	}
        	fast++;
        }
        return count;
    }
}
```


