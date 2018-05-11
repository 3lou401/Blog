# 题目
给一棵二叉树，找出从根节点到叶子节点的所有路径。

# 样例
给出下面这棵二叉树：

![binaryTree1.PNG](http://upload-images.jianshu.io/upload_images/1234352-9500ee518746e146.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所有根到叶子的路径为：

![binaryTree2.PNG](http://upload-images.jianshu.io/upload_images/1234352-0ca6b390db7ffa1e.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 分析
显然本道题可以使用递归算法。每天路径结束的条件的是遇到叶子节点，该树有多少个叶子节点就会有多少路径。
分别递归求解左子树和右子树。

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
     * @param root the root of the binary tree
     * @return all root-to-leaf paths
     */
    public List<String> binaryTreePaths(TreeNode root) {
        // Write your code here
        List result = new ArrayList<String>();
        if(root == null)
            return result;
        binaryTreePathsCore(root,result, root.val + "" );
        return result;
    }
    
    void binaryTreePathsCore(TreeNode root,List<String> str,String strpath)
    {
        if(root.left==null&&root.right==null)//叶子结点
        {
            str.add(strpath);
            return;
        }
        if(root.left!=null)
            binaryTreePathsCore(root.left,str,strpath+"->"+ root.left.val);
        if(root.right!=null)
            binaryTreePathsCore(root.right,str,strpath+"->"+ root.right.val);
    }
}
```
