# 题目
实现 pow(x,n)

 注意事项

不用担心精度，当答案和标准输出差绝对值小于1e-3时都算正确

样例
Pow(2.1, 3) = 9.261
Pow(0, 1) = 0
Pow(1, 0) = 1

# 分析
二分法递归
详细见代码注释

# 代码
```
public class Solution {
    /**
     * @param x the base number
     * @param n the power number
     * @return the result
     */
    public double myPow(double x, int n) {

        // Write your code here
        if (x == 0) {
            return 0;
        }
        
        // base case: when n = 0, the result is 1;
        if (n == 0) {
            return 1;
        }
        
        /*
        递归的主体部分
        */
        
        // X^(-n) = X^(n + 1) * X
        // X^n = 1/(x^(-n))
        if (n < 0) {
            double ret = x * myPow(x, -(n + 1));
            return (double)1/ret;
        }
        
        // 将求pow对半分。再将结果相乘
        double ret = myPow(x, n / 2);
        ret = ret * ret;
        
        //如果有余数，再乘以x本身。
        if (n % 2 != 0) {
            ret = ret * x;
        }
        
        return ret;
    }
}
```
