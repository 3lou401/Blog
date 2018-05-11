Given an array of integers, every element appears twice except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

# 题目
给出2*n + 1 个的数字，除其中一个数字之外其他每个数字均出现两次，找到这个数字。

**样例**
给出** [1,2,2,1,3,4,3]**，返回 4

# 分析
显然用异或就行了，因为其他的数均出现两次

# 代码
```
public class Solution {
    /**
      *@param A : an integer array
      *return : a integer 
      */
    public class Solution {
    public int singleNumber(int[] nums) {
    int ans =0;
    
    int len = nums.length;
    for(int i=0;i!=len;i++)
        ans ^= nums[i];
    
    return ans;
    
}
}
}
```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-d8b5637c657c9dc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
