# 题目
给定一个排序数组和一个目标值，如果在数组中找到目标值则返回索引。如果没有，返回到它将会被按顺序插入的位置。
你可以假设在数组中无重复元素。

**样例**
**[1,3,5,6]**，5 → 2
**[1,3,5,6]**，2 → 1
**[1,3,5,6]**， 7 → 4
**[1,3,5,6]**，0 → 0

# 分析
显然这道题就是一道简单的查找题，采用二分查找的算法，最后在尝试给出返回值就可以了，这题较为简单

# 代码
```
public class Solution {
    /** 
     * param A : an integer sorted array
     * param target :  an integer to be inserted
     * return : an integer
     */
    public int searchInsert(int[] A, int target) {
        // write your code here
        if(A.length == 0)
    		return 0;
    	if(A == null)
    		return -1;
        int lo = 0;
        int hi = A.length - 1;
        int mid;
        while(lo <= hi)
        {
        	mid = (hi-lo)/2 + lo;
        	if(target < A[mid])
        		hi = mid-1;
        	else if(target > A[mid])
        		lo = mid+1;
        	else
        	    return mid;
        }
        return lo;
    }
}
```
