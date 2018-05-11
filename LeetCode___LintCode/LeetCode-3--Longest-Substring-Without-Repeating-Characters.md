# 题目
给定一个字符串，请找出其中无重复字符的最长子字符串。

样例
例如，在"abcabcbb"中，其无重复字符的最长子字符串是"abc"，其长度为 3。

对于，"bbbbb"，其无重复字符的最长子字符串为"b"，长度为1。

# 分析

## 思路1：
暴力求解（超时）
最自然的想法就是我们考虑所有子串的情况，然后判断当前的子串包不包含重复的字符，并且更新最大长度。
所以我们在此主要实现两个部分
* 求出所有子串
* 判断字符串有没有重复元素

下面我们就来具体实现：
* 求出所有子串，思路很简单，两根指针，i从0到n，j从i+1到n就可以列举出所有子串
* 判断子串有没有重复字符，显然用一个set可以实现

代码：
```
package TwoPointer;

import java.util.HashSet;
import java.util.Set;

public class LongestSubstringWithoutRepeatingCharacters {
	//brute
	public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int res = 0;
        for(int i=0;i<n;i++)
        	for(int j=i+1;j<n;j++)
        		if(noDupicate(s,i,j))
        			res = Math.max(res, j-i+1);
        return res;
    }

	private boolean noDupicate(String s, int left, int right) {
		Set<Character> set = new HashSet<>();
		
		for(int i=left;i<=right;i++) {
			if(set.contains(s.charAt(i)))
				return false;
			else {
				set.add(s.charAt(i));
			}
		}
		return true;
	}
}
```

## 思路二：
滑动窗口法

滑动窗口方法是一个抽象的概念，广泛应用于数组和String的问题上，一个窗口就是包含了一系列的元素，然后移动窗口两边的元素。所以叫滑动窗口法。
简单的直接暴力求解非常容易理解，但是效率太低，我们需要想出更好的解法
在暴力求解中，我们做了很多重复判断是否有子串的工作，这些都是没有必要的。
比如，如果我们已经知道i到j-1是不含重复子串的，那么当我们要判断，i到j是否含重复子串时，只需要判断j是不是在i到j-1中就可以了。这里我们就可以用一个hashset来进行判断。

我们用hashset来存储当前窗口所包括的字符，然后我们向右滑动，如果当前字符不再hashset中，我们就可以继续滑动并且更新最大长度，如果在窗口中，我们就将窗口的左边界右移，继续循环判断，这样只要遍历一次就可以了。
代码：
```
//slide windows
	public int lengthOfLongestSubstring2(String s) {
        int n = s.length();
        Set<Character> set = new HashSet<>();
        int j = 0;
        int res = 0;
        for(int i=0;i<n;i++) {
        	while(!set.contains(s.charAt(j)) && j<n) {
        		set.add(s.charAt(j));
        		res = Math.max(res, j-i+1);
        		j++;
        	}
        	set.remove(s.charAt(i));
        }
        return res;
    }
```

## 思路三：
滑动窗口的优化：
上一中方方法中，当我们发现重复元素时，我们是移动i也就是左窗口的边界，一步步移动，其实这样是可以优化的，我们实际只需要从出现重复元素的位置的后一个开始移动，因为前面那些元素是小于之前的长度的。
举个例子，假设（i，j）我们发现j元素和j‘元素重复，那么i不需要一步一步右移，而是可以直接移动到j'+1的位置，所以我们需要记录重复元素的下标，这时候我们可以使用hashmap。
代码
```
public int lengthOfLongestSubstring(String s) {
        if (s.length()==0) return 0;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int max=0;
        for (int i=0, j=0; i<s.length(); ++i){
            if (map.containsKey(s.charAt(i))){
                j = Math.max(j,map.get(s.charAt(i))+1);
            }
            map.put(s.charAt(i),i);
            max = Math.max(max,i-j+1);
        }
        return max;
    }
```

## 进一步优化
我们可以用数组进一步优化，用一个int数组记录出现的元素
* int[26] for Letters 'a' - 'z' or 'A' - 'Z'
* int[128] for ASCII
* int[256] for Extended ASCII

```
public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        int[] index = new int[128]; // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            i = Math.max(index[s.charAt(j)], i);
            ans = Math.max(ans, j - i + 1);
            index[s.charAt(j)] = j + 1;
        }
        return ans;
    }
```
