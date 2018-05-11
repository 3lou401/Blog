# 题目
设计一种方式检查一个链表是否为回文链表。

**样例**
1->2->1 就是一个回文链表。

# 分析
链表由于其特殊的结构，没法像数组那样判断回文，所以比较原始的方法，先找到链表的中间节点，然后将后半部分反转，然后逐个比较即可。
链表中的算法，通常以寻找链表中间节点，反转链表，合并两个链表这些基本操作构成，所以掌握这些基本操作很重要。
例如本题中寻找链表的中间节点的方法，就是用两个指针一快一慢，一个走两步，一个走一步，快指针先走到底了，这时候慢指针就指向中间节点。

# 代码
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
    /**
     * @param head a ListNode
     * @return a boolean
     */
     public boolean isPalindrome(ListNode head) {
        if (head == null) {
            return true;
        }
        
        ListNode middle = findMiddle(head);
        middle.next = reverse(middle.next);
        
        ListNode p1 = head, p2 = middle.next;
        while (p1 != null && p2 != null && p1.val == p2.val) {
            p1 = p1.next;
            p2 = p2.next;
        }
        
        return p2 == null;
    }
    
    private ListNode findMiddle(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode slow = head, fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return slow;
    }
    
    private ListNode reverse(ListNode head) {
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
