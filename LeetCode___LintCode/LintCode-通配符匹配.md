判断两个可能包含通配符“？”和“*”的字符串是否匹配。匹配规则如下：

'?' 可以匹配任何单个字符。
'*' 可以匹配任意字符串（包括空字符串）。

两个串完全匹配才算匹配成功。

函数接口如下:
bool isMatch(const char *s, const char *p)

样例
一些例子：

isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false

# 分析

## 方法一：

动态规划：
match[i][j] : 表示从i到s.length,从j到p.length的是否匹配
状态转移方程：
如果 s[i]==p[j],
那么自然，match[i][j] = match[i+1][j+1];
如果 p[j] == '?'
自然，match[i][j] = match[i+1][j+1];
如果p[j] == '*'
分三种情况，* 只匹配s[i]
那么，match[i][j] = [i+1][j+1];
*作为空值出现
那么，macth[i][j] = match[i][j+1]
*匹配两个或者以上字符
那么,match[i][j] = match[i+1][j]

初始化：
如果p的后面有连续字符为*时，可以初始化为true。
```
public class Solution {
    /**
     * @param s: A string 
     * @param p: A string includes "?" and "*"
     * @return: A boolean
     */
    public boolean isMatch(String s, String p) {
       boolean[][] match=new boolean[s.length()+1][p.length()+1];
        match[s.length()][p.length()]=true;
        
        
        
        for(int i=p.length()-1;i>=0;i--){
            if(p.charAt(i)!='*')
                break;
            else
                match[s.length()][i]=true;
        }
        for(int i=s.length()-1;i>=0;i--){
            for(int j=p.length()-1;j>=0;j--){
                if(s.charAt(i)==p.charAt(j)||p.charAt(j)=='?')
                        match[i][j]=match[i+1][j+1];
                else if(p.charAt(j)=='*')
                        match[i][j]=match[i+1][j]||match[i][j+1] || match[i+1][j+1];
                else
                    match[i][j]=false;
            }
        }
        return match[0][0];
     }
}

/*aab  s 

a**b   p

m[i][j]  

m[2][3] = m[3][4]
m[2][2] = if * == b:m[3][3]
            if * == empty m[2][3]
            if * == 其他 m[3][2]
            
*/
```

## 方法二
两根指针遍历，记录*出现的位置
```

public class WildcardMatching {
	public boolean isMatch(String str, String pattern) {
	       int s = 0;
	       int p = 0;
	       int sMatch = -1;//*匹配s中的字符的终止位置
	       int starIdx = -1;//最近一个*出现的位置
	       while (s < str.length()){
	            // advancing both pointers
	            if (p < pattern.length()  && (pattern.charAt(p) == '?' || str.charAt(s) == pattern.charAt(p))){
	                s++;
	                p++;
	            }
	            // * found, only advancing pattern pointer，，先让* 作为empty出现，所以s不需要++
	            else if (p < pattern.length() && pattern.charAt(p) == '*'){
	                starIdx = p;//record the index of *
	                sMatch = s;//record the * 匹配的s中的位置
	                p++;
	            }
	           //last pattern pointer was *, advancing string pointer，用*去匹配当前的串
	            else if (starIdx != -1){
	                p = starIdx + 1;//只能用* 去匹配，所以p要回到*后面一个元素开始判断
	                sMatch++;//被*匹配了
	                s = sMatch;
	            }
	           //current pattern pointer is not star, last patter pointer was not *
	          //characters do not match
	            else return false;
	        }
	        
	        //check for remaining characters in pattern
	        while (p < pattern.length() && pattern.charAt(p) == '*')
	            p++;
	        
	        return p == pattern.length();
	     }
}

```
