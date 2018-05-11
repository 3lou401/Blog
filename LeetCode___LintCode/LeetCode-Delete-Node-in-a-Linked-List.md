# Question
> Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.
Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.

# 题目
给定一个单链表中的一个等待被删除的节点(非表头或表尾)。请在在O(1)时间复杂度删除该链表节点。

**样例**
给定1->2->3->4，和节点3，删除 3 之后，链表应该变为1->2->4。

# 分析
一般，删除链表节点需要前置节点，需要n的时间复杂度。但这里要实现1的复杂度。
看似很难，无法解决。其实很简单，我们转换思路，我们可以删除当前的节点的下一个节点，那么我们就删除下一个节点，不过，在删除之前，把下一个节点的值保存到当前节点就可以了，这样不久相当于删除了当前节点么？当然当头尾节点这样就行不通了。

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
     * @param node: the node in the list should be deleted
     * @return: nothing
     */
    public void deleteNode(ListNode node) {
        // write your code here
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```
