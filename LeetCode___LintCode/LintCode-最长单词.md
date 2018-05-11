# 题目
给一个词典，找出其中所有最长的单词。

**样例**
在词典
{ "dog", "google", "facebook", "internationalization", "blabla"}中, 最长的单词集合为 ["internationalization"]

在词典
{ "like", "love", "hate", "yes"}中，最长的单词集合为 ["like", "love", "hate"]

# 分析
这里有两种方法，最简单的就是先遍历一遍，找到最大的，再遍历一遍，找到最大的长度的所有单词
第二种方法就是再遍历的同时记录最大的和最大长度的所有单词

# 代码
```
class Solution {
    /**
     * @param dictionary: an array of strings
     * @return: an arraylist of strings
     */
    ArrayList<String> longestWords(String[] dictionary) {
        // write your code here
        ArrayList<String> res = new ArrayList<>();
    	
    	int len = 0;
    	
    	for(int i=0;i<dictionary.length;i++)
    	{
    		if(dictionary[i].length() >= len)
    		{
    			if(dictionary[i].length() > len)
    			{
    				res.clear();
    				len = dictionary[i].length();
    			}
    			res.add(dictionary[i]);
    		}
    	}
    	
    	return res;
    }
};
```

```
class Solution {
    /**
     * @param dictionary: an array of strings
     * @return: an arraylist of strings
     */
    ArrayList<String> longestWords(String[] dictionary) {
        // write your code here
        int maxLen = 0;
        ArrayList<String> ans = new ArrayList<String>();
        for (int i=0; i<dictionary.length; ++i) 
            if (dictionary[i].length()>maxLen) maxLen = dictionary[i].length();
        for (int i=0; i<dictionary.length; ++i)
            if (dictionary[i].length()==maxLen) ans.add(dictionary[i]);
        return ans;
    }
};
```
