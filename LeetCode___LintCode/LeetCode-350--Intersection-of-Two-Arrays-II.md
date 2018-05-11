> Given two arrays, write a function to compute their intersection.
**Example:**Given *nums1* = [1, 2, 2, 1]
, *nums2* = [2, 2]
, return [2, 2]
**Note:**
Each element in the result should appear as many times as it shows in both arrays.
The result can be in any order.
**Follow up:**
What if the given array is already sorted? How would you optimize your algorithm?
What if *nums1*'s size is small compared to *nums2*'s size? Which algorithm is better?
What if elements of *nums2* are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.

# 题目
计算两个数组的交

**样例**
nums1 = [1, 2, 2, 1], nums2 = [2, 2], 返回 [2, 2].

# 分析
这道题是上一道题的扩展，只是这次要记录重复的元素的个数，这次我们就用一个哈希表，键记录重复元素，值记录重复个数就行了。采用hashset的方法

# 代码
```
public class Solution {
    /**
     * @param nums1 an integer array
     * @param nums2 an integer array
     * @return an integer array
     */
    public int[] intersection(int[] nums1, int[] nums2) {
        // Write your code here
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for(int i = 0; i < nums1.length; ++i) {
            if (map.containsKey(nums1[i]))
                map.put(nums1[i], map.get(nums1[i]) + 1); 
            else
                map.put(nums1[i], 1);
        }

        List<Integer> results = new ArrayList<Integer>();

        for (int i = 0; i < nums2.length; ++i)
            if (map.containsKey(nums2[i]) &&
                map.get(nums2[i]) > 0) {
                results.add(nums2[i]);
                map.put(nums2[i], map.get(nums2[i]) - 1); 
            }

        int result[] = new int[results.size()];
        for(int i = 0; i < results.size(); ++i)
            result[i] = results.get(i);

        return result;
    }
}
```
