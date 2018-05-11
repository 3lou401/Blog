> Given an array with *n* objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.
Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.
**Note:**You are not suppose to use the library's sort function for this problem.
[click to show follow up.](https://leetcode.com/problems/sort-colors/#)
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
给定一个包含红，白，蓝且长度为 n 的数组，将数组元素进行分类使相同颜色的元素相邻，并按照红、白、蓝的顺序进行排序。
我们可以使用整数 0，1 和 2 分别代表红，白，蓝。
 注意事项
不能使用代码库中的排序函数来解决这个问题。
排序需要在原数组中进行。
样例
给你数组 [1, 0, 1, 2], 需要将该数组原地排序为 [0, 1, 1, 2]。

# 分析

## 方法一
一个相当直接的解决方案是使用计数排序扫描2遍的算法。

首先，迭代数组计算 0,1,2 出现的次数，然后依次用 0,1,2 出现的次数去覆盖数组。

代码
```
void sortColors(int A[]) {
    int n = A.length;
    int num0 = 0, num1 = 0, num2 = 0;
    
    for(int i = 0; i < n; i++) {
        if (A[i] == 0) ++num0;
        else if (A[i] == 1) ++num1;
        else if (A[i] == 2) ++num2;
    }
    
    for(int i = 0; i < num0; ++i) A[i] = 0;
    for(int i = 0; i < num1; ++i) A[num0+i] = 1;
    for(int i = 0; i < num2; ++i) A[num0+num1+i] = 2;
}
```

## 方法二
思路是，如果找到一个0，后面元素不用移动，因为0总是在前面，如果找到一个1，那么后面的1和2都要加一，如果找到2，那么2右移。

代码
```
// one pass in place solution
void sortColors(int A[]) {
    int n = A.length;
    int n0 = -1, n1 = -1, n2 = -1;
    for (int i = 0; i < n; ++i) {
        if (A[i] == 0) 
        {
            A[++n2] = 2; A[++n1] = 1; A[++n0] = 0;
        }
        else if (A[i] == 1) 
        {
            A[++n2] = 2; A[++n1] = 1;
        }
        else if (A[i] == 2) 
        {
            A[++n2] = 2;
        }
    }
}
```

## 方法三
两根指针记录0和2的边界位置，就是0的最右边，2的最左边，1在中间不用判断
```
public void sortColors(int[] a) {
        int n = a.length;
        int second=n-1, zero=0;
            for (int i=0; i<=second; i++) {
                while (a[i]==2 && i<second) swap(a,i, second--);
                while (a[i]==0 && i>zero) swap(a,i, zero++);
            }
    }
```
