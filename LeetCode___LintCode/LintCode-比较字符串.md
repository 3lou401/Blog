# 题目
比较两个字符串A和B，确定A中是否包含B中所有的字符。字符串A和B中的字符都是 **大写字母**
***
** 注意事项**
在 A 中出现的 B 字符串里的字符不需要连续或者有序。

**样例**
给出 A ="ABCD"  B ="ACD"，返回true
给出 A ="ABCD"  B ="AABC"， 返回false

# 分析
对于这类问题，我们可以用一个数组来记录A出现的次数和字母，对于B就去相减，对于=0的则是A中有，B中没有的。
** 利用一个中介帮助判断，是算法中常用的一种思想 **
# 代码
```
public class Solution {
    /**
     * @param A : A string includes Upper Case letters
     * @param B : A string includes Upper Case letter
     * @return :  if string A contains all of the characters in B return true else return false
     */
    public boolean compareStrings(String A, String B) {
        // write your code here
        char[] A1 = A.toCharArray();
        char[] B1 = B.toCharArray();
        
        if(B1.length > A1.length)
        	return false;
        	
        int[] letters = new int[26];
        
        for(int i=0;i<A1.length;i++) {
        	letters[A1[i]-'A']++;
        }
        
        for(int i=0;i<B1.length;i++)
        {
        	if(letters[B1[i]-'A']<=0)
        		return false;
        	else
        		letters[B1[i]-'A']--;
        }
        return true;
    }
}
```
