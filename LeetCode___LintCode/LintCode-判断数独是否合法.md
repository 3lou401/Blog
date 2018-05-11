# 题目
请判定一个数独是否有效。
该数独可能只填充了部分数字，其中缺少的数字用 .表示。

注意事项
一个合法的数独（仅部分填充）并不一定是可解的。我们仅需使填充的空格有效即可。

**说明**
什么是 数独？
[http://sudoku.com.au/TheRules.aspx](http://sudoku.com.au/TheRules.aspx)
[http://baike.baidu.com/subview/961/10842669.htm](http://baike.baidu.com/subview/961/10842669.htm)

**样例**

![shudu.PNG](http://upload-images.jianshu.io/upload_images/1234352-19d85f62cd63f692.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面就是一个合法数独的样例

# 分析
初看上去题目似乎很复杂，其实不然。本题就是判断数组行列不能有重复元素，以及小九宫格不能有重复元素的算法。
首先，分别判断行，列，最后判断九宫格。

# 代码
```
class Solution {
    /**
      * @param board: the board
        @return: wether the Sudoku is valid
      */
    public boolean isValidSudoku(char[][] board) {
        
        boolean[] visited = new boolean[9];
        
        // row
        for(int i = 0; i<9; i++){
            Arrays.fill(visited, false);
            for(int j = 0; j<9; j++){
                if(!process(visited, board[i][j]))
                    return false;
            }
        }
        
        //col
        for(int i = 0; i<9; i++){
            Arrays.fill(visited, false);
            for(int j = 0; j<9; j++){
                if(!process(visited, board[j][i]))
                    return false;
            }
        }
        
        // sub matrix
        for(int i = 0; i<9; i+= 3){
            for(int j = 0; j<9; j+= 3){
                Arrays.fill(visited, false);
                for(int k = 0; k<9; k++){
                    if(!process(visited, board[i + k/3][ j + k%3]))
                    return false;                   
                }
            }
        }
        return true;
    }
    private boolean process(boolean[] visited, char digit){
        if(digit == '.'){
            return true;
        }
        
        int num = digit - '0';
        if ( num < 1 || num > 9 || visited[num-1]){
            return false;
        }
        
        visited[num-1] = true;
        return true;
    }
};
```
