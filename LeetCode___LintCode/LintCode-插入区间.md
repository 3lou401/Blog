# 题目
给出一个**无重叠的**按照区间起始端点排序的区间列表。
在列表中插入一个新的区间，你要确保列表中的区间仍然有序且**不重叠**
（如果有必要的话，可以合并区间）。

**样例**
插入区间**[2, 5]**到 **[[1,2], [5,9]]**，我们得到 **[[1,9]]**。
插入区间**[3, 4]**到 **[[1,2], [5,9]]**，我们得到** [[1,2], [3,4], [5,9]]**。

# 分析
总共插入就三种情况，一种插到中间
一种插到最前面
一种插到最后面

# 代码
```
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 */

class Solution {
    /**
     * Insert newInterval into intervals.
     * @param intervals: Sorted interval list.
     * @param newInterval: A new interval.
     * @return: A new sorted interval list.
     */
    public ArrayList<Interval> insert(ArrayList<Interval> intervals, Interval newInterval) {
        if (newInterval == null || intervals == null) {
            return intervals;
        }

        ArrayList<Interval> results = new ArrayList<Interval>();
        int insertPos = 0;

        for (Interval interval : intervals) {
            if (interval.end < newInterval.start) {
                results.add(interval);
                insertPos++;
            } else if (interval.start > newInterval.end) {
                results.add(interval);
            } else {
                newInterval.start = Math.min(interval.start, newInterval.start);
                newInterval.end = Math.max(interval.end, newInterval.end);
            }
        }

        results.add(insertPos, newInterval);

        return results;
    }
}
```
