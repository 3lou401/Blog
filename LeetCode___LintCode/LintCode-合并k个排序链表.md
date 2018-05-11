# 题目
合并k个排序链表，并且返回合并后的排序链表。尝试分析和描述其复杂度。

**样例**
给出3个排序链表[2->4->null,null,-1->null]，返回 -1->2->4->null

# 分析
按照前面实现的合并两个排序的链表的方法，两两合并，如果是奇数个，最后记得再合并最后一个即可。

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
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    public ListNode mergeKLists(List<ListNode> lists) {  
        // write your code here
    	if (lists == null || lists.size() == 0) {
            return null;
        }
    	
    	while(lists.size() > 1) {
    		List<ListNode> newLists = new ArrayList<ListNode>();
    		for(int i=0;i+1<lists.size();i=i+2) {
    			ListNode mergedList = mergeTwoLists(lists.get(i), lists.get(i+1));
    			newLists.add(mergedList);
    		}
    		
    		if(lists.size()%2 == 1) {
    			newLists.add(lists.get(lists.size()-1));
    		}
    		
    		lists = newLists;
    	}
    	
    	return lists.get(0);
    	
    }
    
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    	ListNode head = new ListNode(0);
    	ListNode point = head;
    	
    	while(l1!=null && l2!=null) {
    		if(l1.val<l2.val) {
    			point.next = l1;
    			l1 = l1.next;
    		}
    		else {
    			point.next = l2;
    			l2 = l2.next;
    		}
    		
    		point = point.next;
    	}
    	
    	if(l1!=null) {
    		point.next = l1;
    	}
    	
    	if(l2!=null) {
    		point.next = l2;
    	}
    	
    	return head.next;	
    }
}
```
