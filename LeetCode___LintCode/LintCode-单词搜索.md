# 题目
给出一个二维的字母板和一个单词，寻找字母板网格中是否存在这个单词。

单词可以由按顺序的相邻单元的字母组成，其中相邻单元指的是水平或者垂直方向相邻。每个单元中的字母最多只能使用一次。

样例
给出board =

[

  "ABCE",

  "SFCS",

  "ADEE"

]

word = "ABCCED"， ->返回 true,

word = "SEE"，-> 返回 true,

word = "ABCB"， -> 返回 false.

# 分析
深度搜索方法

# 代码
```
public class Solution {
    // recursion
    public boolean exist(char[][] board, String word) {
        if(board == null || board.length == 0)
            return false;
        if(word.length() == 0)
            return true;
        
        for(int i = 0; i< board.length; i++){
            for(int j=0; j< board[0].length; j++){
                if(board[i][j] == word.charAt(0)){
                    
                    boolean rst = find(board, i, j, word, 0);
                    if(rst)
                        return true;
                }
            }
        }
        return false;
    }
    
    private boolean find(char[][] board, int i, int j, String word, int start){
        if(start == word.length())
            return true;
        
        if (i < 0 || i>= board.length || 
     j < 0 || j >= board[0].length || board[i][j] != word.charAt(start)){
            return false;
	 }
        
        board[i][j] = '#'; // should remember to mark it
        boolean rst = find(board, i-1, j, word, start+1) 
|| find(board, i, j-1, word, start+1) 
|| find(board, i+1, j, word, start+1) 
|| find(board, i, j+1, word, start+1);
        board[i][j] = word.charAt(start);
        return rst;
    }
}
```
