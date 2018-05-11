> Implement strStr().
Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.

# 题目
对于一个给定的 source 字符串和一个 target 字符串，你应该在 source 字符串中找出 target 字符串出现的第一个位置(从0开始)。如果不存在，则返回 -1。

**说明**
在面试中我是否需要实现KMP算法？
不需要，当这种问题出现在面试中时，面试官很可能只是想要测试一下你的基础应用能力。当然你需要先跟面试官确认清楚要怎么实现这个题。

**样例**
如果 source = "source"和 target = "target"，返回 -1。
如果 source = "abcdabcdefg"和 target = "bcd"，返回 1。

# 分析
不用kmp算法，用简单的二重循环判断。
首先第一重循环，source的第一位到减去target长度的位数加一（这样是为了处理两个串都是空的情况）
第二重循环，每次都和target的每一位进行比较是否相等，如果找到一位不相等的就直接跳出循环。
最后如果能全部循环完成，就说明这个串是相等的。这样就可以返回所在下标了，也就是i。
这种通过判断不等来判断相等的思想是算法中常用的，需要好好体会掌握

# 代码
```
public class Solution {
    public int strStr(String haystack, String needle) {
        if(haystack == null || needle == null)
            return -1;
        int hLen = haystack.length();
        int nLen = needle.length();
        if(hLen == 0 && nLen == 0)
            return 0;
        
        for(int i=0;i<hLen-nLen+1;i++) {
            int j=0;
            for(j=0;j<nLen;j++)
            if(haystack.charAt(i+j) != needle.charAt(j))
                break;
            if(j == nLen)
                return i;
        }
        return -1;
    }
}
```



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-122f0e1e84897172.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

