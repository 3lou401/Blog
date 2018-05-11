# 题目
跟进“删除重复数字”：

如果可以允许出现两次重复将如何处理？

**样例**给出数组A =**[1,1,1,2,2,3]**，你的函数应该返回长度5，此时A=**[1,1,2,2,3]**。

# 分析
这是上一篇删除数组中重复元素的加强版，显然主要思路是不变的，为了控制重复出现的次数的话，显然需要设置一个变量来记录重复的次数，然后根据这个变量的值来判断是否保留或者删除。

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
        int fast;
        int slow = 1;
        int count = 0;
        for(fast = 1; fast < nums.length; ++fast)
        {
            if(nums[fast] == nums[slow-1])
            {
                count++;
                if(count>=2)
                	continue;
            }
            else
            	count = 0;
            nums[slow] = nums[fast];
            slow++;
        }
        return slow;
    }
}
```
