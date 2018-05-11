# 题目
报数指的是，按照其中的整数的顺序进行报数，然后得到下一个数。如下所示：

1, 11, 21, 1211, 111221, ...

1 读作 "one 1" -> 11.

11 读作 "two 1s" -> 21.

21 读作 "one 2, then one 1" -> 1211.

给定一个整数 n, 返回 第 n 个顺序。 

** 注意事项
整数的顺序将表示为一个字符串。**

**样例**
给定 n =5, 返回"111221"

# 分析
看过去这道题，好像很复杂，其实仔细分析，会发现思路很清晰。不管算第几个数，我们都要从第一个数算起，每个数都要根据前一个数算出来。算数的过程就两个点，一个计算count，那个数出现的次数，一个这个数本身，所以两个临时变量每次都要这两个数就可以。

# 代码
```
public class Solution {
    /**
     * @param n the nth
     * @return the nth sequence
     */
    public String countAndSay(int n) {
        // Write your code here
        String oldString = "1";
        while (--n>0){
            StringBuilder sb = new StringBuilder();
            char[] oldChars = oldString.toCharArray();
            for(int i=0;i<oldChars.length;i++){
                int count = 1;
                while((i+1)<oldChars.length && oldChars[i]==oldChars[i+1]){
                    count++;
                    i++;
                }
                sb.append(String.valueOf(count) + String.valueOf(oldChars[i]));
            }
            oldString = sb.toString();
        }
        return oldString;
    }
}
```
