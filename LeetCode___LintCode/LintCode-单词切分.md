# 题目
给出一个字符串s和一个词典，判断字符串s是否可以被空格切分成一个或多个出现在字典中的单词。

**样例**
给出
s = "lintcode"
dict = ["lint","code"]
返回 true 因为"lintcode"可以被空格切分成"lint code"

# 分析
这道题算动态规划里比较复杂的，对时间复杂度要求也比较高。笔者没加最大长度的判断语句，就直接Time Limit Exceeded了。
下面来分析具体的算法思路：
dp[i]：表示前i个字符能不能被完整的切分，要么为true,要么为false.
假设判断到了第i个字符，我们还要在内部用一个循环判断，从1到i 个字符，在哪个地方可以被切分，这个循环变量用j表示，那么dp[i]为true的条件是，dp[i-j]为true，且后面s.subString(i-j,i)这个字符串要在dict里面出现，只要满足这个条件，那么dp[i]才能为true。
这样的思路是不错的，但是结果却超时了，说明算法还有可以优化的地方，那么在哪里呢？
当我们在j循环的时候，显然需要小于dict里面最大长度

```
public class Solution {
    /**
     * @param s: A string s
     * @param dict: A dictionary of words dict
     */
    public boolean wordBreak(String s, Set<String> dict) {
        if (s == null || s.length() == 0) {
            return true;
        }

        int minLength = getMinLength(dict);
        int maxLength = getMaxLength(dict);
        //前i个字符能不能切分
        boolean[] canSegment = new boolean[s.length() + 1];

        canSegment[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            canSegment[i] = false;
            for (int j = 0;
                     j <=i-minLength  && j<=i;
                     j++) {
                         
                if(j < i-maxLength)
                    continue;
                
            	//如果前半部分不可切分，那不用看后半部分，直接进入下一个循环判断
                if (!canSegment[j]) {
                    continue;
                }
                //前半部分可分割，取出后半部分的字符串，进行遍历判断
                String word = s.substring(j, i);
                if (dict.contains(word)) {
                	//如果存在，则表明此时是可以分割的，直接跳出第二层循环
                    canSegment[i] = true;
                    break;
                }
            }
        }

        return canSegment[s.length()];
    }
    
    private int getMinLength(Set<String> dict) {
        int maxlenth = 0;
        for(String word : dict) {
        	maxlenth = Math.min(maxlenth, word.length());
        }
        return maxlenth;
    }
    
    private int getMaxLength(Set<String> dict) {
        int maxlenth = 0;
        for(String word : dict) {
        	maxlenth = Math.max(maxlenth, word.length());
        }
        return maxlenth;
    }
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-e9be75e6cb75d04e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们需要换一个想法，就是在j循环的时候倒过来

```
public boolean wordBreak(String s, Set<String> dict) {
        if (s == null || s.length() == 0) {
            return true;
        }

        int maxLength = getMaxLength(dict);
        //前i个字符能不能切分
        boolean[] canSegment = new boolean[s.length() + 1];

        canSegment[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            canSegment[i] = false;
            for (int lastWordLength = 1;
                     lastWordLength <= maxLength && lastWordLength <= i;
                     lastWordLength++) {
            	//如果前半部分不可切分，那不用看后半部分，直接进入下一个循环判断
                if (!canSegment[i - lastWordLength]) {
                    continue;
                }
                //前半部分可分割，取出后半部分的字符串，进行遍历判断
                String word = s.substring(i - lastWordLength, i);
                if (dict.contains(word)) {
                	//如果存在，则表明此时是可以分割的，直接跳出第二层循环
                    canSegment[i] = true;
                    break;
                }
            }
        }

        return canSegment[s.length()];
    }
    
    private int getMaxLength(Set<String> dict) {
        int maxlenth = 0;
        for(String word : dict) {
        	maxlenth = Math.max(maxlenth, word.length());
        }
        return maxlenth;
    }
```
