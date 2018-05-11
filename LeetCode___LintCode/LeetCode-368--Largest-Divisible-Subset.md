> Given a set of distinct positive integers, find the largest subset such that every pair (Si, Sj) of elements in this subset satisfies: Si % Sj = 0 or Sj % Si = 0.
If there are multiple solutions, return any subset is fine.
Example 1:
![image.png](http://upload-images.jianshu.io/upload_images/1234352-5183bb150a1f79c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Example 2:
![image.png](http://upload-images.jianshu.io/upload_images/1234352-ee7307aa5dd76c24.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 分析
这里需要发现一个规律，就是如果result中已经有【1，2】，那么对于要新加进来的数，只要它能整除掉result中最大的数就可以，因为如果先把result按大小排序，那么显然result中最大的数可以整除其他比他小的数，那么新加来的数都可以整除最大的数，自然也可以其他数。所以我们先将数组排序。然后用一个数组记录添加的元素，也就是类似记录路径，这种方法记录路径的方法很常用，类似于并查集中的应用。具体看代码

# 代码
```
public class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        int n = nums.length;
        int[] count = new int[n];
        int[] pre = new int[n];
        Arrays.sort(nums);
        int max = 0, index = -1;
        for (int i = 0; i < n; i++) {
            count[i] = 1;
            pre[i] = -1;
            for (int j = i - 1; j >= 0; j--) {
                if (nums[i] % nums[j] == 0) {
                    if (1 + count[j] > count[i]) {
                        count[i] = count[j] + 1;
                        pre[i] = j;
                    }
                }
            }
            if (count[i] > max) {
                max = count[i];
                index = i;
            }
        }
        List<Integer> res = new ArrayList<>();
        while (index != -1) {
            res.add(nums[index]);
            index = pre[index];
        }
        return res;
    }
}
```

