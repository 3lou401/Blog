# 题目
假设你正在爬楼梯，需要n步你才能到达顶部。但每次你只能爬一步或者两步，你能有多少种不同的方法爬到楼顶部？

**样例**
比如n=3，1+1+1=1+2=2+1=3，共有3中不同的方法，返回 3

# 分析
典型的动态规划问题。
我们假设到达第n级台阶的方法数为f(n)，那么最后一步的时候，我们有两种选择，一是选择爬一步，那么这时候的方法数就是f(n-1),另一种选择是选择爬两步，方法数f(n-2).所以通过这个我们就可以得出f(n)的状态转移方程：
f(n)=f(n-1)+f(n-2)
显然看到这里，我们可以利用递归求解这个问题。

```
public class Solution {
    /**
     * @param n: An integer
     * @return: An integer
     */
    public int climbStairs(int n) {
        // write your code here
      if (n == 0 || n == 1) return 1;  
        if (n < 0) return 0;  
        return climbStairs(n - 1) + climbStairs(n - 2); 
    }
}
```

![climb.PNG](http://upload-images.jianshu.io/upload_images/1234352-6b8fd27d92d363ee.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

提交结果告诉我们超时了。所以我们不能用递归，需要采用效率更高的算法。
其实这类似于斐波那契数列，我们直接利用一个数组来记录f(n)就可以了。
```
public class Solution {
    /**
     * @param n: An integer
     * @return: An integer
     */
    public int climbStairs(int n) {
        // write your code here
      if (n == 1 || n==0) return 1;
      int[] f = new int[n+1];
      f[0] = 1;
      f[1] = 1;
      for(int i=2;i<=n;i++)
         f[i] = f[i-1] + f[i-2];
      return f[n];
    }
}

```
这种解法可以正确通过!
但我们可以更进一步优化算法，只采用两个变量即可
f(n) = f(n-1)+f(n-2)
f(n-1) = f(n) - f(n-2)
利用这个关系，
one = one + zero;
 zero  = one - zero;
# 代码
```
public class Solution {
    /**
     * @param n: An integer
     * @return: An integer
     */
    public int climbStairs(int n) {
        // write your code here
      int one = 0;
      int two = 1;
      while(n>0)  {
          two=one+two;
          one=two-one;
          n--;
      }
      return two;
    }
}
```
# 小结
爬楼梯是一个典型的一维动态规划问题
