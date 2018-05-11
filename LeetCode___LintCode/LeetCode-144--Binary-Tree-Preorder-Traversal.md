Given a binary tree, return the *preorder* traversal of its nodes' values.
For example:Given binary tree {1,#,2,3}
,
1 \ 2 / 3

return [1,2,3]
.
**Note:** Recursive solution is trivial, could you do it iteratively?

[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.

# 题目
给出一棵二叉树，返回其节点值的前序遍历。

# 分析
前序排序的非递归算法是相对简单的，只要使用栈，保证right，left的顺序

# 代码

## 方法一，非递归，运用一个栈即可
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
     * @return: Preorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        // write your code here
        Stack<TreeNode> stack = new Stack<TreeNode>();
        ArrayList<Integer> preorder = new ArrayList<Integer>();
        
        if (root == null) {
            return preorder;
        }
        
        stack.push(root);
        while (!stack.empty()) {
            TreeNode node = stack.pop();
            preorder.add(node.val);
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }
        
        return preorder;
    }
    
}
```

## 方法二： 递归遍历
```
public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        Traversal(root,res);
        return res;
    }
    
    private void Traversal(TreeNode root, List<Integer> res) {
    	if(root == null)
    		return;
    	res.add(root.val);
    	Traversal(root.left,res);
    	Traversal(root.right,res);
    }
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-b03f9c3d80df52c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 方法三 ： 分治法
```
public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null)
        	return res;
        
        res.add(root.val);
        List<Integer> left = preorderTraversal(root.left);
        List<Integer> right = preorderTraversal(root.right);
        
        res.addAll(left);
        res.addAll(right);
        return res;
    }
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-b8504b4efe9119ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
