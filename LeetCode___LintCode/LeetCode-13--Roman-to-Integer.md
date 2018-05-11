> Given a roman numeral, convert it to an integer.
Input is guaranteed to be within the range from 1 to 3999.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
给定一个罗马数字，将其转换成整数。
返回的结果要求在1到3999的范围内。
**说明**什么是 *罗马数字*?
[https://en.wikipedia.org/wiki/Roman_numerals](https://en.wikipedia.org/wiki/Roman_numerals)
[https://zh.wikipedia.org/wiki/%E7%BD%97%E9%A9%AC%E6%95%B0%E5%AD%97](https://zh.wikipedia.org/wiki/%E7%BD%97%E9%A9%AC%E6%95%B0%E5%AD%97)
[http://baike.baidu.com/view/42061.htm](http://baike.baidu.com/view/42061.htm)
**样例**IV
 -> 4
XII
 -> 12
XXI
 -> 21
XCIX
 -> 99


# 分析
运用hashmap

# 代码
```
public class Solution {
    /**
     * @param s Roman representation
     * @return an integer
     */
    public int romanToInt(String s) {
        if (s == null || s.length()==0) {
                return 0;
	    }
	    Map<Character, Integer> m = new HashMap<Character, Integer>();
	    m.put('I', 1);
	    m.put('V', 5);
	    m.put('X', 10);
	    m.put('L', 50);
	    m.put('C', 100);
	    m.put('D', 500);
	    m.put('M', 1000);

	    int length = s.length();
	    int result = m.get(s.charAt(length - 1));
	    for (int i = length - 2; i >= 0; i--) {
	        if (m.get(s.charAt(i + 1)) <= m.get(s.charAt(i))) {
	            result += m.get(s.charAt(i));
	        } else {
	            result -= m.get(s.charAt(i));
	        }
	    }
	    return result;
    }
}
```
