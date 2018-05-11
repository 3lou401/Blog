>Total Accepted: 200641
Total Submissions: 798974
Difficulty: Medium
Contributor: LeetCode
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.
给出一个字符串（假设长度最长为1000），求出它的最长回文子串，你可以假定只有一个满足条件的最长回文串。
样例
给出字符串 "abcdzdcab"，它的最长回文子串为 "cdzdc"。

# 分析

## 方法一：最长公共子串
最容易想到的，先将字符串反转，然后找两个字符串的最大公共子串，就是最长回文子串
```
public class Solution {
    /**
     * @param s input string
     * @return the longest palindromic substring
     */
    public String longestCommonSubstring(String A, String B) {
        int n = A.length();
        int m = B.length();
        
        int[][] dp = new int[n+1][m+1];
        //dp[0][0] = 0;
        
        for(int i=1;i<=n;i++) {
        	for(int j=1;j<=m;j++) {
        		if(A.charAt(i-1) == B.charAt(j-1))
        			dp[i][j] = dp[i-1][j-1] + 1;
        		else
        			dp[i][j] = 0;
        	}
        }
        int x = 0,y = 0;
        int max = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if(dp[i][j] > max) {
                	max = dp[i][j];
                	x = i-1;
                	y = j-1;
                }
            }
        }
        
        StringBuilder sb =new StringBuilder();
        while(x >=0 && y>=0) {
        	if(A.charAt(x) == B.charAt(y))
        		{sb.append(A.charAt(x));
        		x--;
        		y--;}
        	else
        		break;
        }
        
        return sb.reverse().toString();
    }
	
	public String longestPalindrome(String s) {
		if(s.length() == 0)
			return "";
		
		String sr = reverseStr(s);
		
		return longestCommonSubstring(s, sr);
		
	}
	
	public String reverseStr(String s) {
		StringBuilder sb = new StringBuilder(s);
		sb.reverse();
		return sb.toString();
	}

}
```

##方法二：动态规划
判断一个字符串是不是回文串，可以用动态规划方法
dp[i][j]:表示i到j的字符串，是不是回文串，是就为true，不是就为false
那么当s[i] == s[j]的时候，dp[i][j] = dp[i+1][j-1],这是根据回文串的特点，比较容易理解的，比如我们知道bab是回文串，如果a = a,那么ababa也就是回文串！
所以我们可以用动态规划法求出最长回文串，然后也知道了他的起始位置，i和j，所以就比较容易求解了，唯一注意的问题是，我们得倒着求，而不能跟往常一样顺着求解，因为状态转移方程的依赖关系，是倒着依赖的。
```
public String longestPalindrome(String s) {
  int n = s.length();
  String res = null;
    
  boolean[][] dp = new boolean[n][n];
    
  for (int i = n - 1; i >= 0; i--) {
    for (int j = i; j < n; j++) {
      dp[i][j] = s.charAt(i) == s.charAt(j) && (j - i < 3 || dp[i + 1][j - 1]);
            
      if (dp[i][j] && (res == null || j - i + 1 > res.length())) {
        res = s.substring(i, j + 1);
      }
    }
  }
    
  return res;
}
```

## 方法三：两边扩展法
我们考虑对每个字符进行两边扩展，寻找回文串，并记录长度。有两种情况，一种是bab，从a向两边扩展，一种abba,从bb中间向两边扩展。
```
public String longestPalindrome(String s) {
    int start = 0, end = 0;
    for (int i = 0; i < s.length(); i++) {
        int len1 = expandAroundCenter(s, i, i);
        int len2 = expandAroundCenter(s, i, i + 1);
        int len = Math.max(len1, len2);
        if (len > end - start) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expandAroundCenter(String s, int left, int right) {
    int L = left, R = right;
    while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {
        L--;
        R++;
    }
    return R - L - 1;
}
```
