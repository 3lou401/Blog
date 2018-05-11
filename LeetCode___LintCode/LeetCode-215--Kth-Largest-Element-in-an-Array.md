> Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.
For example,
Given [3,2,1,5,6,4] and k = 2, return 5.
Note:
You may assume k is always valid, 1 ≤ k ≤ array's length.
在数组中找到第k大的元素
注意事项
你可以交换数组中的元素的位置
 样例
给出数组 [9,3,2,4,8]，第三大的元素是 4
给出数组 [1,2,3,4,5]，第一大的元素是 5，第二大的元素是 4，第三大的元素是 3，以此类推

# 分析
这个一个经典的寻找第k大数的问题，我们可以有很多种解法，下面我们一一介绍。

## 1直接排序
显然最简单的思想就是排序，然后取出倒数第k个元素就可以了，我们可以直接调用内部的排序函数。
```
public int findKthLargest(int[] nums, int k) {
        final int N = nums.length;
        Arrays.sort(nums);
        return nums[N - k];
}
```
** O(N lg N) running time + O(1) memory **

![图片.png](http://upload-images.jianshu.io/upload_images/1234352-22baac8b9e17fd21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在用例不多，数据量不大的时候，这种简单的方法，效率反而很高，超过大部分算法

## 2 利用堆
从第k大元素我们自然想到堆的性质，我们可以维护一个只有k个元素的最小堆，遍历一遍所有元素，组后留下的k个就是前k大的元素
```
public int findKthLargest(int[] nums, int k) {

    final PriorityQueue<Integer> pq = new PriorityQueue<>();
    for(int val : nums) {
        pq.offer(val);

        if(pq.size() > k) {
            pq.poll();
        }
    }
    return pq.peek();
}
```
** O(N lg K) running time + O(K) memory **

![图片.png](http://upload-images.jianshu.io/upload_images/1234352-3163a75bbac0288b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3 快排思想
利用快排的partiton思想，这种方法在最好情况下，可以达到线性时间，但是如果输入是有序的，则是最坏情况，会达到平方时间的复杂度
```
public int findKthLargest(int[] nums, int k) {

        k = nums.length - k;
        int lo = 0;
        int hi = nums.length - 1;
        while (lo < hi) {
            final int j = partition(nums, lo, hi);
            if(j < k) {
                lo = j + 1;
            } else if (j > k) {
                hi = j - 1;
            } else {
                break;
            }
        }
        return nums[k];
    }

    private int partition(int[] a, int lo, int hi) {

        int i = lo;
        int j = hi + 1;
        while(true) {
            while(i < hi && less(a[++i], a[lo]));
            while(j > lo && less(a[lo], a[--j]));
            if(i >= j) {
                break;
            }
            exch(a, i, j);
        }
        exch(a, lo, j);
        return j;
    }

    private void exch(int[] a, int i, int j) {
        final int tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
    }

    private boolean less(int v, int w) {
        return v < w;
    }
```

![图片.png](http://upload-images.jianshu.io/upload_images/1234352-f77251d07fc84a95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以看到由于最坏情况的存在，所以其实效率并不理想

## 4改进快排
我们考虑改进上面的快排算法，我们知道当数据有序时，会产生最坏情况，那么我们就随机化输入的数据，这样可以尽量避免最坏情况的发生。
```
public class Solution {
    public int findKthLargest(int[] nums, int k) {

        shuffle(nums);
        k = nums.length - k;
        int lo = 0;
        int hi = nums.length - 1;
        while (lo < hi) {
            final int j = partition(nums, lo, hi);
            if(j < k) {
                lo = j + 1;
            } else if (j > k) {
                hi = j - 1;
            } else {
                break;
            }
        }
        return nums[k];
    }

private void shuffle(int a[]) {

        final Random random = new Random();
        for(int ind = 1; ind < a.length; ind++) {
            final int r = random.nextInt(ind + 1);
            exch(a, ind, r);
        }
    }

    private int partition(int[] a, int lo, int hi) {

        int i = lo;
        int j = hi + 1;
        while(true) {
            while(i < hi && less(a[++i], a[lo]));
            while(j > lo && less(a[lo], a[--j]));
            if(i >= j) {
                break;
            }
            exch(a, i, j);
        }
        exch(a, lo, j);
        return j;
    }

    private void exch(int[] a, int i, int j) {
        final int tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
    }

    private boolean less(int v, int w) {
        return v < w;
    }
}
```

![](http://upload-images.jianshu.io/upload_images/1234352-579d8c4cdf0ff80b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到算法果然改进了不少
