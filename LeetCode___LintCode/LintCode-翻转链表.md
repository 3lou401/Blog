# 题目
翻转一个链表

**样例**
给出一个链表**1->2->3->null**，这个翻转后的链表为**3->2->1->null**

# 分析
原地翻转链表，这道题对链表操作很有实践意义
设置两个指针，每次交换两个节点。

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
     * @param head: The head of linked list.
     * @return: The new head of reversed linked list.
     */
    public ListNode reverse(ListNode head) {
        // write your code here
        ListNode prev = null;
        while (head != null) {
            ListNode temp = head.next;
            head.next = prev;
            prev = head;
            head = temp;
        }
        return prev;
    }
}

```
