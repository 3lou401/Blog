# 题目
实现二叉树遍历的中序非递归算法

# 代码

## 方法一：递归遍历
```
public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        
        Traversal1(root, res);
        return res;
    }
    
    private void Traversal1(TreeNode root, List<Integer> res) {
    	if(root == null)
    		return;
    	
    	Traversal1(root.left, res);
    	res.add(root.val);
    	Traversal1(root.right, res);
    }
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-31edb6ae982cbe82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 非递归的中序遍历

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
     * @return: Inorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        // write your code here
        Stack<TreeNode> stack = new Stack<TreeNode>();
        ArrayList<Integer> result = new ArrayList<Integer>();
        TreeNode curt = root;
        while (curt != null || !stack.empty()) {
            while (curt != null) {
                stack.add(curt);
                curt = curt.left;
            }
            curt = stack.peek();
            stack.pop();
            result.add(curt.val);
            curt = curt.right;
        }
        return result;
    }  
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-1bde53e1dcedf7aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 方法三：分治法
```
public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        
        if(root == null)
        	return res;
        
        List<Integer> left = inorderTraversal(root.left);
        List<Integer> right = inorderTraversal(root.right);
        
        res.addAll(left);
        res.add(root.val);
        res.addAll(right);
        
        return res;
    }
```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-4c9f53ee6801134d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
