# 题目
根据中序遍历和后序遍历树构造二叉树

 注意事项

你可以假设树中不存在相同数值的节点

样例
给出树的中序遍历： [1,2,3] 和后序遍历： [1,3,2]

返回如下的树：

  2

 /  \

1    3

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
     *@param inorder : A list of integers that inorder traversal of a tree
     *@param postorder : A list of integers that postorder traversal of a tree
     *@return : Root of a tree
     */
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if (inorder.length != postorder.length) {
            return null;
        }
        return myBuildTree(inorder, 0, inorder.length - 1, postorder, 0, postorder.length - 1);
    }

	private TreeNode myBuildTree(int[] inorder, int instart, int inend, int[] postorder, int poststart, int postend) {
		
		if(instart > inend)
			return null;
		
		TreeNode root =  new TreeNode(postorder[postend]);
		
		int position = findposition(inorder, root.val);
		
		
		root.left = myBuildTree(inorder, instart, position-1,postorder,poststart,poststart+position-instart-1);
		root.right = myBuildTree(inorder, position+1,inend, postorder, poststart+position-instart,postend-1);
		
		return root;
	}
	
	private int findposition(int[] inorder, int key) {
		for(int i=0;i<inorder.length;i++) {
			if(inorder[i] == key)
				return i;
		}
		return -1;
	}
}
```
