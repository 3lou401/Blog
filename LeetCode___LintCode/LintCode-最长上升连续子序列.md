# 题目
给定一个整数数组（下标从 0 到 n-1， n 表示整个数组的规模），请找出该数组中的最长上升连续子序列。（最长上升连续子序列可以定义为从右到左或从左到右的序列。）

# 样例
给定 [5, 4, 2, 1, 3], 其最长上升连续子序列（LICS）为 [5, 4, 2, 1], 返回 4.
给定 [5, 1, 2, 3, 4], 其最长上升连续子序列（LICS）为 [1, 2, 3, 4], 返回 4.

# 分析1（普通解法）
最简单的思路，遍历两遍，正向和反向各一遍，利用两个变量记录最长序列的长度。具体阅读代码，就可以知道。
```
public class Solution {
    /**
     * @param A an array of Integer
     * @return  an integer
     */
    public int longestIncreasingContinuousSubsequence(int[] A) {
        // Write your code here
        int max=1,count=1;
    	if(A.length == 0)
    		return A.length;
    	for(int i=1;i<A.length;i++)
    	{
    		while(i<A.length && A[i-1]<A[i])
    		{
    			i++;
    			count++;
    		}
    		if(count>max)
    			max=count;
    		else
    			count=1;
    		count=1;
    	}
    	
    	for(int i=A.length-1;i>0;i--)
    	{
    		while(i>0 && A[i-1]>A[i])
    		{
    			i--;
    			count++;
    		}
    		if(count>max)
    			max=count;
    		else
    			count=1;
    		count=1;
    	}
    	
    	return max;
    }
}
```
时间复杂度遍历了数组两次，较慢

# 分析2（使用队列）
引入队列可以是思路更清晰，而且我们只要遍历一遍就可以了
原理是：首先将第一个元素进队，然后循环将后面的元素进队，如果是递增的，就直接进队，直到碰到不是递增的，记录下此时队列的大小，队列的大小就是这个递增序列的长度，然后清空队列，继续进队，重复这个过程，最后留下来的就是最长递增序列的长度。

```
public class Solution {
    /**
     * @param A an array of Integer
     * @return  an integer
     */
    public int longestIncreasingContinuousSubsequence(int[] A) {
        // Write your code here
        if(A.length == 0)
            return 0;
        Queue<Integer> queue = new LinkedList<Integer>();
    	queue.offer(A[0]);
    	int max=1,count=1;
    	for(int i=1;i<A.length;i++)
    	{
    		if(A[i]<A[i-1])
    		{
    			count = queue.size();
    			if(count>max)
    				max = count;
    			queue.clear();
    		}
    		queue.offer(A[i]);
    	}
    	count = queue.size();
    	if(count>max)
			max = count;
    	
    	queue.clear();
    	
    	queue.offer(A[A.length-1]);
    	for(int i=A.length-1;i>0;i--)
    	{
    		if(A[i]>A[i-1])
    		{
    			count = queue.size();
    			if(count>max)
    				max = count;
    			queue.clear();
    		}
    		queue.offer(A[i]);
    	}
    	
    	count = queue.size();
    	if(count>max)
			max = count;
    	
    	return max;
    }
}
```
