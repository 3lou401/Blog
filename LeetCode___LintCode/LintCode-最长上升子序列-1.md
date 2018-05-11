# 题目
给定一个整数序列，找到最长上升子序列（LIS），返回LIS的长度。
**说明**最长上升子序列的定义：
最长上升子序列问题是在一个无序的给定序列中找到一个尽可能长的由低到高排列的子序列，这种子序列不一定是连续的或者唯一的。[https://en.wikipedia.org/wiki/Longest_increasing_subsequence](https://en.wikipedia.org/wiki/Longest_increasing_subsequence)

**样例**
给出 [5,4,1,2,3]，LIS 是 [1,2,3]，返回 3
给出 [4,2,4,5,3,7]，LIS 是 [2,4,5,7]，返回 4

# 分析

方法一：动态规划
用dp[i]表示前i个数的最长上升子序列，可以循环判断，每增加一个数，就要循环更新一遍前面的dp[]数组

![fang](http://upload-images.jianshu.io/upload_images/1234352-7e9ff49605d0b61f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
public class Solution {
    /**
     * @param nums: The integer array
     * @return: The length of LIS (longest increasing subsequence)
     */
    public int longestIncreasingSubsequence(int[] nums) {
        if(nums==null || nums.length<1) return 0;  
          
        int[] dp = new int[nums.length];
        dp[0] = 1;
        
        int max = dp[0];
        
        for(int i=1;i<nums.length;i++) {
        	dp[i] = 1;
        	for(int j=0;j<i;j++) {
        		if(nums[i] > nums[j])
        			dp[i] = Math.max(dp[i], dp[j]+1);
        	}
        	max = Math.max(max, dp[i]);
        }
        return max;
    }
}
```
方法二：
用一个list来存储元素：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-d11b14e4c44be729.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
public class Solution {
    /**
     * @param nums: The integer array
     * @return: The length of LIS (longest increasing subsequence)
     */
    public int longestIncreasingSubsequence(int[] nums) {
        if(nums==null || nums.length==0)
			return 0;
		ArrayList<Integer> list = new ArrayList<Integer>();
		for(int num: nums){
			if(list.size()==0){
				list.add(num);
			}else if(num>list.get(list.size()-1)){
				list.add(num);
			}else{
				int i=0;
				int j=list.size()-1;
				while(i<j){
					int mid = (i+j)/2;
					if(list.get(mid) < num){
						i=mid+1;
					}else{
						j=mid;
					}
				}
				list.set(j, num);
			}
		}
		return list.size();
    }
}

```
