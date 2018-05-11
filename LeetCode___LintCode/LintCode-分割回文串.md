# 题目
给定一个字符串s，将s分割成一些子串，使每个子串都是回文串。

返回s所有可能的回文串分割方案。

样例
给出 s = "aab"，返回

[
  ["aa", "b"],
  ["a", "a", "b"]
]

# 分析
这是一道很综合的题目，结合了动态规划，深度搜索和回溯法。

首先为了分割出回文串，我们首先要写出判断回文串的方法，这里使用动态规划来判断回文串，而且可以存储子串的回文串的情况。
isPalindrome[i][j]:表示i到j的子串是否是回文串。
状态转移方程：
isPalindrome[i][j] = isPalindrome[i+1][j-1] && s.charAt(i) == s.charAt(j)
自然如果一个串是回文串，那么首尾必须要相等，并且中间也是子串。
初始化，显然当i==j的时候都是回文串
当串只有两个字符且相等的时候也是回文串。

知道如何判断子串是否是回文串就好办了，然后只要模式化的深度搜索回溯即可

# 代码
```
public class Solution {
    /**
     * @param s: A string
     * @return: A list of lists of string
     */
    
    List<List<String>> results;
    boolean[][] isPalindrome;
    
    /**
     * @param s: A string
     * @return: A list of lists of string
     */
    public List<List<String>> partition(String s) {
        results = new ArrayList<>();
        if (s == null || s.length() == 0) {
            return results;
        }
        
        getIsPalindrome(s);
        
        helper(s, 0, new ArrayList<Integer>());
        
        return results;
    }
    
    private void getIsPalindrome(String s) {
        int n = s.length();
        isPalindrome = new boolean[n][n];
        
        for (int i = 0; i < n; i++) {
            isPalindrome[i][i] = true;
        }
        for (int i = 0; i < n - 1; i++) {
            isPalindrome[i][i + 1] = (s.charAt(i) == s.charAt(i + 1));
        }
        
        for (int i = n - 3; i >= 0; i--) {
            for (int j = i + 2; j < n; j++) {
                isPalindrome[i][j] = isPalindrome[i + 1][j - 1] && s.charAt(i) == s.charAt(j);
            }
        }
    }
    
    private void helper(String s,
                        int startIndex,
                        List<Integer> partition) {
        if (startIndex == s.length()) {
            addResult(s, partition);
            return;
        }
        
        for (int i = startIndex; i < s.length(); i++) {
            if (!isPalindrome[startIndex][i]) {
                continue;
            }
            partition.add(i);
            helper(s, i + 1, partition);
            partition.remove(partition.size() - 1);
        }
    }
    
    private void addResult(String s, List<Integer> partition) {
        List<String> result = new ArrayList<>();
        int startIndex = 0;
        for (int i = 0; i < partition.size(); i++) {
            result.add(s.substring(startIndex, partition.get(i) + 1));
            startIndex = partition.get(i) + 1;
        }
        results.add(result);
    }
}
```
