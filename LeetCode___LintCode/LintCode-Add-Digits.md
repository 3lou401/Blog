Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

样例
Given num = 38.
The process is like: 3 + 8 = 11, 1 + 1 = 2. Since 2 has only one digit, return 2.

# 分析

首先：12345 = 1 * 9999 + 2 * 999 + 3 * 99 + 4 * 9
+ 5 + (1+ 2+ 3 + 4 + 5)只要证明：12345 % 9 = (1 + 2 + 3 + 4 +5 ) % 9 就能往下递推了。那么，我们已知：m % 9 = a; n % 9 = b 即 m = 9 * x +
a; n = 9 * y + b；可推出
(m + n) % 9 = a + b = m % 9 + n % 9；[1 * 9999 + 2 * 999 + 3 * 99 + 4 * 9 + (1+
2+ 3 + 4 + 5)] % 9 = (1 * 9999) % 9 + (2 * 999) % 9 + (3 * 99) % 9 + (4 * 9) %
9 + (1+ 2+ 3 + 4 + 5) % 9 = 0 + 0 + 0 + 0 + (1 + 2 + 3 + 4 + 5) % 9 = (1 + 2 +
3 + 4 + 5) % 9。证明完成：12345 % 9 = (1 + 2 + 3 + 4 + 5) % 9 ;

# 代码
```
public class Solution {
    /**
     * @param num a non-negative integer
     * @return one digit
     */
    public int addDigits(int num) {
         return (num - 1) % 9 + 1;
    }
}
```
