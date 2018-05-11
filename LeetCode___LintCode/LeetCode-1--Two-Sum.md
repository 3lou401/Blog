> Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
**Example**:
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
给一个整数数组，找到两个数使得他们的和等于一个给定的数 target。
你需要实现的函数twoSum需要返回这两个数的下标, 并且第一个下标小于第二个下标。注意这里下标的范围是 1 到 n，不是以 0 开头。

# 分析
利用hashmap存储每个元素的值和所在的下标，遍历一遍，如果target-nums[i]在map里直接取出index值返回就可以了。leetcode 的第一道题。

# 代码
```
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++) {
        	if(map.containsKey(target-nums[i])) {
        		return new int[] {i, map.get(target-nums[i])};
        	}
        	else {
        		map.put(nums[i], i);
        	}
        }
        return new int[]{};
    }
}
```
