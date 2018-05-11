# 题目
给定一个字符串， 包含大小写字母、空格' '
，请返回其最后一个单词的长度。
如果不存在最后一个单词，请返回 0
 。

**样例**
给定 s = "Hello World"，返回 5。

# 分析
两种方法，一个顺着找，一个倒着找。

# 代码
```
public class Solution {
    /**
     * @param s A string
     * @return the length of last word
     */
    public int lengthOfLastWord(String s) {
        // Write your code here
                int tLen = 0;  
        //int maxLen = 0;
        char[] sc = s.toCharArray();
        int lastLen = 0;  
        for (int i = 0; i < sc.length; i++)  
        {  
            if (sc[i] != ' ')  
            {  
                tLen++;  
                /*if (tLen > maxLen) 
                { 
                    maxLen = tLen; 
                }*/  
            } else  
            {  
                lastLen = tLen;  
                tLen = 0;  
            }  
        }  
        if (tLen != 0)  
        {  
            lastLen = tLen;  
        }  
        return lastLen;  
    }
}
```

```
public class Solution {
    /**
     * @param s A string
     * @return the length of last word
     */
    public int lengthOfLastWord(String s) {
        // Write your code here
                int tLen = 0;  
        //int maxLen = 0;
        int length = 0;
        char[] chars = s.toCharArray();
        for (int i = s.length() - 1; i >= 0; i--) {
            if (length == 0) {
                if (chars[i] == ' ') {
                    continue;
                } else {
                    length++;
                }
            } else {
                if (chars[i] == ' ') {
                    break;
                } else {
                    length++;
                }
            }
        }

        return length; 
    }
}
```
