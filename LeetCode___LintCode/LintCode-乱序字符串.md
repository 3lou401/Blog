# 题目
给出一个字符串数组S，找到其中所有的乱序字符串(Anagram)。如果一个字符串是乱序字符串，那么他存在一个字母集合相同，但顺序不同的字符串也在S中。

 注意事项

所有的字符串都只包含小写字母

样例
对于字符串数组 ["lint","intl","inlt","code"]

返回 ["lint","inlt","intl"]

# 分析
通过hash的思想，我们就是要计算出一个字符串出现的字符以及每个字符出现的次数，如果一样，则说明，两个字符就是Anagram。我们可以写一个hash函数，将每个字符串转换成字母加数字的形式。
比如，lintt,的hash就是i1l1n1t2，这样就可以判断两个字符是不是Anagram。
再利用hashmap的特性，我们很容易实现这个算法，具体看代码

# 代码
```
public class Solution {
    /**
     * @param strs: A list of strings
     * @return: A list of strings
     */
  public ArrayList<String> anagrams(String[] strs) {
		HashMap<String,ArrayList<String>> hash = new HashMap<>();
		
		for(String str : strs) {
			// create unique label for each string
			String key = generalLabel(str);
			// map the label to a list of anagrams
			if(!hash.containsKey(key)) {
				ArrayList<String> temp = new ArrayList<>(); 
				hash.put(key, temp);
				temp.add(str);
			}
			else {
				hash.get(key).add(str);
			}
		}
		
		ArrayList<String> res = new ArrayList<>();
		
		for(ArrayList<String> anagram : hash.values()) {
			 // ignore strings without anagrams
			if(anagram.size() > 1)
				res.addAll(anagram);
		}
		
		return res;
	}
	/*  
	 * create a unique label for a string  
	 * "cat", "atc" => a1c1t1  
	 */  
	private String generalLabel(String str) {
		int[] hash = new int[26];
		for(int i=0;i<str.length();i++) {
			int idx = (int)(str.charAt(i)-'a');
			hash[idx]++;
		}
		
		StringBuilder sb = new StringBuilder();
		
		for(int i=0;i<26;i++) {
			if(hash[i] == 0)
				continue;
			char c = (char)('a' + i);
			sb.append(c);
			sb.append(hash[i]);
		}
		return sb.toString();
	}
}
```
