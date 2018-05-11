# 题目
深度复制一个二叉树。
给定一个二叉树，返回一个他的 **克隆品** 。

**样例**
给定一个二叉树：

![copyTree2.PNG](http://upload-images.jianshu.io/upload_images/1234352-02ea3d35adbc7b03.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


返回其相同结构相同数值的克隆二叉树：

![copyTree2.PNG](http://upload-images.jianshu.io/upload_images/1234352-5a617430aa79dcde.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 分析
这题较简单，只要利用递归就行了。深度复制需要new出treeNode

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
     * @param root: The root of binary tree
     * @return root of new tree
     */
    public TreeNode cloneTree(TreeNode root) {
        // Write your code here
            if(root == null)  
        return null;  
  
    TreeNode dst = new TreeNode(0);  
    dst.val = root.val;  
    dst.left = cloneTree(root.left);  
    dst.right = cloneTree(root.right);  
  
    return dst;
    }
}
```
