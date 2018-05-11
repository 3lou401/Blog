# 题目
给出 n 个非负整数，代表一张X轴上每个区域宽度为 1 的海拔图, 计算这个海拔图最多能接住多少（面积）雨水。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-fd7c5c8ea35125d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

样例
如上图所示，海拔分别为 [0,1,0,2,1,0,1,3,2,1,2,1], 返回 6.

# 分析
有三种方法思路可以解决

## 方法一：
首先从左遍历一遍，找到已左边为基准的高度
再从右边遍历一遍找到以右边为基准的高度，
要能够接住水，就要求左右两边，最小的一边的高度应该比中间的都要大，能接住的水的面积就是这些差值。
看代码
```
public class Solution {
    /**
     * @param heights: an array of integers
     * @return: a integer
     */
    public int trapRainWater(int[] A) {
        if(A == null || A.length < 3)
        	return 0;
        
        int localMax = A[0];
        int[] left = new int[A.length];
        int[] right = new int[A.length];
        
        for(int i=0;i<A.length;i++) {
        	if(A[i] <= localMax)
        		left[i] = localMax;
        	else {
        		localMax = A[i];
        		left[i] = localMax;
        	}
        }
        
        localMax = A[A.length-1];
        for(int i=A.length-1;i>-1;i--) {
        	if(A[i] <= localMax)
        		right[i] = localMax;
        	else {
        		localMax = A[i];
        		right[i] = localMax;
        	}
        }
        
        int area = 0;
        for(int i=0;i<A.length;i++) {
        	area += Math.min(left[i], right[i]) - A[i];
        }
        
        return area;
    }
}
```

## 方法二
首先遍历一遍找到最高点，然后从左右两边分别向最高点进发
```
public class Solution {
    /**
     * @param heights: an array of integers
     * @return: a integer
     */
    public int trapRainWater(int[] heights) {
        if(heights.length <= 2) return 0;
        int max = -1, maxInd = 0;
        int i = 0;
        for(; i < heights.length; ++i){
            if(heights[i] > max){
                max = heights[i];
                maxInd = i;
            }
        }
        int area = 0, root = heights[0];
        for(i = 0; i < maxInd; ++i){
            if(root < heights[i]) root = heights[i];
            else area += (root - heights[i]);
        }
        for(i = heights.length-1, root = heights[heights.length-1]; i > maxInd; --i){
            if(root < heights[i]) root = heights[i];
            else area += (root - heights[i]);
        }
        return area;
    }
}
```

# 方法三
左右两个指针，一头一尾开始遍历，首先找到自己这一边的局部最高点，只要比最高点小就可以装水

```
public class Solution {
    /**
     * @param heights: an array of integers
     * @return: a integer
     */
    public int trapRainWater(int[] A) {
        if(A == null || A.length < 3)
        	return 0;
        
        //两根指针一头一尾向中间进发
        int left = 0;
        int right = A.length-1;
        //两个变量存储左右两边的局部最大高度
        int leftMax = 0;
        int rightMax = 0;
        
        int area = 0;
        
        while(left <= right) {
        	leftMax = Math.max(leftMax, A[left]);
        	rightMax = Math.max(rightMax, A[right]);
        	
        	//小的那边可以存水
        	if(leftMax <= rightMax) {
        		if(leftMax > A[left])
        			area += leftMax - A[left];
        		left++;
        	}
        	else {
        		if(rightMax > A[right])
        			area += rightMax - A[right];
        		right--;
        	}
        }
        
        return area;
    }
}
```
