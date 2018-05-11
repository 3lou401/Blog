# 题目
Given an integer array nums with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

 ** 注意事项 **
The different sequences are counted as different combinations.

样例
Given nums = [1, 2, 4], target = 4

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-408bd56fb15e41e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

return 6

# 分析
这个问题的思路是，4可以由nums里面的2,2组成，那么2又可以由nums里面的什么组成呢？就是分别把每个都作为target计算，结果存入dp[target]

# 代码
```
public int backPackVI(int[] nums, int target) {
        // Write your code here
        int[] dp = new int[target+1];
        dp[0] = 1;
        for (int i = 1; i <= target; i++) {
            for (int num: nums) {
                if (num <= i) dp[i] += dp[i-num];
            }
        }
        return dp[target];
    }
```
