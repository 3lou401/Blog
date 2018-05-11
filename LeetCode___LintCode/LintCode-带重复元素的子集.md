# 题目
给定一个可能具有重复数字的列表，返回其所有可能的子集
 **注意事项**
* 子集中的每个元素都是非降序的
* 两个子集间的顺序是无关紧要的
* 解集中不能包含重复子集

样例
如果 S = [1,2,2]，一个可能的答案为：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-3acd2c6de804ce5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 代码
```
class Solution {
    /**
     * @param nums: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    public ArrayList<ArrayList<Integer>> subsetsWithDup(int[] nums) {
        // write your code here
        ArrayList<ArrayList<Integer>> results = new ArrayList<>();
        if (nums == null) return results;
        
        if (nums.length == 0) {
            results.add(new ArrayList<Integer>());
            return results;
        }
        Arrays.sort(nums);

        ArrayList<Integer> subset = new ArrayList<>();
        helper(nums, 0, subset, results);
        
         return results;
        
        
    }
    public void helper(int[] nums, int startIndex, ArrayList<Integer> subset, ArrayList<ArrayList<Integer>> results){
        results.add(new ArrayList<Integer>(subset));
        for(int i=startIndex; i<nums.length; i++){
            if (i != startIndex && nums[i]==nums[i-1]) {
                continue;
            }
            subset.add(nums[i]);
            helper(nums, i+1, subset, results);
            subset.remove(subset.size()-1);
        }
    }
}

```
