# 题目
给定一个整型数组，找到主元素，它在数组中的出现次数严格大于数组元素个数的三分之一。

** 注意事项**
数组中只有唯一的主元素

**样例**
给出数组[1,2,1,2,1,3,3] 返回 1

# 分析
三三抵销法，但是也有需要注意的地方：
我们对cnt1,cnt2减数时，相当于丢弃了3个数字（当前数字，candidate1, candidate2）。也就是说，每一次丢弃数字，我们是丢弃3个不同的数字。
而Majority number超过了1/3所以它最后一定会留下来。
而m > N/3, N - 3x > 0. 
3m > N,  N > 3x 所以 3m > 3x, m > x 也就是说 m一定没有被扔完
最坏的情况，Majority number每次都被扔掉了，但它一定会在n1,n2中。

# 代码
```
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: The majority number that occurs more than 1/3
     */
    public int majorityNumber(ArrayList<Integer> nums) {
        // write your code
        int candidate1 = 0, candidate2 = 0;
        int count1, count2;
        count1 = count2 = 0;
        
        for(int i=0;i<nums.size();i++) {
        	if(candidate1 == nums.get(i)) {
        		count1++;
        	} else if(candidate2 == nums.get(i)) {
        		count2++;
        	}else if(count1 == 0) {
        		candidate1 = nums.get(i);
        		count1 = 1;
        	}else if(count2 == 0) {
        		candidate2 = nums.get(i);
        		count2=1;
        	} else {
        		count1--;
        		count2--;
        	}
        }
        
        count1 = count2 = 0;
        for(int i=0;i<nums.size();i++) {
        	if(nums.get(i) == candidate1) {
        		count1++;
        	} else if(nums.get(i) == candidate2) {
        		count2++;
        	}
        }
        
        return count1>count2?candidate1	:candidate2;
    }
}

```
