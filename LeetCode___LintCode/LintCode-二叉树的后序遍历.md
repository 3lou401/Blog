# 题目
实现二叉树后序遍历的非递归算法

# 代码

##方法一： 递归遍历
```
public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Traversal2(root, res);
        return res;
    }
    
    private void Traversal2(TreeNode root, List<Integer> res) {
    	if(root == null)
    		return;
    	
    	Traversal2(root.left, res);
    	Traversal2(root.right, res);
    	res.add(root.val);
    }
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-ea48fda16c884fd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


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
     * @return: Postorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> postorderTraversal(TreeNode root) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode prev = null; // previously traversed node
        TreeNode curr = root;

        if (root == null) {
            return result;
        }

        stack.push(root);
        while (!stack.empty()) {
            curr = stack.peek();
            if (prev == null || prev.left == curr || prev.right == curr) { // traverse down the tree
                if (curr.left != null) {
                    stack.push(curr.left);
                } else if (curr.right != null) {
                    stack.push(curr.right);
                }
            } else if (curr.left == prev) { // traverse up the tree from the left
                if (curr.right != null) {
                    stack.push(curr.right);
                }
            } else { // traverse up the tree from the right
                result.add(curr.val);
                stack.pop();
            }
            prev = curr;
        }

        return result;
    }
    
}
```

## 方法三：分治法
```
public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        
        if(root == null)
        	return res;
        
        List<Integer> left = postorderTraversal(root.left);
        //res.add(root.val);
        List<Integer> right = postorderTraversal(root.right);
        
        res.addAll(left);       
        res.addAll(right);
        res.add(root.val);
        
        return res;
    }
```


![Upload Paste_Image.png failed. Please try again.]
