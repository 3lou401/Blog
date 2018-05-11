# 题目
给出一棵二叉树，返回其节点值从底向上的层次序遍历（按从叶节点所在层到根节点所在的层遍历，然后逐层从左往右遍历）

样例
给出一棵二叉树 {3,9,20,#,#,15,7},

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-9d3d24fb036736b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

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
     * @return: buttom-up level order a list of lists of integer
     */
    public ArrayList<ArrayList<Integer>> levelOrderBottom(TreeNode root) {
        // write your code here
        if(root == null)
    		return new ArrayList<>();
    	
    	ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        
        Queue<TreeNode> list = new LinkedList<>();
        
        list.offer(root);
        
        while(!list.isEmpty()) {
        	int size = list.size();
        	ArrayList<Integer> level = new ArrayList<>();
        	for(int i=0;i<size;i++) {
	        	TreeNode cur = list.poll();
	        	level.add(cur.val);
	        	if(cur.left != null)
	        		list.offer(cur.left);
	        	if(cur.right != null)
	        		list.offer(cur.right);
        	}
        	res.add(level);
        }
        Collections.reverse(res);
        return res;
    }
}

```
