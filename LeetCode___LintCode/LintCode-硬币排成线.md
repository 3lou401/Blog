# 题目
有 n 个硬币排成一条线。两个参赛者轮流从右边依次拿走 1 或 2 个硬币，直到没有硬币为止。拿到最后一枚硬币的人获胜。

请判定 第一个玩家 是输还是赢？

** 样例 **
n = 1, 返回 true.

n = 2, 返回 true.

n = 3, 返回 false.

n = 4, 返回 true.

n = 5, 返回 true.

# 分析
这道题是典型的博弈论，显然每次我们都是三个三个循环，如果我先拿，我拿一个，对方可以拿两个，我拿两个，对方可以拿一个，所以如果总数是三的倍数的话，肯定是对方获胜，所以答案就出来了，n不能是三的倍数

# 代码
```
public class Solution {
    /**
     * @param n: an integer
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int n) {
        // write your code here
        return n%3!=0; 
    }
}
```
