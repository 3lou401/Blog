Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.


# 题目
给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的距离。

# 分析
最简单的求深度的代码

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
     * @return: An integer.
     */
    public int maxDepth(TreeNode root) {
        // write your code here
        if (root == null)  
        {  
            return 0;  
        }  
        int left = maxDepth(root.left);  
        int right = maxDepth(root.right);  
        return (left > right) ? left + 1 : right + 1;
    }
}
```

## 非递归的方法
使用有一个栈即可

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null)
		return 0;
	
	Deque<TreeNode> stack = new LinkedList<TreeNode>();
	
	stack.push(root);
	int count = 0;
	
	while (!stack.isEmpty()) {
		int size = stack.size();
		while (size-- > 0) {
			TreeNode cur = stack.pop();
			if (cur.left != null)
				stack.addLast(cur.left);
			if (cur.right != null)
				stack.addLast(cur.right);
		}
		count++;

	}
	return count;
    }
}
```
