# 题目
给一个排序数组（从小到大），将其转换为一棵高度最小的排序二叉树。

**样例**
给出数组[1,2,3,4,5,6,7], 返回

![sortTree.PNG](http://upload-images.jianshu.io/upload_images/1234352-edf4da81b7a0e203.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 分析
显然这个问题可以用递归解决。
中间的节点总是在根节点，所以我们不停的找到中间节点即可，然后分别递归处理左子树和右子树即可

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
     * @param A: an integer array
     * @return: a tree node
     */
    public TreeNode sortedArrayToBST(int[] A) {  
        // write your code here
        if(A == null) return null;  
        TreeNode root = null;  
        return newTreeNode (root, A, 0, A.length-1); 
    }
    
    public TreeNode newTreeNode (TreeNode node, int[] A, int start, int end) {  
        if (start > end) {return null; }  
        //node = new TreeNode(0);  
        int mid = end - (end - start)/2;  
        node.val = A[mid];  
        node.left = newTreeNode(node.left, A, start, mid - 1);  
        node.right = newTreeNode(node.right, A, mid + 1, end);  
        return node;  
    } 
}
```
