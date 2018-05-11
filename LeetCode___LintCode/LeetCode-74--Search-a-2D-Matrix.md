> Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:
Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.
For example,
Consider the following matrix:
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
Given target = 3, return true.
# 题目
写出一个高效的算法来搜索 *m* × *n*矩阵中的值。
这个矩阵具有以下特性：
* 每行中的整数从左到右是排序的。
* 每行的第一个数大于上一行的最后一个整数。
**样例**
考虑下列矩阵：
```
[
  [1, 3, 5, 7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
```
给出 target = 3，返回 true

#  分析

本题主要考察的是二分法，但是这里如何使用二分法又有不同的策略，但本质都是一样的
方法一，我们二分，先从每列的最后一个数开始，确定它的列再搜索行
方法二，就是将二维数组展开一维数组，然后二分法中间的数
方法三，就是分别求行和列，求行时，用二分法，然后求列在用二分法。

# 代码

## 方法一
```
public class Solution {
    /**
     * @param matrix, a list of lists of integers
     * @param target, an integer
     * @return a boolean, indicate whether matrix contains target
     */
    public boolean searchMatrix(int[][] matrix, int target) {
        // write your code here
        if(matrix == null)
            return false;
        int rows=matrix.length;
        if(rows == 0 )
            return false;
        // 行数     
        int cols=matrix[0].length;  // 列数        
        int i=0, j=cols-1;    
        boolean found = false;    
        while(j>=0 && j<cols && i<rows)   //  i>=0 默认满足，无须再判断     
        {    
            if(matrix[i][j] == target) { found = true;  break; }                   
            else if(matrix[i][j] > target) j--;  // 如果矩阵右上角的值比target大，删除所在的列，列号-1     
            else i++;                           // 如果矩阵右上角的值不大于target，删除所在的行，行号+1    
        }                 
        return found;
        
    }
}
```

## 方法二
```
public class Solution {
    /**
     * @param matrix, a list of lists of integers
     * @param target, an integer
     * @return a boolean, indicate whether matrix contains target
     */
    public boolean searchMatrix(int[][] matrix, int target) {
        // write your code here
        if (matrix == null || matrix.length == 0) {
            return false;
        }
        if (matrix[0] == null || matrix[0].length == 0) {
            return false;
        }
        
        int row = matrix.length, column = matrix[0].length;
        int start = 0, end = row * column - 1;
        
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            int number = matrix[mid / column][mid % column];
            if (number == target) {
                return true;
            } else if (number < target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        
        if (matrix[start / column][start % column] == target) {
            return true;
        } else if (matrix[end / column][end % column] == target) {
            return true;
        }
        
        return false;
    }
}
```
## 方法三
```
public class Solution {
    /**
     * @param matrix, a list of lists of integers
     * @param target, an integer
     * @return a boolean, indicate whether matrix contains target
     */
    public boolean searchMatrix(int[][] matrix, int target) {
        // write your code here
        if (matrix == null || matrix.length == 0) {
            return false;
        }
        if (matrix[0] == null || matrix[0].length == 0) {
            return false;
        }
        
        int row = matrix.length;
        int column = matrix[0].length;
        
        // find the row index, the last number <= target 
        int start = 0, end = row - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (matrix[mid][0] == target) {
                return true;
            } else if (matrix[mid][0] < target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if (matrix[end][0] <= target) {
            row = end;
        } else if (matrix[start][0] <= target) {
            row = start;
        } else {
            return false;
        }
        
        // find the column index, the number equal to target
        start = 0;
        end = column - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (matrix[row][mid] == target) {
                return true;
            } else if (matrix[row][mid] < target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if (matrix[row][start] == target) {
            return true;
        } else if (matrix[row][end] == target) {
            return true;
        }
        return false;
    }
}

```
