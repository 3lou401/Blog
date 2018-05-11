# 题目
实现 int sqrt(int x)函数，计算并返回 *x* 的平方根。

**样例**
sqrt(3) = 1
sqrt(4) = 2
sqrt(5) = 2
sqrt(10) = 3

# 分析
这道题是典型的二分法的运用

# 代码
```
class Solution {
    /**
     * @param x: An integer
     * @return: The sqrt of x
     */
    public int sqrt(int x) {
        // write your code here
        if (x == 1 || x == 0) {
            return x;
        }
        
        int left = 1;
        int right = x;
        
        while (left < right - 1) {
            int mid = left + (right - left) / 2;
            int quo = x / mid;
            
            if (quo == mid) {
                return quo;
            // mid is too big    
            } else if (quo < mid) {
                right = mid;
            } else {
                left = mid;
            }
        }
        
        return left;
    }
}
```
