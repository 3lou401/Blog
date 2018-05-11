> Given an array of size *n*, find the majority element. The majority element is the element that appears **more than** ⌊ n/2 ⌋
 times.
You may assume that the array is non-empty and the majority element always exist in the array.
**Credits:**Special thanks to [@ts](https://oj.leetcode.com/discuss/user/ts) for adding this problem and creating all test cases.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.

# 题目
给定一个整型数组，找出主元素，它在数组中的出现次数严格大于数组元素个数的二分之一。

**样例**
给出数组**[1,1,1,1,2,2,2]**，返回 1

# 分析

## 方法一 voting，最重要的一个方法
较为简单，遍历一遍记录次数就可以了。
这种方法的思想是把 majority element 看成是 1，而把其他的元素看成是 -1。算法首先取第一个元素 x 作为 majority element，并计 mark = 1；而后遍历所有的元素，如果元素和 x 相等， 则 mark ++；否则如果不等， 则 mark--, 如果 mark == 0, 则重置 mark = 1， 并且更新 x 为当前元素。 由于majority element 的数量大于一半，所以最后剩下的必然是majority element.

# 代码
```
public class Solution {
    public int majorityElement(int[] nums) {
        int count = 0, candidate = -1;
        for (int i = 0; i < nums.length; i++) {
            if (count == 0) {
                candidate = nums[i];
                count = 1;
            } else if (candidate == nums[i]) {
                count++;
            } else {
                count--;
            }
        }
        return candidate;
    }
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-ae366e7d69c5345b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 方法二 排序法
最短的方法，两行代码搞定，因为主元素一定超过一半的元素，所以将数组排序，中间元素一定是主元素
```
// Sorting
public int majorityElement(int[] nums) {
    Arrays.sort(nums);
    return nums[nums.length/2];
}
```

## 方法三
利用hashMap，记录元素的值，和出现的次数
```
public int majorityElement2(int[] nums) {
    Map<Integer, Integer> myMap = new HashMap<Integer, Integer>();
    //Hashtable<Integer, Integer> myMap = new Hashtable<Integer, Integer>();
    int ret=0;
    for (int num: nums) {
        if (!myMap.containsKey(num))
            myMap.put(num, 1);
        else
            myMap.put(num, myMap.get(num)+1);
        if (myMap.get(num)>nums.length/2) {
            ret = num;
            break;
        }
    }
    return ret;
}
```
