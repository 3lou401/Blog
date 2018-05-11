# 题目
我们有一个栅栏，它有n个柱子，现在要给柱子染色，有k种颜色可以染。
必须保证任意两个相邻的柱子颜色不同，求有多少种染色方案。

** 原题应该表述有误，原文是“必须保证任意两个相邻的柱子颜色不同，应该表述为“不能有连续三个柱子颜色相同”。**

# 分析
这也是典型的动态规划问题，我们一样从最后的情况开始讨论。
假设buff[i]为有i个柱子时的染色方案。可以分为两种情况：
* 最后两根柱子颜色相同
* 最后两根柱子颜色不同
对于第一种情况，最后两根柱子颜色相同，不能三根柱子颜色连续相同，所以最后两根柱子的颜色选择有k-1种，所以buff[i-2]*k-1
对于第二种情况，最后两根柱子颜色不同，那么最后一根柱子的颜色有k-1种选择方案，所以buff[i-1]*k-1
所以综上，我们就得出状态转移方程为：
buff[i] = buff[i-1] * (k-1) + buff[i-2] * (k-1);
初始条件：
buff[1] = k; buff[2] = k*k;

# 代码
```
public class Solution {
    /**
     * @param n non-negative integer, n posts
     * @param k non-negative integer, k colors
     * @return an integer, the total number of ways
     */
    public int numWays(int n, int k) {
        // Write your code here
        if (n == 0)
            return 0;
        if (n == 1)
            return k;
        if (n == 2)
            return k*k;
        int pre = k;
        int now = k*k;
        for (int i = 3; i <= n; i++)
        {
            int tmp = now;
            now = (pre+now) * (k-1);
            pre = tmp;
        }
        return now;
    }
}
```
