# 题目
给出两个整数a和b, 求他们的和, 但不能使用+等数学运算符。
** 注意事项 **
** 你不需要从输入流读入数据，只需要根据aplusb的两个参数a和b，计算他们的和并返回就行。
**
**说明**
a和b都是32位整数么？
是的

我可以使用位运算符么？
当然可以

**样例**
如果a=1
并且b=2
，返回3

# 分析
这里主要是复习一下位操作符的运用

# 代码
```
class Solution {
    /*
     * param a: The first integer
     * param b: The second integer
     * return: The sum of a and b
     */
    public int aplusb(int a, int b) {
        // write your code here, try to do it without arithmetic operators.
        if(a==0)return b;  
        if(b==0)return a;  
        int x1 = a^b;  
        int x2 = (a&b)<<1;  
        return aplusb(x1,x2); 
    }
};
```
