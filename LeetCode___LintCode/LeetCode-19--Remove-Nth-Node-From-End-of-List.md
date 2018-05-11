# 题目
给定一个链表，删除链表中倒数第n个节点，返回链表的头节点。

**样例**
给出链表**1->2->3->4->5->null**和 n = 2.
删除倒数第二个节点之后，这个链表将变成**1->2->3->5->null.**

# 分析
这里利用了一个技巧，倒数第n个节点。
先顺数n个节点，设置两个指针，这时候跟着遍历完，当第一个指针，到末尾时，第二根指针正好指向倒数第n个节点。

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
     * @param n: An integer.
     * @return: The head of linked list.
     */
    ListNode removeNthFromEnd(ListNode head, int n) {
        // write your code here
        if (n <= 0) {
            return null;
        }
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode preDelete = dummy;
        for (int i = 0; i < n; i++) {
            if (head == null) {
                return null;
            }
            head = head.next;
        }
        while (head != null) {
            head = head.next;
            preDelete = preDelete.next;
        }
        preDelete.next = preDelete.next.next;
        return dummy.next;
    }
}

```
