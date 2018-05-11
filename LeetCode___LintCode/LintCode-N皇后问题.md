# 题目
n皇后问题是将n个皇后放置在n*n的棋盘上，皇后彼此之间不能相互攻击。

给定一个整数n，返回所有不同的n皇后问题的解决方案。

每个解决方案包含一个明确的n皇后放置布局，其中“Q”和“.”分别表示一个女王和一个空位置。

**样例**
对于4皇后问题存在两种解决的方案：


    [".Q..", // Solution 1

     "...Q",

     "Q...",

     "..Q."],

    ["..Q.", // Solution 2

     "Q...",

     "...Q",

     ".Q.."]

# 分析
经典的问题，找出各个可行解。
首先我们判断怎样的位置是可行的，也就是不在同一行，不在同一列，且不在对角线上。
自然每个都在不同行上，所以我们可以把问题简化，在每一行找可以放进皇后的列。

一行一行的找遇到可行的列就加进去，当list的大小等于n的时候，就说明加满了，可以把结果存进去。


# 代码
```
class Solution {
    /**
     * Get all distinct N-Queen solutions
     * @param n: The number of queens
     * @return: All distinct solutions
     * For example, A string '...Q' shows a queen on forth position
     */
    ArrayList<ArrayList<String>> solveNQueens(int n) {
        ArrayList<ArrayList<String>> results = new ArrayList<>();
        if (n <= 0) {
            return results;
        }
        
        search(results, new ArrayList<Integer>(), n);
        return results;
    }
    
    /*
     * results store all of the chessboards
     * cols store the column indices for each row
     */
    private void search(ArrayList<ArrayList<String>> results,
                        ArrayList<Integer> cols,
                        int n) {
        if (cols.size() == n) {
            results.add(drawChessboard(cols));
            return;
        }
        
        for (int colIndex = 0; colIndex < n; colIndex++) {
            if (!isValid(cols, colIndex)) {
                continue;
            }
            cols.add(colIndex);
            search(results, cols, n);
            cols.remove(cols.size() - 1);
        }
    }
    
    private ArrayList<String> drawChessboard(ArrayList<Integer> cols) {
        ArrayList<String> chessboard = new ArrayList<>();
        for (int i = 0; i < cols.size(); i++) {
            StringBuilder sb = new StringBuilder();
            for (int j = 0; j < cols.size(); j++) {
                sb.append(j == cols.get(i) ? 'Q' : '.');
            }
            chessboard.add(sb.toString());
        }
        return chessboard;
    }
    
    private boolean isValid(ArrayList<Integer> cols, int column) {
        //cols存储了每一行Q所在的列
    	int row = cols.size();
    	
    	//不能在同一列，且不能为对象线上的元素
        for (int rowIndex = 0; rowIndex < row; rowIndex++) {
            if (cols.get(rowIndex) == column) {
                return false;
            }
            if (Math.abs(rowIndex - row) == Math.abs(column - cols.get(rowIndex))) {
                return false;
            }
            
        }
        return true;
    }
};
```
