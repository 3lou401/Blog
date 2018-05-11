> Given a string **S** and a string **T**, count the number of distinct subsequences of **T** in **S**.
A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE"
 is a subsequence of "ABCDE"
 while "AEC"
 is not).
Here is an example:**S** = "rabbbit"
, **T** = "rabbit"
Return 3.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
给出字符串S和字符串T，计算S的不同的子序列中T出现的个数。
子序列字符串是原始字符串通过删除一些(或零个)产生的一个新的字符串，并且对剩下的字符的相对位置没有影响。(比如，“ACE”是“ABCDE”的子序列字符串,而“AEC”不是)。
**样例**
给出S = "rabbbit", T = "rabbit"
返回 3

# 分析
利用动态规划去做，我们用dp[i][j]表示S与T的前i个字符与前j个字符的匹配子串个数。可以知道：
1）初始条件：T为空字符串时，S为任意字符串都能匹配一次，所以dp[i][0]=1；S为空字符串，T不为空时，不能匹配，所以dp[0][j](j>1)=0。
2）若S的第i个字符等于T的第j个字符时，我们有两种匹配的选择：其一，若S的i-1字符匹配T的j-1字符，我们可以选择S的i字符与T的j字符匹配；其二，若S的i-1字符子串已经能与T的j字符匹配，放弃S的i字符与T的j字符。因此这个情况下，dp[i][j]=dp[i-1][j-1]+dp[i-1][j]。
3）若S的第i个字符不等于T的第j个字符时，这时只有当S的i-1字符子串已经能与T的j字符匹配，该子串能够匹配。因此这个情况下，dp[i][j]=dp[i-1][j]。

# 代码
```
public class Solution {
    /**
     * @param S, T: Two string.
     * @return: Count the number of distinct subsequences
     */
    public int numDistinct(String S, String T) {
        // write your code here
        if (S == null || T == null) {
            return 0;
        }

        int[][] nums = new int[S.length() + 1][T.length() + 1];

        for (int i = 0; i <= S.length(); i++) {
            nums[i][0] = 1;
        }
        for (int i = 1; i <= S.length(); i++) {
            for (int j = 1; j <= T.length(); j++) {
                nums[i][j] = nums[i-1][j];
                if (S.charAt(i - 1) == T.charAt(j - 1)) {
                    nums[i][j] += nums[i - 1][j - 1];
                }
            }
        }
        return nums[S.length()][T.length()];
    }
}
```
