# 题目
给出一组候选数字(C)和目标数字(T),找到C中所有的组合，使找出的数字和为T。C中的数字可以无限制重复被选取。

例如,给出候选数组[2,3,6,7]和目标数字7，所求的解为：

[7]，

[2,2,3]

 注意事项

所有的数字(包括目标数字)均为正整数。
元素组合(a1, a2, … , ak)必须是非降序(ie, a1 ≤ a2 ≤ … ≤ ak)。
解集不能包含重复的组合。 

样例
给出候选数组[2,3,6,7]和目标数字7

返回 [[7],[2,2,3]]

# 分析
也是典型的深度搜索回溯问题，不过需要注意一些点，首先我们需要将数组排序，然后移除掉重复元素，然后由于一个元素可以重复出现，所以start应该是上一次的值。

# 代码
```
public class Solution {
    /**
     * @param candidates: A list of integers
     * @param target:An integer
     * @return: A list of lists of integers
     */
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> results = new ArrayList<>();
        if (candidates == null || candidates.length == 0) {
            return results;
        }
        
        int[] nums = removeDuplicates(candidates);
        
        dfs(nums, 0, new ArrayList<Integer>(), target,0, results);
        
        return results;
    }
    
    private int[] removeDuplicates(int[] candidates) {
        Arrays.sort(candidates);
        
        int index = 0;
        for (int i = 0; i < candidates.length; i++) {
            if (candidates[i] != candidates[index]) {
                candidates[++index] = candidates[i];
            }
        }
        
        int[] nums = new int[index + 1];
        for (int i = 0; i < index + 1; i++) {
            nums[i] = candidates[i];
        }
        
        return nums;
    }
    
    private void dfs(int[] nums,
                     int startIndex,
                     List<Integer> combination,
                     int target,
                     int sum,
                     List<List<Integer>> results) {
        if (sum == target) {
            results.add(new ArrayList<Integer>(combination));
            return;
        }
        
        for (int i = startIndex; i < nums.length; i++) {
            if (target < nums[i] + sum) {
                break;
            }
            combination.add(nums[i]);
            dfs(nums, i, combination, target,sum + nums[i], results);
            combination.remove(combination.size() - 1);
        }
    }
}
```
