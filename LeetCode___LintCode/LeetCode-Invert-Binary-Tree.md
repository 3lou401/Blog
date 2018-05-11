Invert a binary tree.
4 / \ 2 7 / \ / \1 3 6 9
to4 / \ 7 2 / \ / \9 6 3 1
**Trivia:**This problem was inspired by [this original tweet](https://twitter.com/mxcl/status/608682016205344768) by [Max Howell](https://twitter.com/mxcl):Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so fuck off.

[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
翻转一棵二叉树

# 分析

## Approach #1 (Recursive) [Accepted]
递归交换左右子树即可
```
public TreeNode invertTree(TreeNode root) {
    if (root == null) {
        return null;
    }
    TreeNode right = invertTree(root.right);
    TreeNode left = invertTree(root.left);
    root.left = right;
    root.right = left;
    return root;
}
```

## Approach #2 (Iterative) [Accepted]
利用一个队列即可，就像是层次遍历，不过在里面多加入一个交换的操作
```
public TreeNode invertTree(TreeNode root) {
    if (root == null) return null;
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.add(root);
    while (!queue.isEmpty()) {
        TreeNode current = queue.poll();
        TreeNode temp = current.left;
        current.left = current.right;
        current.right = temp;
        if (current.left != null) queue.add(current.left);
        if (current.right != null) queue.add(current.right);
    }
    return root;
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-f7996bea9b1e4265.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
