> Given an integer *n*, generate a square matrix filled with elements from 1 to *n*2
 in spiral order.
For example,Given *n* = 3,
You should return the following matrix:[ [ 1, 2, 3 ], [ 8, 9, 4 ], [ 7, 6, 5 ]]
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
给你一个数n生成一个包含1-n^2的螺旋形矩阵
**样例**
n = 3
矩阵为
[
  [ 1, 2, 3 ],
  [ 8, 9, 4 ],
  [ 7, 6, 5 ]
]

# solution
相比问题一更简单，运用同样的规则遍历即可
```
public class Solution {
    public int[][] generateMatrix(int n) {
        if(n<0)
        	return null;
        int[][] res = new int[n][n];
        
        int count = 1;
        
        int rowBegin = 0;
        int rowEnd = n-1;
        int colBegin = 0;
        int colEnd = n-1;
        
        while(rowBegin <= rowEnd && colBegin <= colEnd) {
        	for(int i=colBegin;i<=colEnd;i++)
        		res[rowBegin][i] = count++;
        	rowBegin++;
        	
        	for(int i=rowBegin;i<=rowEnd;i++)
        		res[i][colEnd] = count++;
        	colEnd--;
        	
        	if(rowBegin <= rowEnd) {
        		for(int i=colEnd;i>=colBegin;i--)
        			res[rowEnd][i] = count++;
        		rowEnd--;
        	}
        	
        	if(colBegin <= colEnd) {
        		for(int i=rowEnd;i>=rowBegin;i--)
        			res[i][colBegin] = count++;
        		colBegin++;
        	}
        }
        return res;
    }
}
```
