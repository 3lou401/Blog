> Given a linked list, determine if it has a cycle in it.
Follow up:Can you solve it without using extra space?
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
给定一个链表，判断它是否有环。

# Approach #1 (Two Pointers)
使用两个指针slow,fast。两个指针都从表头开始走，slow每次走一步，fast每次走两步，如果fast遇到null，则说明没有环，返回false；如果slow==fast，说明有环，并且此时fast超了slow一圈，返回true。

为什么有环的情况下二者一定会相遇呢？因为fast先进入环，在slow进入之后，如果把slow看作在前面，fast在后面每次循环都向slow靠近1，所以一定会相遇，而不会出现fast直接跳过slow的情况。

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
     * @return: True if it has a cycle, or false
     */
    public boolean hasCycle(ListNode head) {  
        // write your code here
        if (head == null || head.next == null) {
            return false;
        }

        ListNode slow = head;
        ListNode fast = head;

    while (true) {
        if (fast == null || fast.next == null) {
            return false;    //遇到null了，说明不存在环
        }
        slow = slow.next;
        fast = fast.next.next;
        if (fast == slow) {
            return true;   //第一次相遇在Z点
        }
    }
    }
}
```

# Approach #2 (Hash Table) 
想法很简单，如果链表中有一个环，那么我们只需要检查是否有一个节点被重复访问过，这个自然可以想到使用哈希表。
我们依次访问所有的链表节点，并将其节点的引用放到哈希表中，如果当前节点为空，就说明，我们已经到达了链表的末尾，而且这时候链表没有环，如果当前访问的节点在哈希表中出现过，也就是说明链表中有一个环。

```
public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null)
        	return false;
        
        Set<ListNode> nodeSeen = new HashSet<ListNode>();
        while(head != null) {
        	if(!nodeSeen.contains(head))
        		nodeSeen.add(head);
        	else
        		return true;
        	head = head.next;
        }
        return false;
    }
```
