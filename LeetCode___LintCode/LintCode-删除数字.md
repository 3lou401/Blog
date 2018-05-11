# 题目
给出一个字符串 A, 表示一个 n 位正整数, 删除其中 k 位数字, 使得剩余的数字仍然按照原来的顺序排列产生一个新的正整数。

找到删除 k 个数字之后的最小正整数。

N <= 240, k <= N

样例
给出一个字符串代表的正整数 A 和一个整数 k, 其中 A = 178542, k = 4
返回一个字符串 "12"

# 分析
相当于删除一个数组中前k个大的数
剩下的数就可以返回.

如果后一个数大于前一个数，那么可以把后一个数删去

# 代码
```
public class Solution {
    /**
     *@param A: A positive integer which has N digits, A is a string.
     *@param k: Remove k digits.
     *@return: A string
     */
    public String DeleteDigits(String A, int k) {
        // write your code here
        StringBuffer sb = new StringBuffer(A);
		int i, j;
		for (i = 0; i < k; i++) {
			for (j = 0; j < sb.length() - 1
					&& sb.charAt(j) <= sb.charAt(j + 1); j++) {
			}
			sb.delete(j, j + 1);
		}
        while (sb.length() > 1 && sb.charAt(0)=='0')
            sb.delete(0,1);
		return sb.toString();
    }
}
```
