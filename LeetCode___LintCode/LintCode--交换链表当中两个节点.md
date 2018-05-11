# 题目
给你一个链表以及两个权值v1和v2，交换链表中权值为v1和v2的这两个节点。保证链表中节点权值各不相同，如果没有找到对应节点，那么什么也不用做。

 **注意事项**
你需要交换两个节点而不是改变节点的权值

**样例**
给出链表 1->2->3->4->null ，以及 v1 = 2 ， v2 = 4
返回结果 1->4->3->2->null。

# 分析
建立dummy结点，指向head（可能要对head进行操作）。
找到值为v1和v2的结点（设为n1，n2）的前结点p1, p2；
若p1和p2其中之一为null，则n1和n2其中之一也一定为null，返回头结点即可。
正式建立n1，n2，以及对应的next结点x1，x2，然后：
先分析n1和n2是相邻结点的两种情况：n1是n2的前结点，或n2是n1的前结点；
再分析非相邻结点的一般情况。
返回dummy.next，结束

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
     * @oaram v1 an integer
     * @param v2 an integer
     * @return a new head of singly-linked list
     */
    public ListNode swapNodes(ListNode head, int v1, int v2) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode node1Prev = null, node2Prev = null;
        ListNode cur = dummy;
        while (cur.next != null) {
            if (cur.next.val == v1) {
                node1Prev = cur;
            } else if (cur.next.val == v2) {
                node2Prev = cur;
            }
            
            if(node1Prev != null && node2Prev != null)
                break;
            cur = cur.next;
        }
        
        if (node1Prev == null || node2Prev == null) {
            return head;
        }
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode node1Prev = null, node2Prev = null;
        ListNode cur = dummy;
        while (cur.next != null) {
            if (cur.next.val == v1) {
                node1Prev = cur;
            } else if (cur.next.val == v2) {
                node2Prev = cur;
            }
            cur = cur.next;
        }
        
        if (node1Prev == null || node2Prev == null) {
            return head;
        }
        
        if (node2Prev.next == node1Prev) {
            // make sure node1Prev is before node2Prev
            ListNode t = node1Prev;
            node1Prev = node2Prev;
            node2Prev = t;
        }
        
        ListNode node1 = node1Prev.next;
        ListNode node2 = node2Prev.next;
        ListNode node2Next = node2.next;
        if (node1Prev.next == node2Prev) {
            node1Prev.next = node2;
            node2.next = node1;
            node1.next = node2Next;
        } else {
            node1Prev.next = node2;
            node2.next = node1.next;
            
            node2Prev.next = node1;
            node1.next = node2Next;
        }
        
        return dummy.next;
        
        ListNode node1 = node1Prev.next;
        ListNode node2 = node2Prev.next;
        ListNode node2Next = node2.next;
        if (node1Prev.next == node2Prev) {
            node1Prev.next = node2;
            node2.next = node1;
            node1.next = node2Next;
        } else {
            node1Prev.next = node2;
            node2.next = node1.next;
            
            node2Prev.next = node1;
            node1.next = node2Next;
        }
        
        return dummy.next;
    }
}
```
