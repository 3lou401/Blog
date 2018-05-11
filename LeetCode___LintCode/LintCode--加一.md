# 题目

给定一个非负数，表示一个数字数组，在该数的基础上+1，返回一个新的数组。

该数字按照大小进行排列，最大的数在列表的最前面。

**样例**
给定[1,2,3]
表示 123, 返回[1,2,4]
.
给定[9,9,9]
表示 999, 返回[1,0,0,0]

# 分析
思路很简单，首先先把数字数组转成字符串，然后把字符串转成int，然后对于int加一，然后再转换为字符串，最后变成数组

# 代码
```
public class Solution {
    /**
     * @param digits a number represented as an array of digits
     * @return the result
     */
    public int[] plusOne(int[] digits) {
        // Write your code here
        String s = "";
        for(int i=0; i<digits.length; i++){
            s += digits[i];
        }
        long m = Long.valueOf(s) + 1;
        String s1 = m + "";
        int[] ans = new int[s1.length()];
        for(int i=0; i<s1.length(); i++){
            ans[i] = (Integer.valueOf(s1.charAt(i) + ""));
        }
        return ans;
    }
}
```
