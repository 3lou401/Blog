# 题目
实现一个算法确定字符串中的字符是否均唯一出现

**样例**
给出"abc"，返回true

给出"aab"，返回false

**挑战**
如果不使用额外的存储空间，你的算法该如何改变？

# 分析
用两种方法，一种借助set没有重复元素的特点，如果add不进去，就说明重复了，就直接returnfalse
第二种方法，设立一个数组，用来判断字符出现的次数，第二次出现就判断为false


# 代码
```
public class Solution {
    /**
     * @param str: a string
     * @return: a boolean
     */
    public boolean isUnique(String str) {
        // write your code here
                Set set = new HashSet();
        for(int i=0;i<str.length();i++)
            if(set.add(str.charAt(i))==false)
                return false;
        return true;
    }
}
```

```
public class Solution {
    /**
     * @param str: a string
     * @return: a boolean
     */
    public boolean isUnique(String str) {
        // write your code here
        boolean[] char_set = new boolean[256];
        for (int i = 0; i < str.length(); i++) {
        int val = str.charAt(i);
        if (char_set[val]) return false;
            char_set[val] = true;
        }
        return true;
    }
}
```
