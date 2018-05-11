# 题目
给定一个文档(Unix-style)的完全路径，请进行路径简化。

样例
"/home/", => "/home"

"/a/./b/../../c/", => "/c"

# 分析
思路比较简单，遇到..就回到上一级，遇到.或者空就不处理。
我们使用一个队列来处理，同时将三个需要特殊处理的字符存到一个set里面

# 代码
```
public class Solution {
    /**
     * @param path the original path
     * @return the simplified path
     */
    public String simplifyPath(String path) {
        Deque<String> queue = new LinkedList<String>();
        Set<String> skip = new HashSet<String>(Arrays.asList("..",".",""));
        
        for(String dir : path.split("/")) {
        	if(dir.equals("..") && !queue.isEmpty()) 
        		queue.pop();
        	else if(!skip.contains(dir))
        		queue.push(dir);
        }
        
        String res = "";
        for(String dir : queue)
        	res = "/" + dir + res;
        
        return res.isEmpty() ? "/" : res;
    }
}
```
