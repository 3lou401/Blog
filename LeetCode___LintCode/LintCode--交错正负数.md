# 题目
给出一个含有正整数和负整数的数组，重新排列成一个正负数交错的数组。

 注意事项

不需要保持正整数或者负整数原来的顺序。

样例
给出数组[-1, -2, -3, 4, 5, 6]，重新排序之后，变成[-1, 5, -2, 4, -3, 6]或者其他任何满足要求的答案

# 分析
最简单的思路显然是用两个数组记录正数和负数，最后在遍历一遍即可

要求原地完成的思路：
两根指针，首先判断正数多还是负数多，并把多的那一部分移到后半部分，最后两根指针分别递增二交换即可
具体思路看代码注释
# 代码
```
class Solution {
    /**
     * @param A: An integer array.
     * @return: void
     */
    public int[] rerange(int[] A) {
        // Check the input parameter.
        if(A == null || A.length < 3)
        	return A;
        
        int n = A.length;
        
        int countPositive = 0;//计算正数的个数
        
        
        
        // store the positive numbers index.
        int positiveIndex = 0;
        int pos = 1;
    	int neg = 0;
        for(int i=0;i<n;i++) {
                if(A[i] > 0) {
                // Put all the positive numbers at in the left part.
                	swap(A,positiveIndex++,i);
                	countPositive++;
                }
        }
        
        if(countPositive > n/2) {
        // If positive numbers are more than negative numbers,
        // Put the positive numbers at first.
        	pos = 0;
        	neg = 1;
            // Reverse the array.
        	
        	int left = 0;
        	int right = n-1;
        	while(left < right) {
        		swap(A,left,right);
        		left++;
        		right--;
        	}
        }
        
        while(pos < n && neg <n) {
        	while(pos<n && A[pos]>0)
        		pos +=2;
        	while(neg<n && A[neg]<0)
        		neg +=2;
        	if(neg >= n || pos>=n)
        	    break;
        	swap(A,pos,neg);
        }
        
        // Reorder the negative and the positive numbers.
       
            // Should move if it is in the range.
            
            // Should move if it is in the range.
       return A; 
   }
   
   public void swap(int[] A, int l, int r) {
       int temp = A[l];
       A[l] = A[r];
       A[r] = temp;
   }
}
```
