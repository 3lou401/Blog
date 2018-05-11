 > 来源：2017年校招全国统一模拟笔试(第五场)编程题集合

时间限制：1秒
空间限制：32768K

> 牛牛从生物科研工作者那里获得一段字符串数据s,牛牛需要帮助科研工作者从中找出最长的DNA序列。DNA序列指的是序列中只包括'A','T','C','G'。牛牛觉得这个问题太简单了,就把问题交给你来解决。
例如: s = "ABCBOATER"中包含最长的DNA片段是"AT",所以最长的长度是2。 
输入描述:
输入包括一个字符串s,字符串长度length(1 ≤ length ≤ 50),字符串中只包括大写字母('A'~'Z')。


输出描述:
输出一个整数,表示最长的DNA片段

输入例子1:
ABCBOATER

输出例子1:
2

# 分析
简单题。思路就是直接暴力搜索。从第一个字母开始判断，找到最长的，再从第二个字母开始。

# 代码
```
import java.util.*;

public class Main {

	
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		
		String s = in.nextLine();
		
		in.close();
		
		System.out.println(helper(s));
	}
	
	
	private static int helper(String s) {
		
		String source = "ATGC";
		
		int res = 0;
		
		for(int i=0;i<s.length();i++) {
			
			
			if(source.indexOf(s.charAt(i) + "") != -1) {
				int index = i;
				index++;
				while(index < s.length() && source.indexOf(s.charAt(index) + "") != -1) {
					index++;
				}
				if((index - i) > res)
					res = index-i;
			}
			
			
		}
		
		return res;
		
	}
}

```
