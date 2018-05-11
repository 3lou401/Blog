> Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
For example, given n = 3, a solution set is:
![image.png](http://upload-images.jianshu.io/upload_images/1234352-53e711652e5a6bcb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
给定 n 对括号，请写一个函数以将其生成新的括号组合，并返回所有组合结果。

# 分析
深度优先搜索，关键是找限制条件，右括号的数量不能比左括号数量多。

# 代码
```
public class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList<>();
        if(n <= 0)
            return res;
        String parent = "";
        dfs(res, "",n , n);
        return res;
    }
    
    private void dfs(List<String> res, String parent, int left, int right) {
        
        if(left == 0 && right == 0) {
            res.add(parent);
            return;
        }
        
        if(left > 0) {
            dfs(res, parent+"(",left-1,right);
        }
        if(right > 0 && right > left) {
            dfs(res, parent + ")",left,right-1);
        }
    }
}
```
