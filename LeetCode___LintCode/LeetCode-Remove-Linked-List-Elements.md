Remove all elements from a linked list of integers that have value ***val***.
**Example*****Given:*** 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, ***val*** = 6***Return:*** 1 --> 2 --> 3 --> 4 --> 5
**Credits:**Special thanks to [@mithmatt](https://leetcode.com/discuss/user/mithmatt) for adding this problem and creating all test cases.

[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.


**描述：**

删除链表中等于给定值val的所有节点。

**样例：**

给出链表 1->2->3->3->4->5->3, 和 val = 3, 你需要返回删除3之后的链表：1->2->4->5。

** 分析： **

1.首先判断head是不是空，为空就直接返回null
2.然后从head.next开始循环遍历，删除相等于val的元素
3.最后判断head是否和val相等，若相等，head = head.next
（这里最后判断head是有原因的，因为head只是一个节点，只要判断一次，如果最先判断head就比较麻烦，因为如果等于val，head就要发生变化）
这里也体现出为什么设计链表的时候要空出一个头结点

** 代码 **：
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode removeElements(ListNode head, int val) {
        
        //边界条件判断，如果head为null,则直接返回null
        if(head == null)
        	return null;
        
        //操作链表时的技巧，为了方便返回结果，我们新建一个dummy节点作为一个头结点
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        //删除链表时的操作，最好加一个头结点，这样删除的操作就是：head.next = head.next.next
        head = dummy;
        
        //开始循环链表，结束的条件是到达最后一个节点，因为有一个头结点的存在，所以是head.next != null
        while(head.next != null) {
        	//如果head.next节点等于val，就是要被删除的节点
        	if(head.next.val == val)
        		head.next = head.next.next;
        	//否则继续判断下一个节点
        	else
        		head = head.next;
        }
        return dummy.next;
        
    }
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-138135b0d600ff5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在leetcode的讨论中，看到一个特别的方法，三行递归代码解决这个问题：
首先我们先看代码：
```
public ListNode removeElements(ListNode head, int val) {
        if (head == null) return null;
        head.next = removeElements(head.next, val);
        return head.val == val ? head.next : head;
}
```
思路比较新颖，但原理很简单，递归调用，如果当前节点等于val就删除，但是效率较低，但不失为拓展思路的好方法，或许链表的结构也利于使用递归？

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-b7a02d4552b825c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
