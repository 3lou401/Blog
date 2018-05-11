# 题目
给一个不包含01的数字字符串，每个数字代表一个字母，请返回其所有可能的字母组合。

下图的手机按键图，就表示了每个数字可以代表的字母。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-f7b0bb715047a4a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 注意事项

以上的答案是按照词典编撰顺序进行输出的，不过，在做本题时，你也可以任意选择你喜欢的输出顺序。

样例
给定 "23"

返回 ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]

# 分析
深度搜索方法和回溯法

# 代码
```
public class Solution {
    /**
     * @param digits A digital string
     * @return all posible letter combinations
     */
    public ArrayList<String> letterCombinations(String digits) {
        ArrayList<String> res = new ArrayList<>();
        
        
        HashMap<Character, char[]> map = new HashMap<>();
        
        map.put('0', new char[] {});
        map.put('1', new char[] {});
        map.put('2', new char[] { 'a', 'b', 'c' });
        map.put('3', new char[] { 'd', 'e', 'f' });
        map.put('4', new char[] { 'g', 'h', 'i' });
        map.put('5', new char[] { 'j', 'k', 'l' });
        map.put('6', new char[] { 'm', 'n', 'o' });
        map.put('7', new char[] { 'p', 'q', 'r', 's' });
        map.put('8', new char[] { 't', 'u', 'v'});
        map.put('9', new char[] { 'w', 'x', 'y', 'z' });
        
        StringBuilder sb = new StringBuilder();
        
        dfs(res, digits, sb, map);
        
        return res;
    }

	private void dfs(ArrayList<String> res, String digits, StringBuilder sb, HashMap<Character, char[]> map) {
		if(digits.length() == sb.length())
		{
			res.add(sb.toString());
			return;
		}
		
		for(char c : map.get(digits.charAt(sb.length()))) {
			sb.append(c);
			dfs(res, digits, sb, map);
			sb.deleteCharAt(sb.length()-1);
		}
	}
}
```
