# 题目
给一个整数数组，调整每个数的大小，使得相邻的两个数的差小于一个给定的整数target，调整每个数的代价为调整前后的差的绝对值，求调整代价之和最小是多少。

 **注意事项**
你可以假设数组中每个整数都是正整数，且小于等于100。

**样例**
对于数组[1, 4, 2, 3]和target=1，最小的调整方案是调整为[2, 3, 2, 3]，调整代价之和是2。返回2。

# 分析
动态规划，一个元素一个元素的调整，由于是要求相邻元素的差值，所以只和前一个相邻元素的值有关，所以只需要记录上一个调整的值就可以。
那么。dp[i][j]表示调整到第i个数时，此时，第i个数取值j,为代价和最小。
显然，假设dp[i-1][k]已知，则dp[i][j] = dp[i-1][k] + abs(j-A[i])
由于j和k都有多种可能取值，所以循环求解判断，k表示前一个数，j表示现在的数，假设j确定，那么k的取值就是在一个范围内，因为差值不能超过target。

# 代码
```
public class Solution {
    /**
     * cnblogs.com/beiyeqingteng/
     */
    public int MinAdjustmentCost(ArrayList<Integer> A, int target) {
        if (A == null || A.size() == 0) return 0;
        int max = getMax(A);
        int[][] costs = new int[A.size()][max + 1];
        
        for (int i = 0; i < costs.length; i++) {
            for (int j = 1; j <= max; j++) {
                costs[i][j] = Integer.MAX_VALUE;
                if (i == 0) {
                    // for the first number in the array, we assume it ranges from 1 to max;
                    costs[i][j] = Math.abs(j - A.get(i));
                } else {
                    // for the number A.get(i), if we change it to j, then the minimum total cost
                    // is decided by Math.abs(j - A.get(i)) + costs[i - 1][k], and the range of
                    // k is from Math.max(1, j - target) to Math.min(j + target, max)
                    for (int k = Math.max(1, j - target); k <= Math.min(j + target, max); k++) {
                        costs[i][j] = Math.min(costs[i][j], Math.abs(j - A.get(i)) + costs[i - 1][k]);
                    }
                }
            }
        }
        
        int min = Integer.MAX_VALUE;
        for (int i = 1; i < costs[0].length; i++) {
            min = Math.min(min, costs[costs.length - 1][i]);
        }
        return min;
    }
    
    private int getMax(ArrayList<Integer> A) {
        int max = A.get(0);
        for (int i = 1; i < A.size(); i++) {
            max = Math.max(max, A.get(i));
        }
        return max;
    }
}
```
