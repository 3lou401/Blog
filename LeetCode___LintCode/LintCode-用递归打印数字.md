# 题目
用递归的方法找到从1到最大的N位整数。

 注意事项

用下面这种方式去递归其实很容易：

recursion(i) {
    if i > largest number:
        return
    results.add(i)
    recursion(i + 1)
}
但是这种方式会耗费很多的递归空间，导致堆栈溢出。你能够用其他的方式来递归使得递归的深度最多只有 N 层么？

您在真实的面试中是否遇到过这个题？ Yes
样例
给出 N = 1, 返回[1,2,3,4,5,6,7,8,9].

给出 N = 2, 返回[1,2,3,4,5,6,7,8,9,10,11,...,99].

# 分析
理解为1到9，乘以n-1个10的循环

# 代码
```
public class Solution {
    /**
     * @param n: An integer.
     * return : An array storing 1 to the largest number with n digits.
     */
    public List<Integer> numbersByRecursion(int n) {
        // write your code here
        ArrayList<Integer> res = new ArrayList<>();
        num(n, 0, res);
        return res;
    }
    
    public void num(int n, int ans,ArrayList<Integer> res){
        
        if(n==0){
            if(ans>0){
                res.add(ans);
            }
            return;
        }
        
        int i;
        for(i=0; i<=9; i++){
            num(n-1, ans*10+i, res);
        }
        
    }
}
```
