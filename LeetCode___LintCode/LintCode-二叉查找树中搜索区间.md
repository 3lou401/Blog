# 题目
给定两个值 k1 和 k2（k1 < k2）和一个二叉查找树的根节点。找到树中所有值在 k1 到 k2 范围内的节点。即打印所有x (k1 <= x <= k2) 其中 x 是二叉查找树的中的节点值。返回所有升序的节点值。

样例
如果有 k1 = 10 和 k2 = 22, 你的程序应该返回 [12, 20, 22].

    20
   /  \
  8   22
 / \
4   12

# 代码
```
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     * @param root: The root of the binary search tree.
     * @param k1 and k2: range k1 to k2.
     * @return: Return all keys that k1<=key<=k2 in ascending order.
     */
     

    public ArrayList<Integer> searchRange(TreeNode root, int k1, int k2) {
      ArrayList<Integer>   results = new ArrayList<Integer>();
        dfs(root, k1, k2,results);
        //Collections.reverse(results);
        return results;
    }
    
    private void dfs(TreeNode root, int k1, int k2, ArrayList<Integer> res) {
		
		if(root == null)
			return;
		
		if(root.val > k1) {
			dfs(root.left,k1,k2,res);
		}
		
		if(root.val >=k1 && root.val <=k2) {
			res.add(root.val);
		}
		
		if(root.val < k2)
			dfs(root.right,k1,k2,res);
		
	}
}
```
