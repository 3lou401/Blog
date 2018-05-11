# 题目
有一个消息包含A-Z通过以下规则编码

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-a71d6883c0c2672d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在给你一个加密过后的消息，问有几种解码的方式

**样例**
给你的消息为12，有两种方式解码 AB(12) 或者 L(12). 所以返回 2

# 分析
这是典型的动态规划问题

dp[i]表示前i个字符的解码方式。那么考虑加进来的第i个字符，如果i个字符可以自己构成一个信息，也就第i个不等于0，那么dp[i] = dp[i-1],如果i和i-1合在一起还能表示一个信息，那么这时，dp[i] = dp[i-2]

所以考虑上面的情况，我们可以分析得出，当第i个字符不等于0的时候，最少dp[i] = dp[i-1],如果i和i-1还能构成一个信息，也就是在11-19到21-26之间的时候，自然dp[i] = dp[i-1]+ dp[i-2].

# 代码
```
public class Solution {
    /**
     * @param s a string,  encoded message
     * @return an integer, the number of ways decoding
     */
    public int numDecodings(String s) {
        // Write your code here
        if (s == null || s.length() == 0) {
            return 0;
        }
        int[] nums = new int[s.length() + 1];
        nums[0] = 1;
        nums[1] = s.charAt(0) != '0' ? 1 : 0;
        for (int i = 2; i <= s.length(); i++) {
            if(s.charAt(i-1) != '0') {
            	nums[i] = nums[i-1];
            }
            int twoDigits = (s.charAt(i - 2) - '0') * 10 + s.charAt(i - 1) - '0';
            if (twoDigits >= 10 && twoDigits <= 26) {
                nums[i] += nums[i - 2] ;
            }
            
            	
        }
        return nums[s.length()] ;
    }
}
```
