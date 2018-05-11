# 题目
给出一棵二叉树，寻找一条路径使其路径和最大，路径可以在任一节点中开始和结束（路径和为两个节点之间所在路径上的节点权值之和）

**样例**
给出一棵二叉树：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-7229332bf21ebdc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
返回 6

# 分析
这道题关于二叉树的最大路径问题，显然需要用递归解决。
由于这道题中所求路径是可以通过根节点的，从任意节点开始和结束的最大路径和。
显然，通过根节点的最大路径=根节点的值，左子树的最大路径+右子树的最大路径
所以我们可以递归求解左右子树的最大路径，最后把他们加到一起就是整个树的最大路径。

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
     * @param root: The root of binary tree.
     * @return: An integer.
     */
     

public int maxPathSum(TreeNode root) {
	    if(root == null)
	    	return 0;
	    ArrayList<Integer> res = new ArrayList<>();
	    res.add(Integer.MIN_VALUE);
	    helper(root, res);
	    return res.get(0);
	}
	
	private int helper(TreeNode root, ArrayList<Integer> res) {
		if(root == null)
			return 0;
		int left = helper(root.left,res);
		int right = helper(root.right,res);
		
		int cur = root.val + (left>0?left:0) + (right>0?right:0);
		
		if(cur>res.get(0))
			res.set(0, cur);
		
		return root.val + Math.max(left, Math.max(right, 0));
	}
    
}
```
