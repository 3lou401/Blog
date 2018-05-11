# 题目
给定一棵二叉树，找到两个节点的最近公共父节点(LCA)。

最近公共祖先是两个节点的公共的祖先节点且具有最大深度。

 注意事项

假设给出的两个节点都在树中存在

样例
对于下面这棵二叉树

  4
 / \
3   7
   / \
  5   6
LCA(3, 5) = 4

LCA(5, 6) = 7

LCA(6, 7) = 7

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
     * @param root: The root of the binary search tree.
     * @param A and B: two nodes in a Binary.
     * @return: Return the least common ancestor(LCA) of the two nodes.
     */
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode node1, TreeNode node2) {
        // write your code here
        if (root == null || root == node1 || root == node2) {
            return root;
        }
        
        TreeNode left = lowestCommonAncestor(root.left, node1, node2);
        TreeNode right =  lowestCommonAncestor(root.right, node1, node2);
        
        if(left != null && right != null) {
        	return root;
        }
        else if(left!= null)
        	return left;
        else if(right != null)
        	return right;
        
        return null;
    }
}
```
