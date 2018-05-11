> Given *n* non-negative integers *a1
*, *a2
*, ..., *an
*, where each represents a point at coordinate (*i*, *ai
*). *n* vertical lines are drawn such that the two endpoints of line *i* is at (*i*, *ai
*) and (*i*, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.
Note: You may not slant the container and *n* is at least 2.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
给定 n 个非负整数 a1, a2, ..., an, 每个数代表了坐标中的一个点 (i, ai)。画 n 条垂直线，使得 i 垂直线的两个端点分别为(i, ai)和(i, 0)。找到两条线，使得其与 x 轴共同构成一个容器，以容纳最多水。
 注意事项
容器不可倾斜。
**样例**
给出[1,3,2], 最大的储水面积是2

# 分析
用两根指针一头一尾，分别计算面积，然后选取高度小的那边向中间移动，因为这样才可能存在更大的面积，更新最大的面积，最后就是结果。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-3d827221a33862ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 代码
```
public class Solution {
    /**
     * @param heights: an array of integers
     * @return: an integer
     */
    int computeArea(int left, int right,  int[] heights) {
        return (right-left)*Math.min(heights[left], heights[right]);
    }
    
    public int maxArea(int[] heights) {
        //两根指针，一头一尾计算，短的那根指针向中间移动，因为只有这种情况才存在更大的情况
    	int left = 0, right = heights.length-1;
    	int res = 0;
    	
    	while(left < right) {
    		res = Math.max(res, computeArea(left,right,heights));
    		if(heights[left] < heights[right]) {
    			left++;
    			//如果小于则不必要计算，直接跳过
    			while(heights[left] <= heights[left-1] && left<right)
    				left++;
    		}
    		else {
    			right--;
    			while(heights[right] <= heights[right+1] && left <right)
    				right--;
    		}
    	}
    	return res;	
    }
}
```
