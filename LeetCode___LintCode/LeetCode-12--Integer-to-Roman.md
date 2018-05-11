> Given an integer, convert it to a roman numeral.
Input is guaranteed to be within the range from 1 to 3999.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
***
# 题目
给定一个整数，将其转换成罗马数字。

返回的结果要求在1-3999的范围内。

**说明**什么是 *罗马数字*?
[https://en.wikipedia.org/wiki/Roman_numerals](https://en.wikipedia.org/wiki/Roman_numerals)
[https://zh.wikipedia.org/wiki/%E7%BD%97%E9%A9%AC%E6%95%B0%E5%AD%97](https://zh.wikipedia.org/wiki/%E7%BD%97%E9%A9%AC%E6%95%B0%E5%AD%97)
[http://baike.baidu.com/view/42061.htm](http://baike.baidu.com/view/42061.htm)

**样例**4
 -> IV

12
 -> XII

21
 -> XXI

99
 -> XCIX

更多案例，请戳 [http://literacy.kent.edu/Minigrants/Cinci/romanchart.htm](http://literacy.kent.edu/Minigrants/Cinci/romanchart.htm)


# 分析
循环判断即可

# 代码
```
public class Solution {
    /**
     * @param n The integer
     * @return Roman representation
     */
    public String intToRoman(int num) {
        if(num <= 0)
        	return "";
        
        String[] symbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        int[] nums = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        
        int digit = 0;
        StringBuilder sb = new StringBuilder();
        while(num > 0) {
        	int times = num/nums[digit];
        	num = num-times*nums[digit];
        	
        	for(;times>0;times--) {
        		sb.append(symbols[digit]);
        	}
        	digit++;
        }
        return sb.toString();
    }
}
```
