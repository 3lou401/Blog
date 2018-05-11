# 题目
给出一棵二叉树，返回其节点值的锯齿形层次遍历（先从左往右，下一层再从右往左，层与层之间交替进行） 


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-37288c43ee38eecb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 分析
较为简单，直接在层次遍历的基础上改，将偶数行反转即可，可以加一个boolean变量判断，也可以最后直接在res上反转


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
     * @return: A list of lists of integer include 
     *          the zigzag level order traversal of its nodes' values 
     */
    public ArrayList<ArrayList<Integer>> zigzagLevelOrder(TreeNode root) {
        if(root == null)
    		return new ArrayList<>();
    	
    	ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        
        Queue<TreeNode> list = new LinkedList<>();
        
        list.offer(root);
        boolean normalorder = true;
        
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
        	
        	if(normalorder)
        		res.add(level);
        	else {
        		Collections.reverse(level);
        		res.add(level);
        	}
        	normalorder = !normalorder;
        }
        //Collections.reverse(res);
        
        
        return res;
    }
}
```
