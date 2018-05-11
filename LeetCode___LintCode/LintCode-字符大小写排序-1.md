# 题目
给定一个只包含字母的字符串，按照先小写字母后大写字母的顺序进行排序。

 注意事项

小写字母或者大写字母他们之间不一定要保持在原始字符串中的相对位置。

样例
给出"abAcD"，一个可能的答案为"acbAD"

# 分析
简单的两根指针，一头一尾

# 代码
```
public class Solution {
    /** 
     *@param chars: The letter array you should sort by Case
     *@return: void
     */
    public void sortLetters(char[] chars) {
        int i = 0, j = chars.length - 1;
		char tmp ;
		while ( i <= j) {
			while (i <= j && Character.isLowerCase(chars[i]) ) i++;
			while (i <= j && Character.isUpperCase(chars[j]) ) j--;
			if (i <= j) {
				tmp = chars[i];
				chars[i] = chars[j];
				chars[j] = tmp;
				i++; j--;
			}
		}
		return ;
    }
}
```
