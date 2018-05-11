# 题目
给一个数组 *nums* 写一个函数将0
移动到数组的最后面，非零元素保持原数组的顺序

 ** 注意事项
1.必须在原数组上操作
2.最小化操作数 **

**样例**给出 *nums* =[0, 1, 0, 3, 12], 调用函数之后, *nums* =[1, 3, 12, 0, 0]

#分析
这类数组原地删除数据的题目，考察的就是两根指针的应用，注意掌握两根指针的思想，这一类问题就可以迎刃而解了。
我们设置两根指针slow和fast，fast每次都递增，所以称之为fast，当遍历到不是0的时候，slow指针就存储fast此时遍历的元素，slow加一，当遍历到需要删除的元素0(就相当于移动0到最后)时，略过slow，直接fast++，相当于删除了0.同时设置一个变量记录新数组的长度。
最后我们再遍历剩下的元素，并给他们赋值为0即可。

** 这道题可以看作是删除元素那道题的扩展应用**

＃代码
```
public class Solution {
    /**
     * @param nums an integer array
     * @return nothing, do this in-place
     */
    public void moveZeroes(int[] A) {
        // Write your code here
        int fast=0,slow=0;  
        int n=A.length;   
          
        while(fast<n){  
            if(A[fast]!=0){  
                A[slow]=A[fast];  
                ++slow;  
            }  
            ++fast;  
        }
        for(int i=slow;i<n;i++)
        	A[i] = 0;
    }
}
```
