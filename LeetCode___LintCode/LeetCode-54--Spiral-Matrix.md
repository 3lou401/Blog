> Given a matrix of *m* x *n* elements (*m* rows, *n* columns), return all elements of the matrix in spiral order.
For example,Given the following matrix:
[ [ 1, 2, 3 ], [ 4, 5, 6 ], [ 7, 8, 9 ]]
You should return [1,2,3,6,9,8,7,4,5].
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
给定一个包含 m x n 个要素的矩阵，（m 行, n 列），按照螺旋顺序，返回该矩阵中的所有要素。
**样例**
给定如下矩阵：
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
应返回 [1,2,3,6,9,8,7,4,5]。

# solution

## 方法一 traverse
利用遍历的方法和规则，设置遍历时，规则的变化，边界的改变，很直观，具体看代码
```
public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
    	List<Integer> res = new ArrayList<Integer>();
        
        if ( matrix == null || matrix.length == 0) {
            return res;
        }
        
        int rowBegin = 0;
        int rowEnd = matrix.length - 1;
        int colBegin = 0;
        int colEnd = matrix[0].length - 1;
        
        while( rowBegin <= rowEnd && colBegin <= colEnd) {
        	//traverse right
        	for(int i=colBegin;i<=colEnd;i++)
        		res.add(matrix[rowBegin][i]);
        	rowBegin++;
        	
        	//traverse down
        	for(int i=rowBegin;i<=rowEnd;i++)
        		res.add(matrix[i][colEnd]);
        	colEnd--;
        	
        	//traverse left
        	if(rowBegin <= rowEnd) {
	        	for(int i=colEnd;i>=colBegin;i--)
	        		res.add(matrix[rowEnd][i]);
	        	rowEnd--;
        	}
        	
        	//traverse up
        	if(colBegin <= colEnd) {
        		for(int i=rowEnd;i>=rowBegin;i--)
        			res.add(matrix[i][colBegin]);
        		colBegin++;
        	}
        }
        
        return res;
    }
}
```

## 方法二 direction
新建一个direction类，用它来控制坐标的移动，有两个函数，一个move，一个是turn。
 ```
public class Solution {
    /**
     * @param matrix a matrix of m x n elements
     * @return an integer list
     */
    public List<Integer> spiralOrder(int[][] matrix) {
        // Write your code here
        List<Integer> order = new ArrayList<Integer>();
        
        // check corner case
        if (matrix == null || matrix.length == 0) {
            return order;
        }
        int n = matrix.length;
        
        if (matrix[0] == null || matrix[0].length == 0) {
            return order;
        }
        int m = matrix[0].length;
        
        int direction = Direction.RIGHT;
        int[] cursor = new int[]{0, 0};
        
        for (int i = 0; i < n * m; i++) {
            order.add(matrix[cursor[0]][cursor[1]]);
            // mark the visited position as -1
            matrix[cursor[0]][cursor[1]] = -1;
            int[] nextCursor = Direction.move(cursor, direction);
            // if outside of matrix or marked before, turn right
            if (nextCursor[0] < 0 || nextCursor[0] >= n ||
                  nextCursor[1] < 0 || nextCursor[1] >= m ||
                  matrix[nextCursor[0]][nextCursor[1]] == -1) {
                direction = Direction.turnRight(direction);
                nextCursor = Direction.move(cursor, direction);
            }
            cursor = nextCursor;
        }
        
        return order;
    }
    
    
}

class Direction {
    public static int DOWN = 0;
    public static int RIGHT = 1;
    public static int LEFT = 2;
    public static int UP = 3;
    
    private static int[] dx = new int[]{1, 0, 0, -1};
    private static int[] dy = new int[]{0, 1, -1, 0};

    public static int turnRight(int direction) {
        if (direction == DOWN) {
            return LEFT;
        } else if (direction == RIGHT) {
            return DOWN;
        } else if (direction == LEFT) {
            return UP;
        } else if (direction == UP) {
            return RIGHT;
        }
        return -1;
    }
    
    public static int[] move(int[] cursor, int direction) {
        int[] nextCursor = new int[2];
        nextCursor[0] = cursor[0] + dx[direction];
        nextCursor[1] = cursor[1] + dy[direction];
        return nextCursor;
    }
}
```
