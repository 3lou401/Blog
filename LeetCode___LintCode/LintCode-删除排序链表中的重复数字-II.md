# 题目
给定一个排序链表，删除所有重复的元素只留下原链表中没有重复的元素。
**样例**
给出 1->2->3->3->4->4->5->null，返回 1->2->5->null
给出 1->1->1->2->3->null，返回 2->3->null

# 分析
注意的一点就是，找到一个要删除的值时，用一个变量记录这个值，然后逐个全部删去值为这个值的节点

# 代码
```
/**
 * Definition for ListNode
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
    /**
     * @param ListNode head is the head of the linked list
     * @return: ListNode head of the linked list
     */
    public static ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null)
            return head;
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        head = dummy;

        while (head.next != null && head.next.next != null) {
            if (head.next.val == head.next.next.val) {
                int val = head.next.val;
                while (head.next != null && head.next.val == val) {
                    head.next = head.next.next;
                }            
            } else {
                head = head.next;
            }
        }
        
        return dummy.next;
    }
}

```
