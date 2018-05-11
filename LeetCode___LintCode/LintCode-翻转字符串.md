# 题目
给定一个字符串，逐个翻转字符串中的每个单词。

**说明**
单词的构成：无空格字母构成一个单词
输入字符串是否包括前导或者尾随空格？可以包括，但是反转后的字符不能包括
如何处理两个单词间的多个空格？在反转字符串中间空格减少到只含一个

**样例**
给出s = **"the sky is blue"**，返回**"blue is sky the"**

# 分析
这个较为简单，先直接用split函数将单词分割出来，然后存到list里，最后取出来依次添加并加上空格就行了。记得在末尾去掉最后的空格。
split函数是字符串处理中很重要的一个函数，需要熟练掌握


# 代码
```
public class Solution {
    /**
     * @param s : A string
     * @return : A string
     */
    public String reverseWords(String s) {
        // write your code
        if (s == null || s.length() == 0) {
            return "";
        }

        String[] array = s.split(" ");
        StringBuilder sb = new StringBuilder();

        for (int i = array.length - 1; i >= 0; --i) {
            if (!array[i].equals("")) {
                sb.append(array[i]).append(" ");
            }
        }

        //remove the last " "
        return sb.length() == 0 ? "" : sb.substring(0, sb.length() - 1);
    }
}

```
