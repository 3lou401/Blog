> Write a program to find the node at which the intersection of two singly linked lists begins.
For example, the following two linked lists:
A: a1 → a2 ↘ c1 → c2 → c3 ↗ B: b1 → b2 → b3
begin to intersect at node c1.
**Notes:**
If the two linked lists have no intersection at all, return null.
The linked lists must retain their original structure after the function returns.
You may assume there are no cycles anywhere in the entire linked structure.
Your code should preferably run in O(n) time and use only O(1) memory.
**Credits:**Special thanks to [@stellari](https://oj.leetcode.com/discuss/user/stellari) for adding this problem and creating all test cases.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
请写一个程序，找到两个单链表最开始的交叉节点。
** 注意事项 **
如果两个链表没有交叉，返回null。
在返回结果后，两个链表仍须保持原有的结构。
可假定整个链表结构中没有循环。
**样例**
下列两个链表：
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-fcc9d748496ad9a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 分析

这道题有两种解法
容易想到的第一种解法，先求出AB两个链表的长度，求出长度的差值，然后较长的那个从减掉长处的那部分开始，两个链表同时递增，遇到相同的节点既就是交叉节点。

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
     * @param headA: the first list
     * @param headB: the second list
     * @return: a ListNode 
     */
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) {
            return null;
        }
        
        int alen = getlen(headA);
        int blen = getlen(headB);
        
        if(alen > blen)
        	return helper(headA,headB,alen-blen);
        else
        	return helper(headB,headA,blen-alen);
    }

	private ListNode helper(ListNode headA, ListNode headB, int n) {
		while(n>0) {
			headA = headA.next;
			n--;
		}
		
		while(headA != headB) {
			headA = headA.next;
			headB = headB.next;
		}
		return headA;
	}

	private int getlen(ListNode head) {
		int len=0;
		while(head != null) {
			len++;
			head = head.next;
		}
		return len;
	}
}
```
第二种解法，则是利用带环链表的问题，我们将第一个表的尾与第二个链表的头相连，自然就成了带环链表，然后的问题就是求出带环链表的环起始节点了，这个可以参考问题带环链表II.由于题目要求不改变表结构，所以最后恢复表结构即可。
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
     * @param headA: the first list
     * @param headB: the second list
     * @return: a ListNode 
     */
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) {
            return null;
        }
        
        // get the tail of list A.
        ListNode node = headA;
        while (node.next != null) {
            node = node.next;
        }
        node.next = headB;
        ListNode result = listCycleII(headA);
        node.next = null;
        return result;
    }
    
    private ListNode listCycleII(ListNode head) {
        ListNode slow = head, fast = head.next;
        
        while (slow != fast) {
            if (fast == null || fast.next == null) {
                return null;
            }
            
            slow = slow.next;
            fast = fast.next.next;
        }
        
        slow = head;
        fast = fast.next;
        while (slow != fast) {
            slow = slow.next;
            fast = fast.next;
        }
        
        return slow;
    }
}
```

## 第三种解法哈希表
先将一个链表存到哈希表中，再遍历另一个，找到第一个存在于哈希表中的节点就是答案
```
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) {
            return null;
        }
        
        Set<ListNode> setA = new HashSet<>();
        while(headA != null) {
        	setA.add(headA);
        	headA = headA.next;
        }
        
        while(headB != null) {
        	if(setA.contains(headB))
        		return headB;
        	headB = headB.next;
        }
        return null;
    }
```
