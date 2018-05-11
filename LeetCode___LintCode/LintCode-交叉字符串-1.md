# 题目
给出三个字符串:s1、s2、s3，判断s3是否由s1和s2交叉构成

**样例**
比如 s1 = "aabcc" s2 = "dbbca"
当 s3 = "aadbbcbcac"，返回  true.
当 s3 = "aadbbbaccc"， 返回 false.

# 分析
这道题可以用动态规划的思想解决。
dp[i][j]：表示s1的前i个字符和s2的前j个字符是否由交叉构成。
显然对于i+j个字符，要么等于s1的第i个，要么等于s2的第j个
* 当等于s1的第i个时，那么必须dp[i-1][j]是true,也就是前面i+j-2个字符是由交叉构成的，那么加进来的就为true
* 同理对于等于s2的第j个也是。
初始条件，当j=0时，那么s1与s3每个字符相等的话，就为true
同样当i=0时，也是一样！

# 代码
```
public class Solution {
    /**
     * Determine whether s3 is formed by interleaving of s1 and s2.
     * @param s1, s2, s3: As description.
     * @return: true or false.
     */
    public boolean isInterleave(String s1, String s2, String s3) {
        if(s1.length()+s2.length() != s3.length())
			 return false;
		 boolean[][] dp = new boolean[s1.length()+1][s2.length()+1];
		 dp[0][0] = true;
		 
		 for(int i=1;i<=s1.length();i++)
			 if(s3.charAt(i-1) == s1.charAt(i-1) && dp[i-1][0])
				 dp[i][0] = true;
		 
		 for(int i=1;i<=s2.length();i++)
			 if(s3.charAt(i-1) == s2.charAt(i-1) && dp[0][i-1])
				 dp[0][i] = true;
		 
		 for(int i=1;i<=s1.length();i++) {
			 for(int j=1;j<=s2.length();j++) {
				 if((s3.charAt(i+j-1) == s1.charAt(i-1) && dp[i-1][j]
						 )|| (s3.charAt(i+j-1) == s2.charAt(j-1) &&dp[i][j-1]))
					 dp[i][j] = true;
			 }
		 }
		 
		 return dp[s1.length()][s2.length()];
    }
}
```
