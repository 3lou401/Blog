# 题目
给定一个链表，旋转链表，使得每个节点向右移动k个位置，其中k是一个非负数

**样例**
给出链表1->2->3->4->5->null和k=2
返回4->5->1->2->3->null

# 分析
关键是找到第一段和第二段的点，分割开来，最后再合在一起就行了，设置两个指针，假设第一段的长度是x,那么x+n会等于链表的长度，所以方法自然出来了类似于寻找倒数第n个节点。

# 代码
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
private int getLength(ListNode head) {
        int length = 0;
        while (head != null) {
            length ++;
            head = head.next;
        }
        return length;
    }
    
    public ListNode rotateRight(ListNode head, int n) {
        if (head == null) {
            return null;
        }
        
        int length = getLength(head);
        n = n % length;
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        head = dummy;
        
        ListNode tail = dummy;
        for (int i = 0; i < n; i++) {
            head = head.next;
        }
        
        while (head.next != null) {
            tail = tail.next;
            head = head.next;
        }
        
        head.next = dummy.next;
        dummy.next = tail.next;
        tail.next = null;
        return dummy.next;
    }
}
```
