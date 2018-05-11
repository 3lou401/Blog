#题目
给定一个字符串和一个偏移量，根据偏移量旋转字符串(从左向右旋转)

**样例**
对于字符串 "abcdefg".
offset=0 => "abcdefg"
offset=1 => "gabcdef"
offset=2 => "fgabcde"
offset=3 => "efgabcd"

# 分析
旋转字符串在原地旋转，利用了一个技巧，旋转三次可以达到效果，具体看代码分析即可。这种旋转的技巧需要熟练掌握

# 代码
```
public class Solution {
    /**
     * @param str: an array of char
     * @param offset: an integer
     * @return: nothing
     */
    public void rotateString(char[] str, int offset) {
        // write your code here
        if (str == null || str.length == 0)
            return;
            
        offset = offset % str.length;
        reverse(str, 0, str.length - offset - 1);
        reverse(str, str.length - offset, str.length - 1);
        reverse(str, 0, str.length - 1);
    }
    
    private void reverse(char[] str, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            char temp = str[i];
            str[i] = str[j];
            str[j] = temp;
        }
    }
}
```
