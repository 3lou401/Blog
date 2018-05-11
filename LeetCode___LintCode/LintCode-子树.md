# 题目
有两个不同大小的二进制树： T1有上百万的节点； T2有好几百的节点。请设计一种算法，判定 T2是否为 T1的子树。

注意事项
若 T1 中存在从节点 n 开始的子树与 T2 相同，我们称 T2 是 T1 的子树。也就是说，如果在 T1 节点 n 处将树砍断，砍断的部分将与 T2 完全相同。

**样例**
下面的例子中 T2 是 T1 的子树：

![subtree.PNG](http://upload-images.jianshu.io/upload_images/1234352-211f6450a538957f.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 分析
这道题是判断两棵树是否相等的类似问题，判断子树是否相等，本质上还是归咎于判断树是否相等，递归求解左子树和右子树即可。


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
     * @param T1, T2: The roots of binary tree.
     * @return: True if T2 is a subtree of T1, or false.
     */
    public boolean isSubtree(TreeNode T1, TreeNode T2) {
        // write your code here
        if (T2 == null) {
            return true;
        }
        if (T1 == null) {
            return false;
        }
        
        if (isEqual(T1, T2)) {
            return true;
        }
        if (isSubtree(T1.left, T2) || isSubtree(T1.right, T2)) {
            return true;
        }
        return false;
        
    }
    
    private boolean isEqual(TreeNode T1, TreeNode T2) {
        if (T1 == null || T2 == null) {
            return T1 == T2;
        }
        if (T1.val != T2.val) {
            return false;
        }
        return isEqual(T1.left, T2.left) && isEqual(T1.right, T2.right);
    }
}
```
