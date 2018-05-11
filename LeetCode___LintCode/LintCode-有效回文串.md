# 题目
给定一个字符串，判断其是否为一个回文串。只包含字母和数字，忽略大小写。
** 注意事项**
你是否考虑过，字符串有可能是空字符串？这是面试过程中，面试官常常会问的问题。
在这个题目中，我们将空字符串判定为有效回文。

**样例**
"A man, a plan, a canal: Panama"
 是一个回文。
"race a car"
 不是一个回文。

# 分析
需要判断的就是去除除字符和数字之外的干扰字符
去除之后，就可以用两根指针进行比较，遇到不相等的直接return false

# 代码
```
public class Solution {
    /**
     * @param s A string
     * @return Whether the string is a valid palindrome
     */
    public boolean isPalindrome(String s) {
        // Write your code here
        if(s == null || s.length() == 0)
    		return true;
    	
    	int front = 0;
    	int end = s.length() - 1;
    	
    	while(front < end)
    	{
    		while(front < s.length() && !isvalid(s.charAt(front)))
    			front++;
    			
    		if(front == s.length())
    		    return true;
    		
    		while(end > 0 && !isvalid(s.charAt(end)))
    			end--;
    		
    		if(Character.toLowerCase(s.charAt(front)) != Character.toLowerCase(s.charAt(end)))
    			return false;
    		else
    		{
    			front++;
    			end--;
    		}
    	}
    	
    	return true; 
    }

    private boolean isvalid (char c) {
        return Character.isLetter(c) || Character.isDigit(c);
    }
}
```
