# 题目
写出一个函数 anagram(s, t)
 判断两个字符串是否可以通过改变字母的顺序变成一样的字符串。

**样例**
给出 s = "abcd"，t="dcab"，返回 true
给出 s = "ab", t = "ab", 返回 true.
给出 s = "ab", t = "ac", 返回 false.

# 分析
这种题目只需判断是否具有相同的字符，以及字符的数量是否相等
技巧是新建一个数组用于记录，若有就加一，多个就继续加一
再在另一个字符串里减一，最后如果碰到小于0的，就说明字符数不相等或者没有

# 代码
```
public class Solution {
    /**
     * @param s: The first string
     * @param b: The second string
     * @return true or false
     */
    public boolean anagram(String s, String t) {
        // write your code here
        if(s.length() != t.length())
    		return false;
    	
    	int[] count = new int[256];
    	
    	for (int i = 0; i < s.length(); i++) {
    		count[(int) s.charAt(i)]++;
    	}
    	
    	for (int i = 0; i < t.length(); i++) {
    		count[(int) t.charAt(i)]--;
    		if(count[(int) t.charAt(i)] < 0)
    			return false;
    	}
    	
    	return true;
    }
};
```
