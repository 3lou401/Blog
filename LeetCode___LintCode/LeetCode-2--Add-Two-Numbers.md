> # 题目
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
You may assume the two numbers do not contain any leading zero, except the number 0 itself.
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
***
你有两个用链表代表的整数，其中每个节点包含一个数字。数字存储按照在原来整数中相反的顺序，使得第一个数字位于链表的开头。写出一个函数将两个整数相加，用链表形式返回和。
**样例**
给出两个链表3->1->5->null 和 5->9->2->null，返回8->0->8->null

# 分析
这道题类似之前的二进制求和，只不过换到了链表这种数据结构之上，同时二进制变成了十进制，要考虑好进位的计算。

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
    /**
     * @param l1: the first list
     * @param l2: the second list
     * @return: the sum list of l1 and l2 
     */
    public ListNode addLists(ListNode l1, ListNode l2) {
        // write your code here
        // write your code here
    	// save the pre node
        ListNode pre = new ListNode(0);
        // save the now node
        ListNode now = new ListNode(0);
        // create a null node to store the result
        ListNode result = null;
        int val = 0; //
        int add = 0; //jinwei
        
        while( l1 != null || l2 != null )
        {
        	val = ((l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val) + add) % 10;
        	add = ((l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val) + add) / 10;
        	l1 = (l1 == null ? null : l1.next);
        	l2 = (l2 == null ? null : l2.next);
        	now.val = val;
        	if(result == null)
        	{
        		result = now;
        	}
        	pre = now;
        	now = new ListNode(0);
        	pre.next = now;
        }
        //最后还要多来一次判断，因为有一种可能，两个链表一样长，最后一位又向上进了一位
        val = ((l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val) + add) % 10;
        add = ((l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val) + add) / 10;
        now.val = val;
        //如果最后一位又向上进了一位，新的最后一位不为0，应该保留，否则就为0，应当舍弃
        if(now.val == 0){
            pre.next = null;
        }
        return result;
    }
}
```
