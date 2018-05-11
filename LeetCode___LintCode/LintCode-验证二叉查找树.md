# 题目
给定一个二叉树，判断它是否是合法的二叉查找树(BST)

一棵BST定义为：

* 节点的左子树中的值要严格小于该节点的值。
* 节点的右子树中的值要严格大于该节点的值。
* 左右子树也必须是二叉查找树。
* 一个节点的树也是二叉查找树。

样例
一个例子：

  2
 / \
1   4
   / \
  3   5
上述这棵二叉树序列化为 {2,1,4,#,#,3,5}.

# 分析
我们可以设置上下bound，递归左右子树时，为它们设置最大值，最小值，并且不可以超过。

注意：下一层递归时，需要把本层的up 或是down继续传递下去。相当巧妙的算法。

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
     * @return: True if the binary tree is BST, or false
     */

    public boolean isValidBST(TreeNode root) {
        // Just use the inOrder traversal to solve the problem.
        if (root == null) {
            return true;
        }
        
        return dfs(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    private boolean dfs(TreeNode root, long min, long max) {
		if(root == null)
			return true;
		
		if(root.val <= min || root.val >= max)
			return false;
		
		return dfs(root.left, min, root.val) && dfs(root.right, root.val, max);
	}
}
```
