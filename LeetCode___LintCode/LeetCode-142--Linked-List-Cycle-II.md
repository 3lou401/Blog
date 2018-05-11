> Given a linked list, return the node where the cycle begins. If there is no cycle, return null.
Note: Do not modify the linked list.
Follow up:
Can you solve it without using extra space?
# 题目
给定一个链表，如果链表中存在环，则返回到链表中环的起始节点的值，如果没有环，返回null。

# 分析
我们画图具体分析

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-e45dafcd7b9a315f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先，使用两个指针slow,fast。两个指针都从表头开始走，slow每次走一步，fast每次走两步，如果fast遇到null，则说明没有环，返回false；如果slow==fast，说明有环，并且此时fast超了slow一圈，返回true。

链表头是X，环的第一个节点是Y，slow和fast第一次的交点是Z。各段的长度分别是a,b,c，如图所示。环的长度是L。slow和fast的速度分别是qs,qf。

第一次相遇时slow走过的距离：a+b，fast走过的距离：a+b+c+b。

因为fast的速度是slow的两倍，所以fast走的距离是slow的两倍，有 2(a+b) = a+b+c+b，可以得到a=c（这个结论很重要！）。

我们发现L=b+c=a+b，也就是说，从一开始到二者第一次相遇，循环的次数就等于环的长度

我们已经得到了结论a=c，那么让两个指针分别从X和Z开始走，每次走一步，那么正好会在Y相遇！也就是环的第一个节点。

# 代码
```
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @return: The node where the cycle begins. 
     *           if there is no cycle, return null
     */
    public ListNode detectCycle(ListNode head) {  
        if(head == null || head.next == null)
    		return null;
    	
    	ListNode slow = head;
    	ListNode fast = head.next;
    	
    	while(slow != fast) {
    		if(fast == null || fast.next == null)
    			return null;
    		slow = slow.next;
    		fast = fast.next.next;
    	}
    	
    	slow = head;
    	fast = fast.next;
    	
    	while(slow!=fast) {
    		slow = slow.next;
    		fast =fast.next;
    	}
    	
    	return slow;
    
    }
}

```
