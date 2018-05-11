# 题目
将一棵二叉树按照前序遍历拆解成为一个假链表。所谓的假链表是说，用二叉树的 *right* 指针，来表示链表中的 *next* 指针。

** 注意事项 **
不要忘记将左儿子标记为 null，否则你可能会得到空间溢出或是时间溢出。

**样例**

![微信截图_20161125162727.png](http://upload-images.jianshu.io/upload_images/1234352-b502038116cafa24.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 分析
显然这个问题可以用递归处理
变成假链表的过程，就是将root的right指向左子树，左子树的尾节点指向右子树，最后再将左子树置空。
返回尾节点即可。

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
     * @param root: a TreeNode, the root of the binary tree
     * @return: nothing
     */
    public void flatten(TreeNode root) {
        // write your code here
        if(root==null)  
            return ;  
  
        ConvertToLink(root);
    
    }
    
    TreeNode ConvertToLink(TreeNode root) {
                if(root==null)  
        return null;  
  
    TreeNode leftLinkTail = ConvertToLink(root.left); // tail of left linked list  
    TreeNode rightLinkTail = ConvertToLink(root.right); // tail of left linked list  
      
    if(leftLinkTail != null)  
    {  
        leftLinkTail.right = root.right;  
        root.right = root.left;  
    }  
      
    root.left = null;  
  
    if(rightLinkTail!= null)  
        return rightLinkTail;  
    else if(leftLinkTail != null)  
        return leftLinkTail;  
    else  
        return root;  
    }
}
```
