# 题目
给定一个整型数组，找到主元素，它在数组中的出现次数严格大于数组元素个数的1/k。
**注意事项**
数组中只有唯一的主元素

**样例**
给出数组 [3,1,2,3,2,3,3,4,4,4] ，和 k = 3，返回 3

# 分析
我们的基本做法如下：
    	首先，先要搞清楚这个问题在数学上的原理。既然主元素的判定标准是个数大于数组长度的1/k，那换句话说，就是满足如下的公式：
    	x/n > 1/k
    	其中，我假设x为主元素，n为数组长度。然后，将这个公式两边同时减1，并化简，得到下面这个式子：
    	(x - 1) / (n - k) > 1/ k
    	这个式子说明了一个很重要的问题：当主元素的个数减1后，如果整个数组的长度也减去k，是不会影响主元素的“地位”的。
于是，我们可以按照以下步骤设计算法：
1. 遍历数组，建立一个键为数组中元素，值为当前元素出现次数的hash表同时根据遍历结果，更新hash表
2. 当hash表中键值对的个数小于k时，继续步骤1，更新即可；而如果hash表中键值对的个数等于k，则对现在hash表中的所有键的值减1，也就是一共减去了k，这样，势必有至少一个键所对的值为0，我们从hash表中剔除值为0的键。需要注意的是，这样做的结果使得hash表的长度永远是小于k的。
3. 持续进行以上两步，直到所有数组元素全部被遍历完。
最后，对现在得到的这个hash表的值归0，然后再遍历一遍数组，统计现在hash表中的元素的个数，返回个数最多的那个元素，也就是主元素。
现在分析一下为什么这样做是对的，分两种情况讨论即可：
1. 数组中不同元素的个数小于k，那hash表的长度始终达不到k，主元素一定被保存在hash表的键中，最后遍历一遍数组之后，就会“浮出水面”
2. 数组中不同元素的个数大于或等于k，那又可分为两种情况：
(1) 当hash表中的键的个数达到k时，主元素恰好在hash表中，那么根据上面的公式知道，对所有键所对的值减1，不会影响主元素的“地位”
 (2) 当hash表中的键的个数达到k时，主元素不在hash表中，那么减去别的元素，也不影响主元素的“地位”，而且，最终，主元素一定会被保存在hash表中。

# 代码
```
public class Solution {
    /**
     * @param nums: A list of integers
     * @param k: As described
     * @return: The majority number
     */
    public int majorityNumber(ArrayList<Integer> nums, int k) {
        // write your code
    	
        HashMap<Integer, Integer> counters = new HashMap<>();
        
        for(Integer i:nums) {
        	if(!counters.containsKey(i)) {
        		counters.put(i, 1);
        	} else {
        		counters.put(i, counters.get(i)+1);
        	}
        	
        	if(counters.size()>=k)
        		removeKey(counters);
        }
        
        // corner case
        if (counters.size() == 0) {
            return Integer.MIN_VALUE;
        }
        
        // recalculate counters
        for(Integer i:counters.keySet()) {
        	counters.put(i, 0);
        }
        
        for(Integer i:nums) {
        	if(counters.containsKey(i))
        		counters.put(i, counters.get(i)+1);
        }
        
        // find the max key
        int maxCounter = 0, maxKey = 0;
        for(Integer key:counters.keySet()) {
        	if(counters.get(key) > maxCounter) {
        		maxCounter = counters.get(key);
        		maxKey = key;
        	}
        }
        
        return maxKey;
    }
    
    private void removeKey(HashMap<Integer, Integer> counters) {
        Set<Integer> keySet = counters.keySet();
        ArrayList<Integer> remove = new ArrayList<>();
        for(Integer key:keySet) {
        	counters.put(key, counters.get(key)-1);
        	if(counters.get(key) == 0) {
        		remove.add(key);
        	}
        }
        
        for(Integer i:remove) {
        	counters.remove(i);
        }
    }
}
```
