> Given two integers *n* and *k*, return all possible combinations of *k* numbers out of 1 ... *n*.
For example,If *n* = 4 and *k* = 2, a solution is:
[ [2,4], [3,4], [2,3], [1,2], [1,3], [1,4],]
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
组给出两个整数n和k，返回从1......n中选出的k个数的组合。
样例
例如 n = 4 且 k = 2
返回的解为：
[[2,4],[3,4],[2,3],[1,2],[1,3],[1,4]]

# 分析
典型的深度搜索回溯问题

# 代码
```
public class Solution {
    /**
     * @param n: Given the range of numbers
     * @param k: Given the numbers of combinations
     * @return: All the combinations of k numbers out of 1..n
     */
    public ArrayList<ArrayList<Integer>> combine(int n, int k) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        
        ArrayList<Integer> list = new ArrayList<>();
        
        if(n < k)
        	return res;
        
        dfs(res, list, n, k, 1);
        
        return res;
    }

	private void dfs(ArrayList<ArrayList<Integer>> res, ArrayList<Integer> list, int n, int k, int start) {
		
		if(list.size() == k) {
			res.add(new ArrayList<>(list));
			return;
		}
		
		for(int i=start;i<=n;i++) {
			if(!list.contains(i))
				list.add(i);
			dfs(res, list, n, k, i+1);
			list.remove(list.size()-1);
		}
	}
}
```
