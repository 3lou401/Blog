> Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
For example, given *n* = 3, a solution set is:
[ "((()))", "(()())", "(())()", "()(())", "()()()"]
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
给定 n 对括号，请写一个函数以将其生成新的括号组合，并返回所有组合结果。
**样例**
给定 n = 3, 可生成的组合如下:
"((()))", "(()())", "(())()", "()(())", "()()()"

# 分析
针对一个长度为2n的合法排列，第1到2n个位置都满足如下规则：左括号的个数大于等于右括号的个数。所以，我们就可以按照这个规则去打印括号：假设在位置k我们还剩余left个左括号和right个右括号，如果left>0，则我们可以直接打印左括号，而不违背规则。能否打印右括号，我们还必须验证left和right的值是否满足规则，如果left>=right，则我们不能打印右括号，因为打印会违背合法排列的规则，否则可以打印右括号。如果left和right均为零，则说明我们已经完成一个合法排列，可以将其打印出来。通过深搜，我们可以很快地解决问题，针对n=2，问题的解空间如下

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-7f44b2018645d591.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 代码
```
public class Solution {
    /**
     * @param n n pairs
     * @return All combinations of well-formed parentheses
     */
     public ArrayList<String> generateParenthesis(int n) {
        ArrayList<String> result = new ArrayList<String>();
        if (n <= 0) {
            return result;
        }
        helper(result, "", n, n);
        return result;
    }
    
	public void helper(ArrayList<String> result,
	                   String paren, // current paren
	                   int left,     // how many left paren we need to add
	                   int right) {  // how many right paren we need to add
		if (left == 0 && right == 0) {
			result.add(paren);
			return;
		}

        if (left > 0) {
		    helper(result, paren + "(", left - 1, right);
        }
        
        if (right > 0 && left < right) {
		    helper(result, paren + ")", left, right - 1);
        }
	}
}
```
