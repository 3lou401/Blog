# 题目：
查找斐波纳契数列中第 N 个数。
所谓的斐波纳契数列是指：
前2个数是 0 和 1 。
第 *i* 个数是第 *i*-1 个数和第*i*-2 个数的和。

斐波纳契数列的前10个数字是：
0, 1, 1, 2, 3, 5, 8, 13, 21, 34 ...

**样例**
给定1，返回0

给定2，返回1

给定10，返回34

# 分析
开始刷LintCode的第一道题，也是很基础的一道题。
斐波那契数列经常用来讲解递归的例子。
所以可以用递归的方法很方便的解决
## 递归
```
class Solution {
    /**
     * @param n: an integer
     * @return an integer f(n)
     */
    public int fibonacci(int n) {
        // write your code here
        if(n == 1)
        {
            return 0;
        }
        else if(n == 2)
        {
            return 1;
        }
        else
        {
            return fibonacci(n-1) + fibonacci(n-2); 
        }
    }
}
```
这是用递归的方法解决，很清晰，几乎是照搬了斐波那契数列的递推式
但是递归算法的时间复杂度太高，提交之后并不通过

![捕获.PNG](http://upload-images.jianshu.io/upload_images/1234352-0e8c2a7eb921765e.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 非递归方法
所以需要尝试非递归方法解决这个问题

```
class Solution {
    /**
     * @param n: an integer
     * @return an integer f(n)
     */
    public int fibonacci(int n) {
        // write your code here
        if(n == 1)
        {
            return 0;
        }
        else if(n == 2)
        {
            return 1;
        }
        else
        {
            int s1 = 0;
            int s2 = 1;
            int sum = 0;
            int i = 3;
            while(i <= n)
            {
                sum = s1 + s2;
                s1 = s2;
                s2 =sum;
                i++;
            }
            return sum;
        }
    }
}
```

# 小结
以上就是斐波那契数列问题。
** 故不积跬步，无以至千里；不积小流，无以成江海 **
希望能慢慢坚持下去，从简单到复杂的算法！
