# 题目
给定一个**旋转**排序数组，在原地恢复其排序。

**说明**
什么是旋转数组？
比如，原始数组为[1,2,3,4], 则其旋转数组可以是[1,2,3,4], [2,3,4,1], [3,4,1,2], [4,1,2,3]

**样例**
[4, 5, 1, 2, 3]->[1, 2, 3, 4, 5]

# 分析
这道题主要考察观察能力，显然旋转数组，不管怎么旋转，由于之前是排好序的，所以找到比第一个数小的第一个数一定是整个排序数组中最小的数。所以，只要找到最小的那个数，然后把前面的数全部按顺序加到后面，最后再将前面的数删除就可以了。

# 代码
```
public class Solution {
    /**
     * @param nums: The rotated sorted array
     * @return: void
     */
    public void recoverRotatedSortedArray(ArrayList<Integer> nums) {
        // write your code
        int temp = nums.get(0);
        int i;
        for( i = 0;i<nums.size();i++){
            if(nums.get(i)<temp)//找到最小的数
                break;
        }
        if(i!=nums.size()){
        for(int j=0 ; j<i;j++)
           nums.add(nums.get(j));
        nums.subList(0, i).clear();
        }
    }
}
```
