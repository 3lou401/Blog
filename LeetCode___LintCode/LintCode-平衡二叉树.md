# 题目
给定一个二叉树,确定它是高度平衡的。对于这个问题,一棵高度平衡的二叉树的定义是：一棵二叉树中每个节点的两个子树的深度相差不会超过1。 

**样例**
给出二叉树 A={3,9,20,#,#,15,7} , B={3,#,20,15,7}


![balence.PNG](http://upload-images.jianshu.io/upload_images/1234352-838b74b714c1fc45.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

二叉树A是高度平衡的二叉树，但是B不是

# 分析
首先求深度，然后判断深度相差是否超过1，注意递归的使用

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
     * @return: True if this Binary tree is Balanced, or false.
     */
    public boolean isBalanced(TreeNode root) {
        // write your code here
        if(root == null)
            return true;
        int left = depth(root.left);
        int right = depth(root.right);
        if(Math.abs(left-right) <= 1)
        {
            return isBalanced(root.left) && isBalanced(root.right);
        }
        else
            return false;
    }
    
    public int depth(TreeNode root)
    {
        if(root == null)
        {
            return 0;
        }
        int left = depth(root.left);
        int right = depth(root.right);
        return Math.max(left,right) + 1;
    }
}
```
