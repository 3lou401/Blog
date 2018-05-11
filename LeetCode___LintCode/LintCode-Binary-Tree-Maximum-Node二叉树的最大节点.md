Find the maximum node in a binary tree, return the node.

样例
给出如下一棵二叉树：

     1
   /   \
 -5     2
 / \   /  \
0   3 -4  -5 
返回值为 3 的节点。

# 分析
简单的递归思路，不过注意为空的情况，所以最好将为空的点的值设为最小值

# 代码
```
public class Solution {
    /**
     * @param root the root of binary tree
     * @return the max ndoe
     */
    public TreeNode maxNode(TreeNode root) {
        if(root == null)
            return null;
        
        return getMaxNode(root);
    }
    
    public TreeNode getMaxNode(TreeNode root) {
        if(root == null)
            return new TreeNode(Integer.MIN_VALUE);
        
        TreeNode left = getMaxNode(root.left);
        TreeNode right = getMaxNode(root.right);
        
        if(root.val >= right.val && root.val >= left.val)
        	return root;
        if(left.val >= right.val && left.val >= root.val)
			return left;
		return right;
    }    
}
```
