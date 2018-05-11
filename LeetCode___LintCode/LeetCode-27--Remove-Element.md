# 题目
给定一个数组和一个值，在原地删除与值相同的数字，返回新数组的长度。
元素的顺序可以改变，并且对新的数组不会有影响。

**样例**
给出一个数组**[0,4,4,0,0,2,4,4]**，和值 4返回 4 并且4个元素的新数组为**[0,0,0,2]**

# 分析
这类数组原地删除数据的题目，考察的就是两根指针的应用，注意掌握两根指针的思想，这一类问题就可以迎刃而解了。
我们设置两根指针slow和fast，fast每次都递增，所以称之为fast，当遍历到不是4的时候，slow指针就存储fast此时遍历的元素，slow加一，当遍历到需要删除的元素4时，略过slow，直接fast++，相当于删除了4.同时设置一个变量记录新数组的长度。

#代码
```
public class Solution {
    /** 
     *@param A: A list of integers
     *@param elem: An integer
     *@return: The new length after remove
     */
    public int removeElement(int[] A, int elem) {
        // write your code here
        int fast=0,slow=0;  
        int n=A.length;  
        int ret=0;  
          
        while(fast<n){  
            if(A[fast]!=elem){  
                ++ret;  
                A[slow]=A[fast];  
                ++slow;  
            }  
            ++fast;  
        }  
        return ret;
    }
}
```
