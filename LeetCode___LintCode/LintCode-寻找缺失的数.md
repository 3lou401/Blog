# 题目
给出一个包含 0 .. N 中 N 个数的序列，找出0 .. N 中没有出现在序列中的那个数。

样例
N = 4 且序列为 [0, 1, 3] 时，缺失的数为2。

# 分析

方法一： 求和法
由于缺失了一个元素，那么我们可以用高斯公式先求出完整的序列的和，然后求出给定数组的和，最后相减就是缺失的元素，效率高
```
public class Solution {
    /**    
     * @param nums: an array of integers
     * @return: an integer
     */
    public int findMissing(int[] nums) {
        // write your code here
        
        int n = nums.length;
        long sum=0;
        for(int i=0;i<n;i++) {
        	sum += nums[i];
        }
        
        return (int) (0.5 * (n+1) * (n) - sum);
    }
}
```

# 方法二 交换法
我们已经知道每个位置的元素应该出现的值，那么就遍历一遍数组，如果nums【i】!=i，就把他换到它应该在的位置，最后遍历一遍就得出那个数
```
public class Solution {
    /**    
     * @param nums: an array of integers
     * @return: an integer
     */
    public int findMissing(int[] nums) {
        // write your code here
        int n = nums.length, i = 0;
        while (i<n) {
            while (nums[i]!=i && nums[i]<n) {
                int t = nums[i];
                nums[i] = nums[t];
                nums[t] = t;
            }
            ++i;
        }
        for (i=0; i<n; ++i)
            if (nums[i]!=i) return i;
        return n;
    }
}
```
